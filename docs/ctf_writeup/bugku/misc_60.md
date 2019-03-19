## 又一张图片，还单纯吗
[http://123.206.87.240:8002/misc/2.jpg](http://123.206.87.240:8002/misc/2.jpg)

好像和上一个有点不一样

存档：[2.jpg](./problems/2.jpg)

### 解决方案
binwalk发现内含两张图片，foremost提取一下：

![misc_60-1.jpg](./img/misc_60-1.jpg)

真够长的：

    falg{NSCTF_e6532a34928a3d1dadd0b049d5a3cc57}

## 猜
[http://123.206.87.240:8002/misc/cai/QQ20170221-132626.png](http://123.206.87.240:8002/misc/cai/QQ20170221-132626.png)

flag格式key{某人名字全拼}

存档：[QQ20170221-132626.png](./problems/QQ20170221-132626.png)

### 解决方案
搞了半天啥用都没有，结果发现大家都是丢到识图里面？

    key{liuyifei}

服了...

## 宽带信息泄露
flag格式：

flag{宽带用户名}

[conf.bin](https://ctf.bugku.com/files/5986768ca8b96cead45aec16a88431b5/conf.bin)

存档：[conf.bin](./problems/conf.bin)

### 解决方案
上RouterPassView就可以看到了，但是似乎没找到源码...挺好奇原理...

![misc_60-2.png](./img/misc_60-2.png)

## 隐写2
[Welcome_.jpg](https://ctf.bugku.com/files/af49803469dfdabb80acf562f9381335/Welcome_.jpg)

存档：[Welcome_,jpg](./problems/Welcome_.jpg)

### 解决方案
下载到的文件，属性里似乎有提示，

![misc_60-3.png](./img/misc_60-3.png)

完全没看懂，直接binwalk看看。内含zip文件，foremost提取，解压zip后有flag.rar和提示.jpg。好吧提示也没看懂，我就看懂了密码是3位数字，那就直接爆破好了。

生成一下三位数字的字典：
``` cpp
#include <iostream>
#include <fstream>
#include <iomanip>

int main() {
    std::ofstream outputFileStream("./dict_3numbers.txt");
    for (size_t i = 0; i < 1000; ++i) {
        outputFileStream << std::setw(3) << std::setfill('0') << i << std::endl;
    }
    outputFileStream.close();

    return 0;
}

```
然后：

    fcrackzip -b -D -p ./dict_3numbers.txt ./flag.rar

或者直接：

    fcrackzip -b -c "1" -l 3 ./flag.rar

可以得到：

    possible pw found: 035 ()
    possible pw found: 337 ()
    possible pw found: 728 ()
    possible pw found: 871 ()
    
871解开压缩包，有个3.jpg，没啥信息，试试：

    strings ./3.jpg
    
末尾有东西，提交了不对：

    f1@g{eTB1IEFyZSBhIGhAY2tlciE=}
    
BASE64解开：

    f1@g{y0u Are a h@cker!}

## 多种方法解决
在做题过程中你会得到一个二维码图片

[http://123.206.87.240:8002/misc/3.zip](http://123.206.87.240:8002/misc/3.zip)

存档：[3.zip](./problems/3.zip)

### 解决方案
直接：

    strings ./KEY.exe

得到了一串BASE64的图片：

    data:image/jpg;base64,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

解开得到二维码：

![misc_60-4.jpg](./img/misc_60-4.jpg)

解码：

    KEY{dca57f966e4e4e31fd5b15417da63269}
    
## 闪的好快
有空补代码

## come_game
有空补上

## 白哥的鸽子
咕咕咕

[jpg](https://ctf.bugku.com/files/57c79d5b04e18a4bf8995d2721d76d5c/jpg)

存档：[jpg](./problems/jpg)
### 解决方案
废话少说，上各种套路：

    strings jpg

末尾可疑：

    fg2ivyo}l{2s3_o@aw__rcl@

看起来像是被打乱了顺序，栅栏密码每组3个，毕竟研表究明，汉字序顺并不定一影阅响读：

    flag{w22_is_v3ry_cool}@@
    
提交竟然挂了...删掉@@就OK