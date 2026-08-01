---
title: EasyClick安卓文档_安卓手机自动化脚本_YOLO使用说明
hide_title: false
hide_table_of_contents: false
sidebar_label: YOLO使用说明
description: 'EasyClick 代码热更新,无需无障碍和USB调试实现自动化'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - 脚本热更新
  - 代码热更新
  - YOLO使用说明
  - https
  - com
  - YOLO
  - ncnn
  - EC
  - docs
  - download
  - anaconda
  - YOLOV8
  - yolov8
  - 手机自动化
  - 自动化测试
---
# YOLO使用说明
:::tip 说明
- EC 的YOLOV8需要在EC 安卓的10.15.0+版本上才支持
- EC安卓版本集成的是Tencent/ncnn网络神经组件为手机设备特地优化的版本
- ncnn支持yolov5-yolov8，这里采用的是yolov8模型进行训练、推理和检测
- YOLOV8官方网站 [https://docs.ultralytics.com/modes/train/](https://docs.ultralytics.com/modes/train/)
- YOLODemo下载，[资源下载](/docs/zh-cn/tools/download_resources)，网盘资源里面的`YOLO资源`文件夹`YOLODemo.zip`
  :::

## 安装及训练模型
- YOLO训练使用的是Python环境，这里下载 **anaconda**这个软件
    - 下载地址 [https://www.anaconda.com/download](https://www.anaconda.com/download)
    - 清华源下载地址 https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2024.06-1-Windows-x86_64.exe
    - 更多的 anaconda知识 请看 https://blog.csdn.net/ddafei/article/details/140326536
### 安装软件
- 下载安装好了**anaconda**，启动这个软件，新建一个虚拟环境
- 点击**create**按钮，新建一个虚拟环境，这里名字叫做**yolotest**为例子，可以是其他名称,Python版本选择3.8.19，点击Create等待环境创建成功
  <br/><img src="/androidimg/yolo/yolo1.png" alt="" style={{zoom:'30%'}} />
- 选择**yolotest**右边的绿色三角号按钮，会弹出选项，选择**Open Terminal**选项，进入了终端
  <br/><img src="/androidimg/yolo/yolo2.png" alt="" style={{zoom:'30%'}} />
    - 终端前面带有 **yolotest** 虚拟环境名称的说明成功了
      <br/><img src="/androidimg/yolo/yolo3.png" alt="" style={{zoom:'30%'}} />

### 配置环境
- 接上面的步骤，安装需要的Python类库
- 在终端依次输入输入以下命令:
  ```shell showLineNumbers
  pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/
  pip config set install.trusted-host pypi.tuna.tsinghua.edu.cn
  # 这里用的是 yolo 0.3.1 版本
  # 使用最新版本运行命令: pip install yolo
  pip install yolo==0.3.1
  # 这里用的是 ultralytics 8.2.79 版本 
  # 使用最新版本运行命令: pip install ultralytics
  pip install ultralytics==8.2.79
  pip install ncnn==1.0.20240410
  pip install labelimg
  #最后输入 pip list 查看安装的类库列表
  ```


- yolo,ultralytics安装结束
  <br/><img src="/androidimg/yolo/yolo4.png" alt="" style={{zoom:'20%'}} /><img src="/androidimg/yolo/yolo5.png" alt="" style={{zoom:'20%'}} />
- 在命令行输入yolo命令验证是否成功
  <br/><img src="/androidimg/yolo/yolo6.png" alt="" style={{zoom:'30%'}} />
- 验证**labelimg**图片标注工具是否正常，控制台输入labelimg，即可打开labelimg程序
  <br/><img src="/androidimg/yolo/yolo7.png" alt="" style={{zoom:'30%'}} />

### 新建训练目录
- 找个空目录，新建文件夹，名称叫做 yolotrain,也可以是其他名称,这里的例子是在E盘的yolotrain文件夹
- 在 yolotrain 新建labels和images文件夹，在images文件夹新建test,train,val三个文件夹，在labesl下面新建train,val两个文件夹
- 结构入下图所示
  <br/><img src="/androidimg/yolo/yolo8.png" alt="" style={{zoom:'30%'}} />
- images文件夹下面的test,train,val三个文件夹，全部存放相同的要训练的图片，例如train下面是 1.png,2.png，将这个两个文件复制一份到val，再复制一份到test
- labels下面的train,val两个文件夹存放的是用labelimg标注的标签数据文件，等会 labelimg 使用会用到这个文件夹

### 标注数据
- 将准备好的图片放到 **images/train** 文件夹，下面并且复制一份到 **images/val** 和 **images/test**两个文件夹
- 接上步骤 **配置环境** 的命令控制台,输入 **labelimg** 命令，即可打开 **labelimg**  程序
    - 点击 **Open Dir** 按钮，选择到你的  **images/train** 文件夹即可，例如 E:/yolotest/images/train
    - 点击 **Change Save Dir** 按钮，选择到你的  **labels/train** 文件夹即可，例如 E:/yolotest/labels/train
    - 点击**Save** 下面的格式按钮，调整到 **YOLO** 格式的模式即可,请看下图
      <br/><img src="/androidimg/yolo/yolo9.png" alt="" style={{zoom:'30%'}} />

- 下面开始标注数据
    - 例子是 某音的 **点赞** 和 **评论** 按钮
    - 在图片上右键菜单，选择 **Create RectBox** 选项，或者点击左侧的 **Create RectBox**
      <img src="/androidimg/yolo/yolo10.png" alt="" style={{zoom:'30%'}} />
    - 在 **点赞** 图片区域拉出选框，会弹出框，输入分类名称，这里填写的是 **aixin**，点击ok即可保存 ，这分类名称注意后面函数调用要用到
      <img src="/androidimg/yolo/yolo11.png" alt="" style={{zoom:'30%'}} />
    - 在 **评论** 图片区域拉出选框，会弹出框，输入分类名称，这里填写的是 **pinglun**，点击ok即可保存
      <img src="/androidimg/yolo/yolo12.png" alt="" style={{zoom:'30%'}} />

    - 第一个图片标注完成后，点击左侧的 **Save** 按钮保存，然后点击 **Next Image** 切换标注下一张图片，操作和第一张图片类似
    - 图标标注保存后，会在 **labels/train** 文件夹存储了标注的数据，classes.txt内容就是分类名称，其他的是和图片相同的名称的txt标注文件
      <img src="/androidimg/yolo/yolo13.png" alt="" style={{zoom:'30%'}} />
    - 将 **labels/train** 文件夹数据复制到   **labels/val** 文件夹，等会验证模型的时候使用
### 训练模型
- 在 **yolotrain** 文件下新建一个训练的配置文件，例如文件名称叫 **aixin.yaml**，使用记事本打开，填写内容如下
  ```yaml showLineNumbers
  path: E:/yolotrain
  train: images/train
  val: images/val
  test: images/test
  nc: 2
  names: ["aixin","pinglun"]
  ```
- 参数解释：
    - path: 代表训练的根目录，这里的 yolotrain 在E盘，就写 E:/yolotrain，其他盘的路径自己修改
    - train: 代表要训练的图片文件夹,相对于path路径
    - val: 代表要验证的图片文件夹,相对于path路径
    - test: 代表要测试的图片文件夹,相对于path路径
    - nc: 代表分类名称数量，这是2个，因为使用labelimg标注的是2个类别，如果是多个数据就跟和实际类别数量一样的即可
    - names: 是一个json数组，代表的是标注的分类名称，labelimg使用的是aixin和pinglun两个分类名称，这里就这样写即可
    - 名称的顺序不要写错，会影响训练结果

- 开始训练
- 在控制台输入命令
- 在cmd窗口，输入 `e:/` 回车，在输入 `cd yolotrain`，进入这个文件夹 `yolotrain`，其他路径自行进入
- 训练命令，下面两个任选一个，截图参数看ultralytics官网
  ```shell showLineNumbers
  yolo detect train data=e:/yolotrain/aixin.yaml model=e:/yolotrain/yolov8s.pt imgsz=640
  ```
  ```shell showLineNumbers
  yolo detect train data=e:/yolotrain/aixin.yaml model=e:/yolotrain/yolov8s.pt epochs=100 imgsz=640
  ```

- 这个时候系统会下载yolov8s.pt的基础训练文件，如果下载失败，直接本章节的开头，直接下载demo文件，然后解压复制 yolov8s.pt 到 e:/yolotrain/
    - 也可以到网址下载 https://docs.ultralytics.com/models/yolov8/#performance-metrics
    - 下载地址是 https://github.com/ultralytics/assets/releases/download/v8.2.0/yolov8s.pt
      <img src="/androidimg/yolo/yolo14.png" alt="" style={{zoom:'30%'}} />
- 环境配置成功，一切无误，开始训练中
  <img src="/androidimg/yolo/yolo15.png" alt="" style={{zoom:'30%'}} />

- 训练完毕，注意这里的 **Results saved**后面的路径是动态的，截图中是在 **runs/detect/train**文件夹下，就是E:/yolotrain文件夹下
  <img src="/androidimg/yolo/yolo16.png" alt="" style={{zoom:'30%'}} />
- 在训练完成的目录中可以找到 `best.pt` 的训练模型以及训练中被标记和选中目标的图片结果集

### 验证模型
- 训练完成后，可以对图片进行验证
- cmd控制台输入命令
    - model 参数代表是 需要验证的模型
    - data 代表检测的配置
  ```shell showLineNumbers
  yolo detect val data=e:/yolotrain/aixin.yaml  model=e:/yolotrain/runs/detect/train/weights/best.pt
  ```
- 验证结果，这里的 **Results saved**后面的路径是动态的，请到对应的文件下查看图片标注结果
  <br/><img src="/androidimg/yolo/yolo17.png" alt="" style={{zoom:'30%'}} />

### 导出模型 onnx
- 将训练好的pt文件导出为 onnx 文件，交给EC的函数调用
- cmd控制台输入命令
  - model 参数代表要转换的模型
  ```shell showLineNumbers
  yolo export model=e:/yolotrain/runs/detect/train/weights/best.pt format=onnx
  ```
- 导出成功结果可以看到`export success`后面的路径，进入结果文件夹，可以找到`onnx`结尾的文件，这个两个放到手机sdcard，就可以给程序调用
- onnx调用使用 `yolov8Api.newYolov8Onxx` 函数初始化onnx环境，其他函数调用都是一样


### 导出模型 ncnn
- 将训练好的pt文件导出为ncnn文件，交给EC的函数调用
- cmd控制台输入命令
    - model 参数代表要转换的模型
  ```shell showLineNumbers
  yolo export model=e:/yolotrain/runs/detect/train/weights/best.pt format=ncnn
  ```
- 导出模型的时候要下载`pnnx`组件，这里可以直接本章节的开头，直接下载demo文件，然后解压复制文件`pnnx-20240819-windows.zip` 到 e:/yolotrain/
    - 如果名字不是`pnnx-20240819-windows.zip`，请修改和报错红框截图一样的文件名称即可，或者直接去复制下载地址手动下载`pnnx`组件
      <br/><img src="/androidimg/yolo/yolo18.png" alt="" style={{zoom:'30%'}} />
- 导出成功结果可以看到`export success`后面的路径，进入结果文件夹，可以找到`model.ncnn.param`和`model.ncnn.bin`两个文件，这个两个放到手机sdcard，就可以给程序调用
  <br/><img src="/androidimg/yolo/yolo19.png" alt="" style={{zoom:'20%'}} />
- ncnn 调用使用 `yolov8Api.newYolov8` 函数初始化ncnn环境
## 函数调用
- 将训练好的模型放到手机`/sdcard/`根目录即可，也可以是其他目录，然后给程序调用
- 具体如何使用请看[YOLOV8函数模块](/docs/zh-cn/funcs/yolo-api)

## GPU
- GPU模式训练请看 https://blog.csdn.net/shangyanaf/article/details/139029717

## CONDA创建虚拟环境错误
- Anaconda创建虚拟环境create不能选择python版本,一般是网络问题导致的,可以切换到清华源
- 1.点channels
  <br/><img src="/androidimg/yolo/q1.png" alt="" style={{zoom:'50%'}} />
- 2.删除默认channel
  <br/><img src="/androidimg/yolo/q2.png" alt="" style={{zoom:'50%'}} />
- 3.添加下方两个清华的channel
  每添加一个回车确认配置,等待进度条加载完
  ```text showLineNumbers
  https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  ```
  ```text showLineNumbers
  https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  ```
  <br/><img src="/androidimg/yolo/q3.png" alt="" style={{zoom:'50%'}} />
- 4.添加后,点击update channels
  <br/><img src="/androidimg/yolo/q4.png" alt="" style={{zoom:'50%'}} />

## 下载Arial.ttf错误
- 点我下载 [Arial.ttf](/docs/Arial.ttf)
- 放到 `C:\Users\Administrator\AppData\Roaming\Ultralytics` 目录下，文件名称改为 `Arial.ttf`, `Administrator` 是你当前电脑的用户名
