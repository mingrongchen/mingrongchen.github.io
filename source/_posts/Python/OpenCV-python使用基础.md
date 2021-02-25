---
title: Opencv-python使用基础
categories: 
  - Python
tags:
  - OpenCV-python
about: 无
date: 2021-02-25 16:29:32
---

# OpenCV-python使用基础

<!--more-->

## 基本图像处理

### 概要

plt.imread和PIL.Image.open读入的都是RGB顺序，而opencv中cv2.imread读入的是BGR通道顺序 。cv2.imread会显示图片更蓝一些。

虽然python 很强大，而且也有自己的图像处理库PIL，但是相对于OpenCV 来讲，它还是弱小很多。跟很多开源软件一样OpenCV 也提供了完善的python 接口，非常便于调用。

Python 作为一种高效简洁的直译式语言非常适合我们用来解决日常工作的问题。而且它简单易学，初学者几个小时就可以基本入门。再加上Numpy 和matplotlib 这两个翅膀，Python 对数据分析的能力不逊于Matlab。Python 还被称为是胶水语言，有很多软件都提供了Python 接口。尤其是在linux 下，可以使用Python 将不同的软件组成一个工作流，发挥每一个软件自己最大的优势从而完成一个复杂的任务。比如我们可以使用Mysql 存储数据，使用R 分析数据，使用matplotlib 展示数据，使用OpenGL 进行3D 建模，使用Qt 构建漂亮的GUI。而Python 可以将他们联合在一起构建一个强大的工作流。

### 基本操作

#### 读取显示图片

opencv支持读取bmp、jpg、png、tiff等常用格式。

```python
import cv2    #导入cv模块

img = cv2.imread("11.jpg")		#读取图片
cv2.namedWindow("Image")		#创建窗口，并命名
cv2.imshow("Image", img)		#在窗口中显示图像
cv2.waitKey(0)					#若不添加，串口会一闪而过
cv2.destroyAllWindows() 		#释放窗口
#cv2.destroyWindow(("Image")
```

#### 创建图像

opencv接口中没有直接创建图像的接口，若要创建图像，需要用到numpy函数。在新的opencv-python中，图像使用Numpy数据的属性来表示图像的尺寸和通道信息。

```python
emptyImage = np.zeros(img.shape, np.uint8) 
emptyImage2 = img.copy();

emptyImage3=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)#使用cvtColor函数获取原图像的副本
```

#### 保存图像

保存图像比较简单，直接使用cv2.imwrite即可。

```python
cv2.imwrite("12.jpg")
```

#### 图片操作

##### 图片翻转

```python
imgflip = cv2.flip(img, 1)    #0:沿x轴翻转；1：沿y轴翻转；-1:沿x，y同时翻转
```

##### 复制图片

```python
imgcopy = img.copy()
```

##### 颜色空间转换

```python
#彩色图像转为灰度图像
img2 = cv2.cvtColor(img,cv2.COLOR_RGB2GRAY) 
#灰度图像转为彩色图像
img3 = cv2.cvtColor(img,cv2.COLOR_GRAY2RGB)
# cv2.COLOR_X2Y，其中X,Y = RGB, BGR, GRAY, HSV, YCrCb, XYZ, Lab, Luv, HLS
```

##### 图片画图

```python
np.set_printoptions(threshold='nan')
# 创建一个宽512高512的黑色画布，RGB(0,0,0)即黑色
img=np.zeros((512,512,3),np.uint8)
# 画直线,图片对象，起始坐标(x轴,y轴)，结束坐标，颜色，宽度
cv2.line(img,(0,0),(311,511),(255,0,0),10)
# 画矩形，图片对象，左上角坐标，右下角坐标，颜色，宽度
cv2.rectangle(img,(30,166),(130,266),(0,255,0),3)
# 画圆形，图片对象，中心点坐标，半径大小，颜色，宽度
cv2.circle(img,(222,222),50,(255.111,111),-1)
# 画椭圆形，图片对象，中心点坐标，长短轴，顺时针旋转度数，开始角度(右长轴表0度，上短轴表270度)，颜色，宽度
cv2.ellipse(img,(333,333),(50,20),0,0,150,(255,222,222),-1)
# 画多边形，指定各个点坐标,array必须是int32类型
pts=np.array([[10,5],[20,30],[70,20],[50,10]], np.int32)
# -1表示该纬度靠后面的纬度自动计算出来，实际上是4
pts = pts.reshape((-1,1,2,))
# 画多条线，False表不闭合，True表示闭合，闭合即多边形
cv2.polylines(img,[pts],True,(255,255,0),5)
#写字,字体选择
font=cv2.FONT_HERSHEY_SCRIPT_COMPLEX
# 图片对象，要写的内容，左边距，字的底部到画布上端的距离，字体，大小，颜色，粗细
cv2.putText(img,"OpenCV",(10,400),font,3.5,(255,255,255),2)
```

### 图片大小区域处理

#### 图片缩放

```python
img = cv2.imread('')
# 缩放成200x200的方形图像，没有指定插值方法，会失真
img_200x200 = cv2.resize(img, (200, 200))
# 不直接指定缩放后大小，通过fx和fy指定缩放比例，0.5则长宽都为原来一半
# 等效于img_200x300 = cv2.resize(img, (300, 200))，注意指定大小的格式是(宽度,高度)
# 插值方法默认是cv2.INTER_LINEAR，这里指定为最近邻插值
img_200x300 = cv2.resize(img, (0, 0), fx=0.5, fy=0.5, 
                              interpolation=cv2.INTER_NEAREST)
```

#### 图片裁剪

```python
cropImg = img[20:150, -180:-50]
```

#### 图片补边

```python
# 给图片上下各贴50像素的黑边，
borderImg = cv2.copyMakeBorder(img, 50, 50, 0, 0,cv2.BORDER_CONSTANT,value=(0, 0, 0))
```

#### 图片本身处理

##### HSV空间调节

图像本身的属性操作也非常多，比如可以通过HSV空间对色调和明暗进行调节。HSV空间是由美国的图形学专家A. R. Smith提出的一种颜色空间，HSV分别是色调（Hue），饱和度（Saturation）和明度（Value）。在HSV空间中进行调节就避免了直接在RGB空间中调节是还需要考虑三个通道的相关性。OpenCV中H的取值是[0, 180)，其他两个通道的取值都是[0, 256)。

```python
# 通过cv2.cvtColor把图像从BGR转换到HSV
img_hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
# H空间中，绿色比黄色的值高一点，所以给每个像素+15，黄色的树叶就会变绿
turn_green_hsv = img_hsv.copy()
turn_green_hsv[:, :, 0] = (turn_green_hsv[:, :, 0]+15) % 180

# 减小饱和度会让图像损失鲜艳，变得更灰
colorless_hsv = img_hsv.copy()
colorless_hsv[:, :, 1] = 0.5 * colorless_hsv[:, :, 1]

# 减小明度为原来一半
darker_hsv = img_hsv.copy()
darker_hsv[:, :, 2] = 0.5 * darker_hsv[:, :, 2]
```

##### 直方图和Gamma变换

我们都较难一眼就对像素中值的分布有细致的了解，这时候就需要直方图。如果直方图中的成分过于靠近0或者255，可能就出现了暗部细节不足或者亮部细节丢失的情况。当背景里的暗部细节非常弱的个时候，一个常用方法是考虑用Gamma变换来提升暗部细节。Gamma变换是矫正相机直接成像和人眼感受图像差别的一种常用手段，简单来说就是通过非线性变换让图像从对曝光强度的线性响应变得更接近人眼感受到的响应。

```python
import numpy as np

# 分通道计算每个通道的直方图
hist_b = cv2.calcHist([img], [0], None, [256], [0, 256])
hist_g = cv2.calcHist([img], [1], None, [256], [0, 256])
hist_r = cv2.calcHist([img], [2], None, [256], [0, 256])

# 定义Gamma矫正的函数
def gamma_trans(img, gamma):
    # 具体做法是先归一化到1，然后gamma作为指数值求出新的像素值再还原
    gamma_table = [np.power(x/255.0, gamma)*255.0 for x in range(256)]
    gamma_table = np.round(np.array(gamma_table)).astype(np.uint8)
    # 实现这个映射用的是OpenCV的查表函数
    return cv2.LUT(img, gamma_table)

# 执行Gamma矫正，小于1的值让暗部细节大量提升，同时亮部细节少量提升
img_corrected = gamma_trans(img, 0.5)
cv2.imwrite('gamma_corrected.jpg', img_corrected)

# 分通道计算Gamma矫正后的直方图
hist_b_corrected = cv2.calcHist([img_corrected], [0], None, [256], [0, 256])
hist_g_corrected = cv2.calcHist([img_corrected], [1], None, [256], [0, 256])
hist_r_corrected = cv2.calcHist([img_corrected], [2], None, [256], [0, 256])

# 将直方图进行可视化
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()

pix_hists = [
    [hist_b, hist_g, hist_r],
    [hist_b_corrected, hist_g_corrected, hist_r_corrected]
]

pix_vals = range(256)
for sub_plt, pix_hist in zip([121, 122], pix_hists):
    ax = fig.add_subplot(sub_plt, projection='3d')
    for c, z, channel_hist in zip(['b', 'g', 'r'], [20, 10, 0], pix_hist):
        cs = [c] * 256
        ax.bar(pix_vals, channel_hist, zs=z, zdir='y', color=cs, alpha=0.618, edgecolor='none', lw=0)

    ax.set_xlabel('Pixel Values')
    ax.set_xlim([0, 256])
    ax.set_ylabel('Channels')
    ax.set_zlabel('Counts')

plt.show()
```

#### 图片仿射变换

##### 仿射变换原理：略

在OpenCV中实现仿射变换是通过仿射变换矩阵和cv2.warpAffine()共同处理。

```python
import cv2
import numpy as np

# 读取一张斯里兰卡拍摄的大象照片
img = cv2.imread('lanka_safari.jpg')

# 沿着横纵轴放大1.6倍，然后平移(-150,-240)，最后沿原图大小截取，等效于裁剪并放大
M_crop_elephant = np.array([
    [1.6, 0, -150],
    [0, 1.6, -240]
], dtype=np.float32)

img_elephant = cv2.warpAffine(img, M_crop_elephant, (400, 600))
cv2.imwrite('lanka_elephant.jpg', img_elephant)

# x轴的剪切变换，角度15°
theta = 15 * np.pi / 180
M_shear = np.array([
    [1, np.tan(theta), 0],
    [0, 1, 0]
], dtype=np.float32)

img_sheared = cv2.warpAffine(img, M_shear, (400, 600))
cv2.imwrite('lanka_safari_sheared.jpg', img_sheared)

# 顺时针旋转，角度15°
M_rotate = np.array([
    [np.cos(theta), -np.sin(theta), 0],
    [np.sin(theta), np.cos(theta), 0]
], dtype=np.float32)

img_rotated = cv2.warpAffine(img, M_rotate, (400, 600))
cv2.imwrite('lanka_safari_rotated.jpg', img_rotated)

# 某种变换，具体旋转+缩放+旋转组合可以通过SVD分解理解
M = np.array([
    [1, 1.5, -400],
    [0.5, 2, -100]
], dtype=np.float32)

img_transformed = cv2.warpAffine(img, M, (400, 600))
cv2.imwrite('lanka_safari_transformed.jpg', img_transformed)
```

## opencv视频功能

视频中最常用的就是从视频设备采集图片或者视频，或者读取视频文件并从中采样。所以比较重要的也是两个模块，一个是VideoCapture，用于获取相机设备并捕获图像和视频，或是从文件中捕获。还有一个VideoWriter，用于生成视频。

```python
import cv2
import time

interval = 60           # 捕获图像的间隔，单位：秒
num_frames = 500        # 捕获图像的总帧数
out_fps = 24            # 输出文件的帧率

# VideoCapture(0)表示打开默认的相机
cap = cv2.VideoCapture(0)

# 获取捕获的分辨率
size =(int(cap.get(cv2.CAP_PROP_FRAME_WIDTH)),
       int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT)))
       
# 设置要保存视频的编码，分辨率和帧率
video = cv2.VideoWriter(
    "time_lapse.avi", 
    cv2.VideoWriter_fourcc('M','P','4','2'), 
    out_fps, 
    size
)

# 对于一些低画质的摄像头，前面的帧可能不稳定，略过
for i in range(42):
    cap.read()

# 开始捕获，通过read()函数获取捕获的帧
try:
    for i in range(num_frames):
        _, frame = cap.read()
        video.write(frame)

        # 如果希望把每一帧也存成文件，比如制作GIF，则取消下面的注释
        # filename = '{:0>6d}.png'.format(i)
        # cv2.imwrite(filename, frame)

        print('Frame {} is captured.'.format(i))
        time.sleep(interval)
except KeyboardInterrupt:
    # 提前停止捕获
    print('Stopped! {}/{} frames captured!'.format(i, num_frames))

# 释放资源并写入视频文件
video.release()
cap.release()
```

VideoWriter中的一个函数cv2.VideoWriter_fourcc()。这个函数指定了视频编码的格式，更多编码方式：http://www.fourcc.org/codecs.php

KeyboardInterrupt，这是一个常用的异常，用来获取用户Ctrl+C的中止，捕获这个异常后直接结束循环并释放VideoCapture和VideoWriter的资源，使已经捕获好的部分视频可以顺利生成。

#### 按照指定帧数截屏保存图片

```python
import cv2
import os
import sys

# 第一个输入参数是包含视频片段的路径
input_path = sys.argv[1]

# 第二个输入参数是设定每隔多少帧截取一帧
frame_interval = int(sys.argv[2])

# 列出文件夹下所有的视频文件
filenames = os.listdir(input_path)

# 获取文件夹名称
video_prefix = input_path.split(os.sep)[-1]

# 建立一个新的文件夹，名称为原文件夹名称后加上_frames
frame_path = '{}_frames'.format(input_path)
if not os.path.exists(frame_path):
    os.mkdir(frame_path)

# 初始化一个VideoCapture对象
cap = cv2.VideoCapture()

# 遍历所有文件
for filename in filenames:
    filepath = os.sep.join([input_path, filename])
    
    # VideoCapture::open函数可以从文件获取视频
    cap.open(filepath)
    
    # 获取视频帧数
    n_frames = int(cap.get(cv2.CAP_PROP_FRAME_COUNT))

    # 同样为了避免视频头几帧质量低下，黑屏或者无关等
    for i in range(42):
        cap.read()
    
    for i in range(n_frames):
        ret, frame = cap.read()
        
        # 每隔frame_interval帧进行一次截屏操作
        if i % frame_interval == 0:
            imagename = '{}_{}_{:0>6d}.jpg'.format(video_prefix, filename.split('.')[0], i)
            imagepath = os.sep.join([frame_path, imagename])
            print('exported {}!'.format(imagepath))
            cv2.imwrite(imagepath, frame)

# 执行结束释放资源
cap.release()
```



