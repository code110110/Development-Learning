# 深度图像

## RGB 图像与深度图像对齐

### [深度图像配准（Registration）原理](https://www.cnblogs.com/cv-pr/p/5769617.html)

机器视觉中，3D相机产生的深度图像（depth image）通常需要配准（registration），以生成配准深度图像（registed depth image）。实际上配准的目的就是想**让深度图和彩色图重合在一起**，即是将深度图像的图像坐标系转换到彩色图像的图像坐标系下。下面我们来介绍其推导的过程。

**1. 原理**

为了描述方便，首先做些简单的假设。如下图所示，3D相机的左侧相机（left camera）为红外相机（即深度相机，ir camera），右侧相机（right camera）为彩色相机（color camera）。现在主流的3D相机都是这样的布局.。已知彩色图像的像素表示为
$$
(u_R,v_R,z_R)^⊤
$$

$$
u_R,v_R,z_R,分别表示彩色图像的横坐标，纵坐标和相机坐标系下的深度值（z方向上的值，非两点的距离）；
$$

同样地，深度图像的像素为
$$
(u_L,v_L,z_L)^⊤
，u_L,v_L,z_L,分别表示深度图像的横坐标，纵坐标和相机坐标系下的深度值（z方向上的值，非两点的距离）。
$$
注意为了方便表示，本文中下标的R,L分别表示Right,Left的意思。那么深度图配准到彩色图的过程就是找到如下公式中的变换矩阵W′：
$$
\left[
\begin{matrix}
u_R\\
v_R \\
1
\end{matrix}
\right]
=W'\left[
\begin{matrix}
u_L\\
v_L \\
1
\end{matrix}
\right]
$$
***a. 构造左侧相机的左侧相机坐标系到图像坐标系的变换***

由相机原理（可参考[相机标定(2)---摄像机标定原理](http://blog.csdn.net/lixianjun913/article/details/10032019)），可知相机坐标到图像坐标的变换为：
$$
z_L\left[
\begin{matrix}
u_L\\
v_L \\
1
\end{matrix}
\right]
=\left[
\begin{matrix}
f/d_x&0&u^0_L&0\\
0&f/d_x&u^0_L&0 \\
0&0&1&0
\end{matrix}
\right]\left[
\begin{matrix}
x_L\\
y_L \\
z_L\\
1
\end{matrix}
\right]
$$
以上变换过程等价于如下的表达：
$$
z_{L}\left[\begin{array}{c}
u_{L} \\
v_{L} \\
1 \\
1 / z_{L}
\end{array}\right]=\underbrace{\left[\begin{array}{cccc}
f / d x & 0 & u_{L}^{0} & 0 \\
0 & f / d y & v_{L}^{0} & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{array}\right]}_{L R}\left[\begin{array}{c}
x_{L} \\
y_{L} \\
z_{L} \\
1
\end{array}\right]
$$
 于是图像坐标系到相机坐标系的变换为：
$$
\left[\begin{array}{c}
x_{L} \\
y_{L} \\
z_{L} \\
1
\end{array}\right]=z_{L} * L R^{-1} *\left[\begin{array}{c}
u_{L} \\
v_{L} \\
1 \\
1 / z_{L}
\end{array}\right]
$$
首先需要了解，原始图像的深度信息与彩色信息一般是不能直接对齐的。这是因为深度相机和RGB相机的成像原理是不同的，往往使用的也是不同的镜头，因此对于同一场景，两个相机捕获的影像可能会存在视场差异或者畸变等问题，因此需要通过某种方法对这两部分信息进行校准，以实现对齐。

对于Opencv和Python，同样需要使用到相机内参（Camera intrinsics）和外参（Extrinsics）进行对齐。如下是一个如何进行对齐的简单例子，首先你需要有两个相机的校准参数。

```python
import cv2
import numpy as np
import struct 

def read_depth(file):   
    f = open(file, 'rb')
    check = np.fromstring(f.read(5), np.uint8)
    w = struct.unpack('i', f.read(4))[0]
    h = struct.unpack('i', f.read(4))[0]
    if (check != np.array([68, 65, 86, 76, w % 256], np.uint8)).all():
        raise Exception("Could not load depth file")
    depth = np.fromstring(f.read(), np.float32).reshape((h, w))
    return depth

def align_depth_to_rgb(depth_map, intrinsic_matrix_depth, extrinsic_matrix_depth, intrinsic_matrix_color, extrinsic_matrix_color):
    h, w = depth_map.shape
    new_depth_map = np.zeros((h, w), dtype=np.uint8)

    fx_d = intrinsic_matrix_depth[0, 0]
    fy_d = intrinsic_matrix_depth[1, 1]
    cx_d = intrinsic_matrix_depth[0, 2]
    cy_d = intrinsic_matrix_depth[1, 2]

    fx_rgb = intrinsic_matrix_color[0, 0]
    fy_rgb = intrinsic_matrix_color[1, 1]
    cx_rgb = intrinsic_matrix_color[0, 2]
    cy_rgb = intrinsic_matrix_color[1, 2]

    for y in range(h):
        for x in range(w):
            p_z = depth_map[y, x]
            if p_z == 0:  # no information
                continue
            # map depth pixel to depth camera frame
            p1 = (inverse_intrinsic_matrix_depth @ np.array([x * p_z, y * p_z, p_z, 1]).reshape(-1, 1)).flatten()
            # map depth camera frame to world frame
            p2 = (extrinsic_matrix_depth @ np.array([p1[0], p1[1], p1[2], 1]).reshape(-1, 1)).flatten()
            # map world frame to color camera frame
            p3 = (inverse_extrinsic_matrix_color @ np.array([p2[0], p2[1], p2[2], 1]).reshape(-1, 1)).flatten()
            # map color camera frame to color pixel
            u = int(fx_rgb * p3[0] / p3[2] + cx_rgb)
            v = int(fy_rgb * p3[1] / p3[2] + cy_rgb)
            if u >= 0 and u < w and v >= 0 and v < h:
                new_depth_map[v, u] = p_z

    return new_depth_map

color_image_filepath = 'your_color_image_filepath'
depth_image_filepath = 'your_depth_image_filepath'
new_depth_image_filepath = 'your_new_depth_image_filepath'

color_image = cv2.imread(color_image_filepath)
depth_image = read_depth(depth_image_filepath)

# Your camera calibration parameters, this is just an example 
intrinsic_matrix_depth = np.eye(3, 3)
extrinsic_matrix_depth = np.eye(4, 4)
intrinsic_matrix_color = np.eye(3, 3)
extrinsic_matrix_color = np.eye(4, 4)

inverse_intrinsic_matrix_depth = np.linalg.inv(intrinsic_matrix_depth)
inverse_extrinsic_matrix_color = np.linalg.inv(extrinsic_matrix_color)

# perform alignment
new_depth_image = align_depth_to_rgb(depth_image, intrinsic_matrix_depth, extrinsic_matrix_depth, intrinsic_matrix_color, extrinsic_matrix_color)
cv2.imwrite(new_depth_image_filepath, new_depth_image)
```

## Realsense相机在linux下的配置使用，RGB与depth图像对齐

可以读取播放之前通过GUI面板保存的bag，或者实时通过相机拍摄记录视频（这种方式还可以指定图像的拍摄大小，以及拍摄的帧率）

另外还提供了两种功能：把深度图对齐到彩色图像，或者把彩色图像对齐到深度图像。

```python
#coding=utf-8
import pyrealsense2 as rs
import numpy as np
import cv2
 
#0:使用相机
# 1:使用API录制好的bag
USE_ROS_BAG=1
#0:彩色图像对齐到深度图;
# 1:深度图对齐到彩色图像
ALIGN_WAY=1
 
def Align_version(frames,align,show_pic=0):
    # 对齐版本
    aligned_frames = align.process(frames)
    depth_frame_aligned = aligned_frames .get_depth_frame()
    color_frame_aligned = aligned_frames .get_color_frame()
    # if not depth_frame_aligned or not color_frame_aligned:
    #     continue
    color_image_aligned = np.asanyarray(color_frame_aligned.get_data())
    if USE_ROS_BAG:
        color_image_aligned=cv2.cvtColor(color_image_aligned,cv2.COLOR_BGR2RGB)
    depth_image_aligned = np.asanyarray(depth_frame_aligned.get_data())
 
    depth_colormap_aligned = cv2.applyColorMap(cv2.convertScaleAbs(depth_image_aligned, alpha=0.05), cv2.COLORMAP_JET)
    images_aligned = np.hstack((color_image_aligned, depth_colormap_aligned))
    if show_pic:
        cv2.imshow('aligned_images', images_aligned)
    return color_image_aligned,depth_image_aligned,depth_colormap_aligned
 
def Unalign_version(frames,show_pic=0):
    # 未对齐版本
    # Wait for a coherent pair of frames: depth and color
    frames = pipeline.wait_for_frames()
    depth_frame = frames .get_depth_frame()
    color_frame = frames .get_color_frame()
 
    if not USE_ROS_BAG:
        left_frame = frames.get_infrared_frame(1)
        right_frame = frames.get_infrared_frame(2)
        left_image = np.asanyarray(left_frame.get_data())
        right_image = np.asanyarray(right_frame.get_data())
        if show_pic:
            cv2.imshow('left_images', left_image)
            cv2.imshow('right_images', right_image)
    # if not depth_frame or not color_frame:
    #     continue
    color_image = np.asanyarray(color_frame.get_data())
    print("color:",color_image.shape)
    depth_image= np.asanyarray(depth_frame.get_data())
    print("depth:",depth_image.shape)
 
    #相机API录制的大小rosbag的rgb图像与depth图像不一致，用resize调整到一样大
    if USE_ROS_BAG:
        color_image=cv2.cvtColor(color_image,cv2.COLOR_BGR2RGB)
        if ALIGN_WAY:  #深度图对齐到彩色图像
            depth_image=cv2.resize(depth_image,(color_image.shape[1],color_image.shape[0]))
        else:   #彩色图像对齐到深度图
            color_image=cv2.resize(color_image,(depth_image.shape[1],depth_image.shape[0]))
    # 上色
    depth_colormap = cv2.applyColorMap(cv2.convertScaleAbs(depth_image, alpha=0.05), cv2.COLORMAP_JET)
    # Stack both images horizontally
    images = np.hstack((color_image, depth_colormap))
    if show_pic:
        cv2.imshow('images', images)
    return color_image,depth_image,depth_colormap
 
if __name__ == "__main__":
    # Configure depth and color streams
    pipeline = rs.pipeline()
    config = rs.config()
 
    if USE_ROS_BAG:
        config.enable_device_from_file("666.bag")#这是打开相机API录制的视频
    else:
        config.enable_stream(rs.stream.color, 640, 480, rs.format.bgr8, 30)  #10、15或者30可选,20或者25会报错，其他帧率未尝试
        config.enable_stream(rs.stream.depth, 640, 480, rs.format.z16, 30)
        #左右双目
        config.enable_stream(rs.stream.infrared, 1, 640, 480, rs.format.y8, 30)  
        config.enable_stream(rs.stream.infrared, 2, 640, 480, rs.format.y8, 30)
 
    if ALIGN_WAY:
        way=rs.stream.color
    else:
        way=rs.stream.depth
    align = rs.align(way)
    profile =pipeline.start(config)
 
 
    depth_sensor = profile.get_device().first_depth_sensor()
    depth_scale = depth_sensor.get_depth_scale()
    print("scale:", depth_scale)
    # 深度比例系数为： 0.0010000000474974513
 
    try:
        while True:
            frames = pipeline.wait_for_frames()
            color_image_aligned,depth_image_aligned,depth_colormap_aligned=Align_version(frames,align,show_pic=1)
            color_image,depth_image,depth_colormap=Unalign_version(frames,show_pic=1)     
            #print(depth_image_aligned*depth_scale)
            key = cv2.waitKey(1)
            # Press esc or 'q' to close the image window
            if key & 0xFF == ord('q') or key == 27:
                cv2.destroyAllWindows()
                break
    finally:
        # Stop streaming
        pipeline.stop()
```

注意：
1.函数Align_version和Unalign_version分别读取相机或bag的图像，前者还进行深度图与RGB图像的对齐。具体是谁对齐到谁，由变量ALIGN_WAY控制。

2.depth_scale是比例系数，用于在深度图像中直接取值以后，乘以这个系数，得到现实里实际的深度值。（即相机坐标系下的z值。）

3.show_pic是用于是否显示图片的变量，两个函数返回的三个变量，分别是彩色图像，深度图像，以及上色以后的深度图像（近处偏冷，远处偏暖）

## L515利用pyrealsense2获取图像及对齐

和其他realsense不同的地方主要是L515的图像尺寸，depth和infrared是1024*768，rgb是1280*720。

对齐图像的问题上，有两中方式。在测试下，以depth为对象的对齐可以将三个传感器的图像尺寸及画面对齐。以color为对象的对齐可以对齐深度图和RGB图像，但是对红外图像无效。

```python
import pyrealsense2 as rs
import numpy as np
import cv2
import os
 
# Configure depth and rgb and infrared streams
pipeline = rs.pipeline()
config = rs.config()
config.enable_stream(rs.stream.depth, 1024, 768, rs.format.z16, 30)
config.enable_stream(rs.stream.infrared, 1024, 768, rs.format.y8, 30)
config.enable_stream(rs.stream.color, 1280, 720, rs.format.bgr8, 30)

# Align objects
#align_to = rs.stream.color
align_to = rs.stream.depth
align = rs.align(align_to)

# Start streaming
profile = pipeline.start(config)

# Depth scale
depth_sensor = profile.get_device().first_depth_sensor()
depth_scale = depth_sensor.get_depth_scale()
print("Depth scale is: " , depth_scale)

# Frames: depth and rgb and infrared
frames = pipeline.wait_for_frames()
aligned_frames = align.process(frames)
depth_frame = aligned_frames.get_depth_frame()
infrared_frame = aligned_frames.get_infrared_frame()
color_frame = aligned_frames.get_color_frame()

# Convert images to numpy arrays
depth_data = np.asanyarray(depth_frame.get_data(), dtype="float16")
depth_image = np.asanyarray(depth_frame.get_data())
infrared_image = np.asanyarray(infrared_frame.get_data())
color_image = np.asanyarray(color_frame.get_data())

# Save data
np.savetxt('./realsense/depth_data.txt', depth_data)
depth_colormap = cv2.applyColorMap(cv2.convertScaleAbs(depth_image, alpha=0.03), cv2.COLORMAP_JET)
cv2.imwrite('./realsense/depth_image.jpg', depth_colormap)
cv2.imwrite('./realsense/color_image.jpg', color_image)
cv2.imwrite('./realsense/infrared_image.jpg', infrared_image)
```

## [librealsense](https://github.com/IntelRealSense/librealsense/tree/master)/[wrappers](https://github.com/IntelRealSense/librealsense/tree/master/wrappers)/[python](https://github.com/IntelRealSense/librealsense/tree/master/wrappers/python)/[examples](https://github.com/IntelRealSense/librealsense/tree/master/wrappers/python/examples)/align-depth2color.py

```python
## License: Apache 2.0. See LICENSE file in root directory.
## Copyright(c) 2017 Intel Corporation. All Rights Reserved.

#####################################################
##              Align Depth to Color               ##
#####################################################

# First import the library
import pyrealsense2 as rs
# Import Numpy for easy array manipulation
import numpy as np
# Import OpenCV for easy image rendering
import cv2

# Create a pipeline
pipeline = rs.pipeline()

# Create a config and configure the pipeline to stream
#  different resolutions of color and depth streams
config = rs.config()

# Get device product line for setting a supporting resolution
pipeline_wrapper = rs.pipeline_wrapper(pipeline)
pipeline_profile = config.resolve(pipeline_wrapper)
device = pipeline_profile.get_device()
device_product_line = str(device.get_info(rs.camera_info.product_line))

found_rgb = False
for s in device.sensors:
    if s.get_info(rs.camera_info.name) == 'RGB Camera':
        found_rgb = True
        break
if not found_rgb:
    print("The demo requires Depth camera with Color sensor")
    exit(0)

config.enable_stream(rs.stream.depth, 640, 480, rs.format.z16, 30)

if device_product_line == 'L500':
    config.enable_stream(rs.stream.color, 960, 540, rs.format.bgr8, 30)
else:
    config.enable_stream(rs.stream.color, 640, 480, rs.format.bgr8, 30)

# Start streaming
profile = pipeline.start(config)

# Getting the depth sensor's depth scale (see rs-align example for explanation)
depth_sensor = profile.get_device().first_depth_sensor()
depth_scale = depth_sensor.get_depth_scale()
print("Depth Scale is: " , depth_scale)

# We will be removing the background of objects more than
#  clipping_distance_in_meters meters away
clipping_distance_in_meters = 1 #1 meter
clipping_distance = clipping_distance_in_meters / depth_scale

# Create an align object
# rs.align allows us to perform alignment of depth frames to others frames
# The "align_to" is the stream type to which we plan to align depth frames.
align_to = rs.stream.color
align = rs.align(align_to)

# Streaming loop
try:
    while True:
        # Get frameset of color and depth
        frames = pipeline.wait_for_frames()
        # frames.get_depth_frame() is a 640x360 depth image

        # Align the depth frame to color frame
        aligned_frames = align.process(frames)

        # Get aligned frames
        aligned_depth_frame = aligned_frames.get_depth_frame() # aligned_depth_frame is a 640x480 depth image
        color_frame = aligned_frames.get_color_frame()

        # Validate that both frames are valid
        if not aligned_depth_frame or not color_frame:
            continue

        depth_image = np.asanyarray(aligned_depth_frame.get_data())
        color_image = np.asanyarray(color_frame.get_data())

        # Remove background - Set pixels further than clipping_distance to grey
        grey_color = 153
        depth_image_3d = np.dstack((depth_image,depth_image,depth_image)) #depth image is 1 channel, color is 3 channels
        bg_removed = np.where((depth_image_3d > clipping_distance) | (depth_image_3d <= 0), grey_color, color_image)

        # Render images:
        #   depth align to color on left
        #   depth on right
        depth_colormap = cv2.applyColorMap(cv2.convertScaleAbs(depth_image, alpha=0.03), cv2.COLORMAP_JET)
        images = np.hstack((bg_removed, depth_colormap))

        cv2.namedWindow('Align Example', cv2.WINDOW_NORMAL)
        cv2.imshow('Align Example', images)
        key = cv2.waitKey(1)
        # Press esc or 'q' to close the image window
        if key & 0xFF == ord('q') or key == 27:
            cv2.destroyAllWindows()
            break
finally:
    pipeline.stop()
```

