---
title: Laboratory for Web Algorihmics数据集格式转换
date: 2022-06-29 15:43:17
tags:
- LWA
- Dataset
categories:
- [Graph]

---





# [Laboratory for Web Algorihmics](http://law.di.unimi.it/datasets.php)数据集格式转换

注：提前安装好java环境

以下案例在linux上配置

## [step1：配置WebGraph](http://webgraph.di.unimi.it/#install)

在项目根目录下载WebGraph和相应依赖包：

```
wget http://search.maven.org/remotecontent?filepath=it/unimi/dsi/webgraph/3.5.2/webgraph-3.5.2.jar
wget http://webgraph.di.unimi.it/webgraph-deps.tar.gz
```

如果以上下载不成功，可以去下面的网盘下载

```
链接：https://pan.baidu.com/s/1SsHF6fqWmVX_LHcod9tkUw 
提取码：h0w5
```

在下载的同一目录下新建lib文件夹，将解压后的WebGraph和WebGraph-deps的jar包放在lib下：

```
mkdir lib
cd lib
cp 
```

在项目根目录下测试是否安装成功：

```
java -cp "lib/*" it.unimi.dsi.webgraph.ArcListASCIIGraph --help
```

## step2：从LWA下载数据集

下载.graph和.properties到项目根

```
wget http://data.law.di.unimi.it/webdata/clueweb12/clueweb12.graph
wget http://data.law.di.unimi.it/webdata/clueweb12/clueweb12.properties
```

## [step3：解压成邻接表格式](http://glatue.com/2017/04/06/webgraph-配置数据集导出（ascii格式）/)

在项目根中运行

```
java -cp "lib/*" it.unimi.dsi.webgraph.ASCIIGraph clueweb12 clueweb12
```

得到的clueweb12-graph.txt即为ASCII码格式的邻接表文件

## [step4：解压成边表格式](http://glatue.com/2017/04/06/webgraph-配置数据集导出（ascii格式）/)

在项目根中运行，这一步得到的结果会非常大，注意内存大小

```
java -cp "lib/*" it.unimi.dsi.webgraph.ArcListASCIIGraph clueweb12 clueweb12-edgelist.txt
```

得到的clueweb12-edgelist.txt即为ASCII码格式的边表文件

## [step5：转成二进制格式（压缩）](http://glatue.com/2017/04/06/asciidataset_to_gemini/)

将convert2binary.cpp复制到项目根下，运行：

```
g++ convert2binary.cpp -o convert2binary
./convert2binary clueweb12-edgelist.txt clueweb12.bin
```

convert2binary.cpp

```c++
#include <iostream>
#include <vector>
#include <string>
#include <fstream>

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/mman.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
#include <fcntl.h>
#include <string.h>

using namespace std;

typedef int ID;
typedef unsigned long long offset_t;

const offset_t BUFF_SIZE = 0x4000; //16K

enum Status{
    OK = 1,
    ERROR = 0
};

class OutFile 
{
protected:
    int fos;
    offset_t wp;
    char buffer[BUFF_SIZE];
public:
    string dir, name, dir_name;

    OutFile(string name, string dir):
        dir(dir), name(name), dir_name(dir + "/" + name),wp(0){
        fos = open((dir_name).c_str(), 
                O_WRONLY | O_CREAT | O_TRUNC, 
                S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH);
    }

    Status write(const char* buff, size_t len)
    {
        //Fill the buffer
        if(wp + len > BUFF_SIZE)
        {
            offset_t remain = BUFF_SIZE - wp;
            memcpy(buffer + wp, buff, remain);
            ::write(fos, buffer, BUFF_SIZE);
            wp = 0;
            len -= remain;
            buff += remain;

            //write big chunks if any   
            if(len > BUFF_SIZE)
            {
                remain = len - (len / BUFF_SIZE) * BUFF_SIZE;
                ::write(fos, buff, len - remain);
                buff += (len - remain);
                len = remain;
            }
        }
        memcpy(buffer, buff, len);
        wp += len;
        return OK;
    }

    Status flush()
    {
        if(::write(fos, buffer, wp) == -1)
            return ERROR;
        wp = 0;
        return OK;
    }

    Status close()
    {
        Status s = flush();
        if(s != OK) return s;
        if(::close(fos) == -1)
        {
            cout << "close OutFile " << dir_name << " failed." << endl;
            return ERROR;
        }
        return OK;
    }

    template<class T>
    Status write_unit(T unit)
    {
        for(int i = 0; i < sizeof(T); i++)
        {
            if(wp >= BUFF_SIZE)
            {   
                ::write(fos, buffer, wp);
                wp = 0;
            }
            char c = static_cast<unsigned char> (unit & 0xff);
            buffer[wp++] = c;
            unit >>= 8;
        }
        return OK;
    }

};


void convert(char *fname, char *to_fname)
{
    FILE *f = fopen(fname, "r");
    OutFile binary_file(string(to_fname), string("."));

    //Start read dataset
    cout << "Start read dataset " << endl;
    ID from;
    long long maxlen = 10000000;
    char *s = (char*)malloc(maxlen);
    char delims[] = " \t\n";

    long long edge_cnt = 0;
    ID max_id = 0;

    while (fgets(s, maxlen, f) != NULL)
    {
        if (s[0] == '#' || s[0] == '%' || s[0] == '\n')
            continue; //Comment

        char *t = strtok(s, delims);
        from = atoi(t);
        max_id = max(max_id, from);

        ID to;
        while ((t = strtok(NULL, delims)) != NULL)
        {
            to = atoi(t);
            max_id = max(max_id, to);

            binary_file.write_unit(from);
            binary_file.write_unit(to);

            edge_cnt++;    
        }
        /*
        if (from % 100000 == 0)
            cout << from << endl;
        //*/
        ///*
        if (edge_cnt % 1000000 == 0)
        cout << "Read edge num: " << edge_cnt << endl;
        //*/
    }

    binary_file.close();

    fclose(f);
    free(s);
    cout << "Edge number: " << edge_cnt << endl;    
    cout << "End read dataset max id: " << max_id << "vertices num: " max_id + 1 << endl;
}


int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        cout << "Usage ./convert2binary <dataset.txt> <binary edgelist file name>" << endl;
        return 0;
    }

    convert(argv[1], argv[2]);

    return 0;
}
```