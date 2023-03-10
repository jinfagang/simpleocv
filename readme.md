# Simple OpenCV

> 大部分情况下，你并不需要OpenCV。

该仓库主要是在一些移动场景下，替代OpenCV的功能，例如一些图片的读写、预处理、resize、gui等被替换，并且大部分情况下你可以把SimpleOCV直接集成到你的项目中，甚至可以直接编译成wasm在浏览器运行。

SimpleOCV大部分实现均来自于ncnn，为了使得它更加模块化，单独拎出来作为一个独立库，方便任何项目进行集成。

理论上我们也可以使用opencv-mobile的版本，但这个东西是在原有的opencv源码上patch出来的，灵活性太差了，直接下载预先编译好的二进制也会出很多问题。

SimpleOCV尽可能秉承以下几个原则：

- 最小化：精简你今需要的几个接口；
- 跨平台：浏览器都能跑，还有什么不能跑；
- 方便集成：任何C++项目都能集成

目的就是：对于不是很复杂的项目，彻底抛弃OpenCV，但是对于已有的项目，SimpleOCV的include接口，保持和OpenCV一模一样。

## 与其他库的对比


| 平台          | OpenCV      | FlyCV            | opencv mobile | simpleocv         |
| :-------------- | ------------- | :----------------- | :-------------- | ------------------- |
| Windows       | ✔️        | ❌               | ✔️          | ✔️              |
| Windows(MSVC) | ✔️        | ❌               | ✔️          | ✔️              |
| macOS         | ✔️        | ✔️             | ✔️          | ✔️              |
| Linux         | ✔️        | ✔️             | ✔️          | ✔️              |
| Webassembly   | -           | -                | -             | ✔️              |
| Speed         | Fast        | FastMore         | Fast          | FastMore          |
| Strength      | Compelete   | Compelete & Fast | Light         | **Simple & Easy** |
| Simplicity    | Complicated | Normal           | Complicated   | **Easiest**       |
| Dpendencies   | Many        | Many             | Less          | **Zero**          |

## 交流群

新建几个simpleocv的QQ交流群，欢迎加入交流，群里不定期推送微信交流群二维码：

- 点击链接加入群聊【奇异AI-轻量视觉交流群】：https://jq.qq.com/?_wv=1027&k=0MaclEYg

## 更新

- *`2022.12.26`*: 我增加了windows下的编译支持，现在可以直接编译一个windows .lib 静态库了，MSVC没有问题，这样你在大部分软件里面都可以直接集成，比opencv编译容易很多；

## 用法

**你不需要opencv，只需要这样：**

```c++
#include "simpleocv.h"

int main(int argc, char **argv) {

  std::string img_f = argv[1];

  cv::Mat a = cv::imread(img_f);
  cv::putText(a, "28.9 C from SimpleOCV", cv::Point(20, 45), 1, 0.5,
              cv::Scalar(255, 0, 255));
  cv::imwrite("a_gray.png", a);
}
```

然后，你就有了一个和opencv一模一样的能力。

上面你就可以看到这样一个可视化的图：

![](https://raw.githubusercontent.com/jinfagang/public_images/master/20221221165207.png)

请注意！这里面没有用到任何opencv的代码。

这是一个实际调用simpleocv的上层应用的效果；

![图片.png](https://s2.loli.net/2022/12/22/sMY7iRP4mJGNQKC.png)

## 高端用法

SimpleOCV 最有用的还是集成到你的项目里，你可以把simpleocv作为一个3rd依赖，也可以手动的把编译出来的`libsimpleocv.a` 拷贝到你的链接目录，然后带上一个单一的头文件 `simpleocv.h` 就行了。

## 支持平台

`simpleocv` 其实目的就是做一个minimal替代opencv的东西，让你在任何终端都可以无痛使用opencv，也不需要更改你的应用c++代码，同时还能保持一定的opencv优化能力，例如图像resize等的速度。目前支持的平台：

- macOS测试没有问题，x86, arm库都能编译；
- windows下MSVC可以编译（但是ncnn版本会有些许的问题）；
- wasm，webassembly端可以集成没有问题。

## 贡献

欢迎老铁们PR一些你用simpleocv实现的东西，例如画图，画box、画keypoints等等，然后写到 `examples/demo_xx.cc` 给我PR。感谢你。

## 编译

```
mkdir build
cd build
cmake ..
make -j8

./examples/demo_color bus.jpg
```

## 增设

相较于ncnn里面的版本，我做了些许的修改，记录如下：

- 增加了`CV_PI, CV_F64C1` 等全局宏定义；
- 增加了 `cv::Point` 对于 `-, ==, -=, !=`等操作符的复写；
- 增加了 `cv::Mat, cv::Scalar` 等更贴近opencv的构造函数；
- 将simpleocv的功能进行了约简，不依赖于ncnn，一个头文件就可以调用；
- 增加了 `cv::Mat::zeros` 等初始化空白Mat函数；
- 增加了 `cv::LINE_AA` 等宏；

## 计划

有些许的函数，其实可以添加进来的，这样可以让这个微型的替代版本更加鲁棒，感兴趣的可以PR：

- [ ] `cv::copyMarkBorder` 函数引入；
- [ ] `cv::polyLines` 函数引入；
- [ ] 将默认的resize bilinear切换到resize nearest；
- [ ] 引入一些flycv的优化；

## 后续

本项目旨在让你不依赖OpenCV，拥有基础的opencv能力，后续我会持续精简相关代码。并且尝试编译到其他平台，例如web，iOS等，这些平台你可以用SimploeOCV来做预处理，但是已经不需要opencv的依赖了。

## Copyright

lucasjin && ncnn reserved.
