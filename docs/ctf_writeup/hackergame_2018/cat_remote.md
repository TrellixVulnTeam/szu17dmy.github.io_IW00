# 猫咪遥控器

## 题目
SOURCE: [https://hack2018.lug.ustc.edu.cn/#catremote](https://hack2018.lug.ustc.edu.cn/#catremote)

提示：flag 格式为 flag{......}，只包含字母，其中有且只有两个为大写字母。
今天的 App Store 首页故事是《猫咪占领世界》(详情)。

SERIOUSLY?

D 同学不禁开始幻想被猫咪占领的世界：集中营里成群的铲屎官，密密麻麻的 Nepeta cataria（Wikipedia）农田，随意摆放的纸箱子占满了道路……

想想就可怕，不过 D 同学知道人类还有终极秘密武器可以用——猫咪遥控器，有了猫咪遥控器，再多的猫咪也只会乖乖地听人类的话，哈哈哈哈哈～

下面是制作猫咪遥控器的技术总结，需要的原料有：

+ 5mW 6mm 点状激光二极管一个；
+ 锂电池一个；
+ 导线若干；

然后用导线将锂电池和激光二极管连接起来（这一步的目的是让二极管亮起来，不想二极管亮起来的同学可以不连），一个美味的猫咪遥控器就做好了。

猫咪遥控器的原理非常简单！撸猫学会曾经有论文给出过结论：激光笔指向哪里，猫咪就会跑到哪里。

为了报复猫咪把自己的代码打乱（见：猫咪与键盘），D 同学把猫咪遥控器绑在可以上（UP）下（DOWN）左（LEFT）右（RIGHT）移动的三轴机械臂上，开始使用树莓派（一款基于 Linux 的单片机计算机）控制三轴机械臂，进而控制猫咪在草地上跑来跑去。

附件是树莓派上留下的调试输出信息，我们赶到现场时只剩下这个了。

## 解决方案
打开文档发现为四个方向的字母，猜想画出该路线即可获得flag。很多工具可以实现这一功能，这里使用OpenCV辅助画图，作图后即可发现flag。

其中，变量map构造函数参数是画布的大小等属性，变量row和col是起始的位置坐标。其次，switch语句中的双层循环是为了让线条更粗一些，延时则是为了展示动画效果。

``` cpp
#include <iostream>
#include <fstream>
#include <opencv2/opencv.hpp>

int main() {
    cv::Mat map(256, 768, CV_8UC1);
    std::ifstream inputFileStream("seq.txt");
    int row = 50, col = 128;
    map.at<uchar>(row, col) = 255;
    std::string line;
    inputFileStream >> line;

    for (auto i : line) {
        switch (i) {
            case 'U':
                row -= 1;
                for (int ir = -1; ir < 1; ++ir) {
                    for (int ic = -1; ic < 1; ++ic) {
                        map.at<uchar>(row + ir, col + ic) = 255;
                    }
                }
                break;
            case 'D':
                row += 1;
                for (int ir = -1; ir < 1; ++ir) {
                    for (int ic = -1; ic < 1; ++ic) {
                        map.at<uchar>(row + ir, col + ic) = 255;
                    }
                }
                break;
            case 'L':
                col -= 1;
                for (int ir = -1; ir < 1; ++ir) {
                    for (int ic = -1; ic < 1; ++ic) {
                        map.at<uchar>(row + ir, col + ic) = 255;
                    }
                }
                break;
            case 'R':
                col += 1;
                for (int ir = -1; ir < 1; ++ir) {
                    for (int ic = -1; ic < 1; ++ic) {
                        map.at<uchar>(row + ir, col + ic) = 255;
                    }
                }
                break;
            default:;
        }
        cv::waitKey(1);
        cv::imshow("MAP", map);
    }
    cv::imshow("MAP", map);

    cv::waitKey(0);
    return 0;
}

```
![CAT-REMOTE](./img/CAT-REMOTE.png)
