# mobilefacenet-ncnn

## 1. 模型的制作与训练

本项目使用insightface的mobilefacenet网络进行人脸检测与识别。首先我们要做的就是模型的训练，然后再把训练好的模型转化为ncnn的模型，方法请看[https://github.com/liguiyuan/mobilefacenet-ncnn/tree/master/deploy](https://github.com/liguiyuan/mobilefacenet-ncnn/tree/master/deploy)，最后输出`mobilefacenet.param` ，`mobilefacenet.bin`这两个文件，这就是我们部署所需的两个文件。



## 2. 编译ncnn

这个步骤可以参考官方的教程：[https://github.com/Tencent/ncnn/wiki/how-to-build#build-for-linux-x86](https://github.com/Tencent/ncnn/wiki/how-to-build#build-for-linux-x86)

根据自己所需部署的环境（linux-x86，Android）进行选择编译方法，其实大同小异。编译成功之后在ncnn/build/install/ 就会生成`include`, `lib` 文件夹，里面包含自己所需的头文件和ncnn 库文件。



## 3. 验证模型

在本项目的examples这里提供了一个ncnn最小化模型部署的例子，里面包含人脸检测和人脸相似度对比。

使用方法：

```bash
$ cd examples/arcface/
$ make		# 编译程序
$ ./main	# 运行程序
```



## 4. 性能分析

我们可以通过ncnn的benchmark工具进行分析，可以在pc或Android设备进行分析。分析方法：[https://github.com/Tencent/ncnn/tree/master/benchmark](https://github.com/Tencent/ncnn/tree/master/benchmark)。

在安卓设备华为P9 Kirin 950 (4x2.3GHz A72+4x1.8GHz A53)上自己性能表现如下:

```
HWEVA:/data/local/tmp $ ./benchncnn 8 4 0                                      
loop_count = 8
num_threads = 4
powersave = 0
gpu_device = -1
          squeezenet  min =   34.52  max =   45.66  avg =   36.50
     squeezenet-int8  min =   32.57  max =   45.46  avg =   34.81
           mobilenet  min =   42.87  max =   67.39  avg =   53.37
      mobilenet-int8  min =   40.73  max =   41.25  avg =   40.95
        mobilenet_v2  min =   44.68  max =   45.42  avg =   44.94
          shufflenet  min =   19.58  max =   43.51  avg =   27.65
             mnasnet  min =   32.29  max =   40.78  avg =   33.51
     proxylessnasnet  min =   44.43  max =   44.75  avg =   44.56
           googlenet  min =  110.20  max =  161.82  avg =  124.23
      googlenet-int8  min =   93.00  max =   95.14  avg =   93.86
            resnet18  min =  143.92  max =  145.31  avg =  144.34
       resnet18-int8  min =   84.11  max =  118.15  avg =   88.80
             alexnet  min =  301.81  max =  327.68  avg =  305.20
               vgg16  min =  578.48  max =  626.68  avg =  604.68
            resnet50  min =  614.11  max =  663.31  avg =  621.09
       resnet50-int8  min =  193.42  max =  194.41  avg =  193.89
      squeezenet-ssd  min =  102.94  max =  103.96  avg =  103.40
 squeezenet-ssd-int8  min =  106.35  max =  110.12  avg =  107.56
       mobilenet-ssd  min =   95.07  max =   96.55  avg =   95.52
  mobilenet-ssd-int8  min =   83.72  max =   84.12  avg =   83.90
      mobilenet-yolo  min =  264.28  max =  270.89  avg =  266.10
    mobilenet-yolov3  min =  243.05  max =  252.33  avg =  245.20
       mobilefacenet  min =   17.05  max =   31.43  avg =   22.16
```



## 5.手机端人脸检测识别

安卓端的代码是在官网提供的mtcnn人脸检测的工程上进行修改，使用华为P9 Kirin 950，在多人脸检测上平均时间 在90～110ms 之间，第一次检测的时间比较长（可能第一次要加载到内存吧？），人脸检测效果还不错吧。

|              | Min     | Max      | Avg      |
| ------------ | ------- | -------- | -------- |
| 多人脸检测   | 90.65ms | 212.35ms | 106.73ms |
| 最大人脸检测 | 31ms    | 194ms    | 69ms     |
| 人脸识别     | 135ms   | 278ms    | 159ms    |

编译好的apk 文件在tools文件夹，可以在自己是手机上进行测试。

