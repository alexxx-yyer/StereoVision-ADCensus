# StereoVision-ADCensus 立体视觉项目

这是西威斯特法伦应用科学大学机器人实验室的硕士项目。
通过这个仓库中的程序，你可以标定单目相机、双目系统，计算视差图并评估数据。这是一个完整的立体视觉解决方案包！软件运行在Windows和Linux系统上，使用C++编写。

## 🚀 OpenCV 4.x 兼容性更新

**此分支已更新以支持现代OpenCV 4.x版本！**

原始项目是为OpenCV 2.x设计的，但这个版本包含了全面的更新以确保与OpenCV 4.x的兼容性：

- ✅ 所有已弃用的`CV_`常量已更新
- ✅ 头文件包含已现代化，使用`opencv2/opencv.hpp`
- ✅ CMakeLists.txt文件已更新，移除了版本限制
- ✅ OpenCVBM模块的Qt4已升级到Qt5
- ✅ 所有模块已测试并验证可成功编译

### 快速开始
```bash
# 克隆此仓库
git clone https://github.com/alexxx-yyer/StereoVision-ADCensus.git
cd StereoVision-ADCensus

# 切换到OpenCV 4.x兼容分支
git checkout opencv4.x-linux

# 构建任意模块（示例：IntrinsicExtrinsicCalib）
cd IntrinsicExtrinsicCalib
mkdir build && cd build
cmake .. && make
```

### 分支说明
- `master` - 原始OpenCV 2.x版本
- `opencv4.x-linux` - **OpenCV 4.x兼容版本（推荐）**

查看项目运行效果或其他优秀项目：[我的YouTube频道](https://www.youtube.com/user/dluensch/videos), [Robalab W-HS YouTube频道](https://www.youtube.com/user/RoblabFhGe/videos)

[![YouTube项目视频](http://img.youtube.com/vi/MZsSTpS-XGI/0.jpg)](http://www.youtube.com/watch?v=MZsSTpS-XGI)

基于以下库：
* OpenCV 4.x（从2.x更新）
* BOOST
* libConfig
* PCL
* OpenMP
* Qt 5.x（从4.8.X更新）

项目包中的程序：
* IntrinsicExtrinsicCalib - 相机内参外参标定
* ImageRectify - 图像校正
* ADCensusBM - ADCensus立体匹配算法
* PictureOverlay - 图像叠加工具
* OpenCVBM - OpenCV块匹配比较工具
* evalDisp - 视差评估

## 📋 OpenCV 4.x 升级详情

### 更新内容
此分支包含全面的更新以使项目与OpenCV 4.x兼容：

#### 1. **已弃用常量更新**
- `CV_BGR2GRAY` → `COLOR_BGR2GRAY`
- `CV_TERMCRIT_EPS` → `TermCriteria::EPS`
- `CV_TERMCRIT_ITER` → `TermCriteria::MAX_ITER`
- `CV_CALIB_*` → `CALIB_*`
- `CV_L2` → `NORM_L2`
- `CV_RGB()` → `Scalar()`
- `CV_INTER_LINEAR` → `INTER_LINEAR`
- `CV_LOAD_IMAGE_GRAYSCALE` → `IMREAD_GRAYSCALE`

#### 2. **头文件包含现代化**
- 将多个特定的OpenCV头文件替换为`#include <opencv2/opencv.hpp>`
- 这确保了与OpenCV 4.x模块结构的兼容性

#### 3. **CMakeLists.txt更新**
- 移除了OpenCV版本限制（`find_package(OpenCV 2.4 REQUIRED)` → `find_package(OpenCV REQUIRED)`）
- 将OpenCVBM模块的Qt4更新为Qt5

#### 4. **Qt兼容性**
- 更新`Qt::WFlags` → `Qt::WindowFlags`
- 更改`QtGui/QMainWindow` → `QtWidgets/QMainWindow`
- 现代化Qt5 CMake配置

### 系统要求
- **OpenCV 4.x**（测试版本4.6.0）
- **Qt 5.x**（用于OpenCVBM模块）
- **CMake 3.5+**
- **C++11兼容编译器**
- **Linux/Windows**（在Ubuntu 24.04上测试）

### 构建状态
所有模块已测试并成功编译：
- ✅ IntrinsicExtrinsicCalib
- ✅ ADCensusBM  
- ✅ ImageRectify
- ✅ evalDisp
- ✅ PictureOverlay
- ✅ OpenCVBM

# IntrinsicExtrinsicCalib 相机标定
使用此程序可以计算相机的内参和外参。标定需要多张带有棋盘格图案的图像，可以是任意尺寸，例如8x6（宽x高）[图案](./Documents/chessboards/Chessboard_A4_8x6_2.65cm.pdf)。我们在项目中使用了这个[18x12](./Documents/chessboards/Chessboard_A0_18x12_5cm.pdf)（仅计算内边缘）图案，打印在A0纸上。每次标定都基于约20张来自不同视角、距离和角度的图像。重要的是图案在相机传感器的每个部分都可见！对于每个单目相机和标定步骤（内参和外参），你必须拍摄这些图像。

### 在Linux系统上构建和执行

**构建程序：**
 1. 进入"IntrinsicExtrinsicCalib"文件夹。
 2. `mkdir build`
 3. `cd build`
 4. `cmake ..`
 5. `make`
 6. 如果成功构建程序，你应该能看到"intrinsic"和"extrinsic"可执行文件。

**执行内参标定：**
 1.  创建"intrinsicConfig.cfg"文件，如示例文件夹中所示。（例如：configIntrinsicLeft.cfg, configIntrinsicRight.cfg）
 2.  创建"intrinsicImages.xml"文件，如示例文件夹中所示。（例如：imagesLeftIntrinsic.xml, imagesRightIntrinsic.xml）
 3.  执行内参标定：`./intrinsic PATH_TO_YOUR_CONFIG/intrinsicConfig.cfg`
 4.  这可能需要几分钟。检查目标文件夹中的标定结果！
  
**执行外参标定：** 
 1.  创建"extrinsicConfig.cfg"文件，如示例文件夹中所示。（例如：configExtrinsic.cfg）
 2.  创建"extrinsicImages.xml"文件，如示例文件夹中所示。（例如：imagesExtrinsic.xml）
 3.  执行外参标定：`./extrinsic PATH_TO_YOUR_CONFIG/extrinsicConfig.cfg`
 4.  这可能需要几分钟。检查目标文件夹中的标定结果！
 5.  检查结果4x4矩阵中的平移向量。这应该与所选测量单位中相机之间的距离匹配。

# ImageRectify 图像校正
使用此程序可以轻松校正使用已标定双目相机系统拍摄的新图像。你需要每个相机的内参标定和双目系统的外参标定。所有标定都可以使用"IntrinsicExtrinsicCalib"进行估计。

### 在Linux系统上构建和执行

**构建程序：**
 1. 进入"ImageRectify"文件夹。
 2. `mkdir build`
 3. `cd build`
 4. `cmake ..`
 5. `make`
 6. 如果成功构建程序，你应该能看到"ImageRectify"可执行文件。

**执行图像校正：**
 1.  创建"config.cfg"文件，如示例文件夹中所示。
 2.  创建"images.xml"文件，如示例文件夹中所示。
 4.  执行图像校正：`./ImageRectify PATH_TO_YOUR_CONFIG/config.cfg`

# ADCensusBM ADCensus立体匹配
此程序使用ADCensus算法为给定的校正图像集创建视差图。你可以在"Documents"部分找到论文或[这里](./Documents/ADCensus.pdf)。当前实现不使用你的显卡进行并行化（算法为此而设计），而是使用OpenMP。通过使用OpenCV，应该很容易移植到显卡上。

如果你有图像的外参标定文件，程序会创建点云。否则创建一个空的外参文件并更改配置中的特定参数，如示例文件夹中所示。如果不存在外参，只会创建深度图。无论如何，输入图像必须是校正的！

你可以在这里看到算法的结果和评估：http://vision.middlebury.edu/stereo/eval/

**构建程序：**
 1. 进入"ADCensusBM"文件夹。
 2. `mkdir build`
 3. `cd build`
 4. `cmake ..`
 5. `make`
 6. 如果成功构建程序，你应该能看到"ADCensusBM"可执行文件。

**执行图像校正：**
 1.  从示例文件夹复制并编辑"config.cfg"。"config.cfg"中的值适用于middleburry图像集。这些值也适用于其他图像，但你自己数据集的特定值可以提高质量。
 2.  创建"images.xml"文件，如示例文件夹中所示。
 3.  执行图像校正：`./ADCensusBM PATH_TO_YOUR_CONFIG/config.cfg`

# PictureOverlay 图像叠加
这是一个小工具，用于检查标定和场景中视差的准确性。程序简单地将左右相机的2张对应图像叠加并显示。之后你可以移动一张图像（第一张）并检查对象是否位于相同的极线上。进一步你可以检查对象之间的视差。

**构建程序：**
 1. 进入"PictureOverlay"文件夹
 2. `mkdir build`
 3. `cd build`
 4. `cmake ..`
 5. `make`
 6. 如果成功构建程序，你应该能看到"PictureOverlay"可执行文件

**执行PictureOverlay：**
 1. `./PictureOverlay PATH_TO_YOUR_IMAGE/image1.jpg PATH_TO_YOUR_IMAGE/image2.jpg alpha1 alpha2 scale`
  * alpha1: 第一张图像的alpha值。值必须在0-1之间。（例如：0.5）
  * alpha2: 第二张图像的alpha值。值必须在0-1之间。（例如：0.5）
  * scale: 如果图像大于屏幕分辨率，可以用此参数减小尺寸。值必须在0和1之间。如果不想减小尺寸，将值设置为1。
  * 使用"A"和"D"键可以左右移动第一张图像。
  * 使用"W"和"S"键可以上下移动第一张图像。

# OpenCVBM OpenCV块匹配
这是一个小程序，用于将ADCensus的结果与OpenCV内置的块匹配方法进行比较，包括块匹配（BM）和半全局块匹配（SGBM）。作为输入，程序需要来自双目相机系统的2张校正图像。你可以调整每个算法的所有自由参数并立即看到结果。你可以随时在2种算法之间切换来比较它们。

**构建程序：**
 1. 进入"OpenCVBM"文件夹
 2. `mkdir build`
 3. `cd build`
 4. `cmake ..`
 5. `make`
 6. 如果成功构建程序，你应该能看到"OpenCVBM"可执行文件

**执行OpenCVBM：**
 1. `./OpenCVBM PATH_TO_YOUR_IMAGE/image1.jpg PATH_TO_YOUR_IMAGE/image2.jpg scale`
  * scale: 如果图像大于屏幕分辨率，可以用此参数减小尺寸。值必须在0和1之间。如果不想减小尺寸，将值设置为1。

# evalDisp 视差评估
使用此程序你可以评估每个像素并查看该像素的视差和计算距离。你需要的只是校正图像、计算的视差图像和相机外参。

**编写配置文件：**

在使用程序之前，你需要一个配置。示例配置在"evalDisp/sample"下！

参数：
 *  cameraName: 相机名称
 *  extrinsicFile: 使用"IntrinsicExtrinsicCalib"创建的文件
 *  imagesXML: 包含图像路径的OpenCV XML文件
  * 你只需要来自相机的一张图像（左相机或右相机）！
 *  dispsXML: 包含视差图路径的OpenCV XML文件
  * 注意：你需要由ADCensus创建的.yml视差图！视差图必须是与imagesXML中图像对应的文件！参见项目中的示例！
 *  scaleFactor: 如果图像大于屏幕分辨率，可以用此参数减小尺寸。值必须在0和1之间。如果不想减小尺寸，将值设置为1。

示例配置1个相机：
```
cameras =(  
            {
                cameraName="Sample_Camera";
                extrinsicFile="PATH_TO_THE_CALCULATED_CAMERA_EXTRINSIC_FILE/CAMERA_EXTRINSIC_FILE.yml";
				imagesXML="PATH_TO_THE_IMAGE_CONFIG/IMAGE.xml";
                dispsXML="PATH_TO_THE_DISPARITY_MAPS/DISPARITY_MAPS.xml";
                scaleFactor=0.5;
            }
    };
```

多个相机的示例配置：
```
cameras =(  
            {
                cameraName="Sample_Camera_1";
                extrinsicFile="PATH_TO_THE_CALCULATED_CAMERA_EXTRINSIC_FILE/CAMERA1_EXTRINSIC_FILE.yml";
				imagesXML="PATH_TO_THE_IMAGE_CONFIG/IMAGE1.xml";
                dispsXML="PATH_TO_THE_DISPARITY_MAPS/DISPARITY1_MAPS.xml";
                scaleFactor=0.5;
            },
            {
                cameraName="Sample_Camera_2";
                extrinsicFile="PATH_TO_THE_CALCULATED_CAMERA_EXTRINSIC_FILE/CAMERA2_EXTRINSIC_FILE.yml";
				imagesXML="PATH_TO_THE_IMAGE_CONFIG/IMAGE2.xml";
                dispsXML="PATH_TO_THE_DISPARITY_MAPS/DISPARITY2_MAPS.xml";
                scaleFactor=0.5;
            }
    };
```

**构建程序：**
 1. 进入"evalDisp"文件夹
 2. `mkdir build`
 3. `cd build`
 4. `cmake ..`
 5. `make`
 6. 如果成功构建程序，你应该能看到"evalDisp"可执行文件

**执行evalDisp：**
 1. `./evalDisp PATH_TO_YOUR_CONFIG/YOUR_CONFIG.jpg`
  * 使用"A"和"D"键可以左右移动十字。
  * 使用"W"和"S"键可以上下移动十字。
  * 使用"C"可以在视差和彩色图像之间切换。
  * 使用"B"和"N"可以转到列表中的上一张/下一张图像。
  * 使用"H"和"J"可以转到上一个/下一个相机（如果存在）。
  * 使用"Q"或"ESC"可以退出程序。

## 许可证
此项目使用BSD许可证。详见LICENSE文件。

## 贡献
欢迎提交问题和拉取请求！

## 致谢
感谢原始作者Dennis Lünsch, Tom Marvin Liebelt, Christian Blesing的出色工作。