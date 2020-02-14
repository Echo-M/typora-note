# 工作相关

## 图像存储

### tps

https://tps.alibaba-inc.com/app/imageupload



# 图像基础算法

## 边缘检测

### 1 canny

#### 函数原型

```java
import org.opencv.imgproc.Imgproc;

Imgproc.Canny(I1,res,3,11);
```

```java
void cvCanny(

  const CvArr* image,

  CvArr* edges,

  double threshold1,double threshold2,

  int aperture_size=3

);
```

#### **参数详解**

1. 第一个参数表示**输入**图像，必须为单通道灰度图。

2. 第二个参数表示**输出**的边缘图像，为单通道黑白图。

3. 第三个参数和第四个参数表示**输入**阈值，这二个阈值中当中的小阈值用来控制边缘连接，大的阈值用来控制强边缘的初始分割即如果一个像素的梯度大与上限值，则被认为是边缘像素，如果小于下限阈值，则被抛弃。如果该点的梯度在两者之间则当这个点与高于上限值的像素点连接时我们才保留，否则删除。

   阈值说明：

   > 1.两个阈值是有区别的，高的那个阈值是将要提取轮廓的物体与背景区分开来，就像阈值分割的参数一样，是决定目标与背景对比度的；
   >
   > 低的阈值是用来平滑边缘的轮廓，有时高的阈值设置太大了，可能边缘轮廓不连续或者不够平滑，通过低阈值来平滑轮廓线，或者使不连续的部分连接起来。

   >  2.两个阈值：T1，T2。大于T1的称为强边界。T1和T2之间的为弱边界。
   >
   > 如果只有强边界，那么边界可能断断续续。而且会少分割。所以弱边界的作用就是解决上面这个问题。如果强边界点的8连通区域内有弱边界点，那么认为该弱边界点为强边界。

4. 第五个参数表示**输入** Sobel 算子大小，默认为3即表示一个3*3的矩阵。Sobel 算子与高斯拉普拉斯算子都是常用的边缘算子，详细的数学原理可以查阅专业书籍。

### 2 Sobel

### 3 Laplacian

## 图像轮廓

### 1 findContours查找

#### **函数原型：**

```java
findContours( InputOutputArray image, OutputArrayOfArrays contours,
                              OutputArray hierarchy, int mode,
                              int method, Point offset=Point());
```

#### **参数详解：**

1. **`image`**，定义为`Mat`，单通道图像矩阵，可以是灰度图，但更常用的是二值图像，一般是经过Canny、拉普拉斯等边缘检测算子处理过的二值图像。

2. **`contours`**，定义为“`vector<vector<Point>> contours`”，是一个向量，并且是一个双重向量，向量内每个元素保存了一组由连续的Point点构成的点的集合的向量，每一组Point点集就是一个轮廓。  有多少轮廓，向量contours就有多少元素。

3. **`hierarchy`**，定义为“`vector<Vec4i> hierarchy`”。

   向量`hiararchy`内的元素和轮廓向量`contours`内的元素是一一对应的，向量的容量相同。`hierarchy`向量内每一个元素的4个`int`型变量——`hierarchy[i][0] ~ hierarchy[i][3]`，分别表示第 i 个轮廓的后一个轮廓、前一个轮廓、父轮廓、内嵌轮廓的索引编号。如果当前轮廓没有对应的后一个轮廓、前一个轮廓、父轮廓或内嵌轮廓的话，则`hierarchy[i][0] ~ hierarchy[i][3]`的相应位被设置为默认值-1。   

   >来看一下Vec4i的定义：
   >
   >typedef    Vec<int, 4>   Vec4i;    
   >
   >Vec4i是Vec<int,4>的别名，定义了一个“向量内每一个元素包含了4个int型变量”的向量。                                                                                                                                
   >
   >所以从定义上看，hierarchy也是一个向量，向量内每个元素保存了一个包含4个int整型的数组。

4. **`mode`**，int型，定义轮廓的检索模式：
   - 取值一：CV_RETR_EXTERNAL，只检测最外围轮廓，包含在外围轮廓内的内围轮廓被忽略
   - 取值二：CV_RETR_LIST，检测所有的轮廓，包括内围、外围轮廓，但是检测到的轮廓不建立等级关系，彼此之间独立，没有等级关系，这就意味着这个检索模式下不存在父轮廓或内嵌轮廓，所以hierarchy向量内所有元素的第3、第4个分量都会被置为-1，具体下文会讲到
   - 取值三：CV_RETR_CCOMP，检测所有的轮廓，但所有轮廓只建立两个等级关系，外围为顶层，若外围内的内围轮廓还包含了其他的轮廓信息，则内围内的所有轮廓均归属于顶层
   - 取值四：CV_RETR_TREE，检测所有轮廓，所有轮廓建立一个等级树结构。外层轮廓包含内层轮廓，内层轮廓还可以继续包含内嵌轮廓。


5. **`method`**，int型，定义轮廓的近似方法：
   - 取值一：CV_CHAIN_APPROX_NONE 保存物体边界上所有连续的轮廓点到contours向量内
   - 取值二：CV_CHAIN_APPROX_SIMPLE 仅保存轮廓的拐点信息，把所有轮廓拐点处的点保存入contours向量内，拐点与拐点之间直线段上的信息点不予保留
   - 取值三和四：CV_CHAIN_APPROX_TC89_L1，CV_CHAIN_APPROX_TC89_KCOS使用teh-Chinl chain 近似算法


6. **`Point`**，偏移量，所有的轮廓信息相对于原始图像对应点的偏移量，相当于在每一个检测出的轮廓点上加上该偏移量，并且Point还可以是负值！

## 图像对比

### 1 直方图

##### **方法描述**：

有两幅图像patch(当然也可是整幅图像)，分别计算两幅图像的直方图，并将直方图进行归一化，然后按照某种距离度量的标准进行相似度的测量。

##### **方法的思想**：

基于简单的向量相似度来对图像相似度进行度量。

##### 优点：

直方图能够很好的归一化，比如256个bin条，那么即使是不同分辨率的图像都可以直接通过其直方图来计算相似度，计算量适中。比较适合描述难以自动分割的图像。

##### 缺点：

直方图反应的是图像灰度值得概率分布，并没有图像的空间位置信息在里面，因此，常常出现误判；从信息论来讲，通过直方图转换，信息丢失量较大，因此单一的通过直方图进行匹配显得有点力不从心。

### 2 图像模板匹配

> （包括多种算法，例如平均绝对差算法（MAD）、绝对误差和算法（SAD）、误差平方和算法（SSD）、平均误差平方和算法（MSD）、归一化积相关算法（NCC）、序贯相似性检测算法（SSDA）、hadamard变换算法（SATD），具体可以参考该blog：https://blog.csdn.net/KYJL888/article/details/81480494）
> 一般而言，源图像与模板图像patch尺寸一样的话，可以直接使用上面介绍的图像相似度测量的方法；如果源图像与模板图像尺寸不一样，通常需要进行滑动匹配窗口（从左到右），扫面个整幅图像获得最好的匹配patch。
> 在OpenCV中对应的函数为：matchTemplate()：函数功能是在输入图像中滑动窗口寻找各个位置与模板图像patch的相似度。

### 3 MSE均方差

##### **计算公式：**

 ![image-20191107114754230](/Users/baola/Library/Application Support/typora-user-images/image-20191107114754230.png)

##### **实现：**

```python
mse = (np.abs(im1 - im2) ** 2).mean()
```



### 4 PSNR峰值信噪比

##### **方法描述：**

PSNR（Peak Signal to Noise Ratio），一种全参考的图像质量评价指标。

##### **方法优缺点：**

PSNR是最普遍和使用最为广泛的一种图像客观评价指标，然而它是基于对应像素点间的误差，即基于误差敏感的图像质量评价。由于并未考虑到人眼的视觉特性（人眼对空间频率较低的对比差异敏感度较高，人眼对亮度对比差异的敏感度较色度高，人眼对一个区域的感知结果会受到其周围邻近区域的影响等），因而经常出现评价结果与人的主观感觉不一致的情况。PSNR算法简单，检查的速度也很快。但是其呈现的差异值有时候和人的主观感受不成比例。

##### **计算公式：**

 ![image-20191107114754230](/Users/baola/Library/Application Support/typora-user-images/image-20191107114754230.png)

 ![image-20191107114804797](/Users/baola/Library/Application Support/typora-user-images/image-20191107114804797.png)

MSE表示当前图像X和参考图像Y的均方误差（MeanSquare Error），H、W分别为图像的高度和宽度；n为每像素的比特数，一般取8，即像素灰阶数为256. PSNR的单位是dB，数值越大表示失真越小。

##### **指标值：**

psnr越大，图像越接近

> - 高于40dB说明图像质量极好（即非常接近原始图像），
>
> - 在30—40dB通常表示图像质量是好的（即失真可以察觉但可以接受），
>
> - 在20—30dB说明图像质量差，
>
> - 低于20dB图像不可接受。

##### **实现：**

```python
  def cal_psnr(im1, im2):
      mse = (np.abs(im1 - im2) ** 2).mean()
      psnr = 10 * np.log10(255 * 255 / mse)
      return psnr
```

### 5 SSIM结构相似性

##### **方法描述：**

也是一种全参考的图像质量评价指标，它分别从亮度 luminance、对比度 contrast、结构 structure 三方面度量图像相似性。

##### **计算公式：**

 ![image-20191107124327864](/Users/baola/Library/Application Support/typora-user-images/image-20191107124327864.png)

##### **指标值：**

SSIM取值范围[0,1]，值越大，表示图像失真越小。

##### **MSSIM平均结构相似性：**

在实际应用中，可以利用滑动窗将图像分块，令分块总数为N，考虑到窗口形状对分块的影响，采用高斯加权计算每一窗口的均值、方差以及协方差，然后计算对应块的结构相似度SSIM，最后将平均值作为两图像的结构相似性度量，即平均结构相似性MSSIM。

##### 实现：

###### ssim（python）

```python
  def cal_ssim(im1,im2):
      assert len(im1.shape) == 2 and len(im2.shape) == 2
      assert im1.shape == im2.shape
      mu1 = im1.mean()
      mu2 = im2.mean()
      sigma1 = np.sqrt(((im1 - mu1) ** 2).mean())
      sigma2 = np.sqrt(((im2 - mu2) ** 2).mean())
      sigma12 = ((im1 - mu1) * (im2 - mu2)).mean()
      k1, k2, L = 0.01, 0.03, 255
      C1 = (k1*L) ** 2
      C2 = (k2*L) ** 2
      C3 = C2/2
      l12 = (2*mu1*mu2 + C1)/(mu1 ** 2 + mu2 ** 2 + C1)
      c12 = (2*sigma1*sigma2 + C2)/(sigma1 ** 2 + sigma2 ** 2 + C2)
      s12 = (sigma12 + C3)/(sigma1*sigma2 + C3)
      ssim = l12 * c12 * s12
      return ssim

```

###### mssim（C#）

```C#
public class SSIMResult
    {
        public double score {
            get
            {
                return (mssim.Val0 + mssim.Val1 + mssim.Val2) / 3;
            }
        }
        public Scalar mssim;
        public Mat diff;
    }
 
 
public static SSIMResult getMSSIM(Mat i1, Mat i2)
        {
            const double C1 = 6.5025, C2 = 58.5225;
            /***************************** INITS **********************************/
            MatType d = MatType.CV_32F;
 
            Mat I1 = new Mat(), I2 = new Mat();
            i1.ConvertTo(I1, d);           // cannot calculate on one byte large values
            i2.ConvertTo(I2, d);
 
            Mat I2_2 = I2.Mul(I2);        // I2^2
            Mat I1_2 = I1.Mul(I1);        // I1^2
            Mat I1_I2 = I1.Mul(I2);        // I1 * I2
 
            /***********************PRELIMINARY COMPUTING ******************************/
 
            Mat mu1 = new Mat(), mu2 = new Mat();   //
            Cv2.GaussianBlur(I1, mu1, new OpenCvSharp.Size(11, 11), 1.5);
            Cv2.GaussianBlur(I2, mu2, new OpenCvSharp.Size(11, 11), 1.5);
 
            Mat mu1_2 = mu1.Mul(mu1);
            Mat mu2_2 = mu2.Mul(mu2);
            Mat mu1_mu2 = mu1.Mul(mu2);
 
            Mat sigma1_2 = new Mat(), sigma2_2 = new Mat(), sigma12 = new Mat();
 
            Cv2.GaussianBlur(I1_2, sigma1_2, new OpenCvSharp.Size(11, 11), 1.5);
            sigma1_2 -= mu1_2;
 
            Cv2.GaussianBlur(I2_2, sigma2_2, new OpenCvSharp.Size(11, 11), 1.5);
            sigma2_2 -= mu2_2;
 
            Cv2.GaussianBlur(I1_I2, sigma12, new OpenCvSharp.Size(11, 11), 1.5);
            sigma12 -= mu1_mu2;
 
            ///////////////////////////////// FORMULA ////////////////////////////////
            Mat t1, t2, t3;
 
            t1 = 2 * mu1_mu2 + C1;
            t2 = 2 * sigma12 + C2;
            t3 = t1.Mul(t2);              // t3 = ((2*mu1_mu2 + C1).*(2*sigma12 + C2))
 
            t1 = mu1_2 + mu2_2 + C1;
            t2 = sigma1_2 + sigma2_2 + C2;
            t1 = t1.Mul(t2);               // t1 =((mu1_2 + mu2_2 + C1).*(sigma1_2 + sigma2_2 + C2))
 
            Mat ssim_map = new Mat();
            Cv2.Divide(t3, t1, ssim_map);      // ssim_map =  t3./t1;
 
            Scalar mssim = Cv2.Mean(ssim_map);// mssim = average of ssim map
 
 
 
            SSIMResult result = new SSIMResult();
            result.diff = ssim_map;
            result.mssim = mssim;
 
 
            return result;
        }
```



### 6 感知哈希算法

感知哈希（Perceptual Hash, PHash）比均值哈希要稳健，PHash使用DCT将图像由空域转为频域，并对频域的低频成分进行散列化。PHash算法可分为以下几个步骤：

1. 将图片resize到32*32最好，这样可以简化DCT计算；
2. 将彩色图像转为灰度图像；
3. 计算DCT，使用32*32的DCT变换；
4. DCT的结果是32*32的矩阵，只要保留左上角的8*8矩阵就可以了，因为这部分呈现了图片中的最低频率；
5. 计算DCT的平均值；
6. 根据8*8的DCT矩阵来与平均值进行比较，大于平均值的为1，小于的为0。

> 具体可见新更新的感知哈希算法（https://www.jianshu.com/p/ad7131f7999b）
> (perceptual hash algorithm）
> http://blog.csdn.net/fengbingchun/article/details/42153261
> 感知哈希算法(perceptual hash algorithm)，它的作用是对每张图像生成一个“指纹”(fingerprint)字符串，然后比较不同图像的指纹。结果越接近，就说明图像越相似。
> 实现步骤：
>
> 1. 缩小尺寸：将图像缩小到8*8的尺寸，总共64个像素。这一步的作用是去除图像的细节，只保留结构/明暗等基本信息，摒弃不同尺寸/比例带来的图像差异
> 2. 简化色彩：将缩小后的图像，转为64级灰度，即所有像素点总共只有64种颜色；
> 3. 计算平均值：计算所有64个像素的灰度平均值；
> 4. 比较像素的灰度：将每个像素的灰度，与平均值进行比较，大于或等于平均值记为1，小于平均值记为0；
> 5. 计算哈希值：将上一步的比较结果，组合在一起，就构成了一个64位的整数，这就是这张图像的指纹。组合的次序并不重要，只要保证所有图像都采用同样次序就行了；
> 6. 得到指纹以后，就可以对比不同的图像，看看64位中有多少位是不一样的。在理论上，这等同于”汉明距离”(Hamming distance,在信息论中，两个等长字符串之间的汉明距离是两个字符串对应位置的不同字符的个数)。如果不相同的数据位数不超过5，就说明两张图像很相似；如果大于10，就说明这是两张不同的图像。

DCT变换

core.dct()参数

- src 输入浮点数组。
- dst 输出与src大小和类型相同的数组。
- flags 转换标志

### 7 尺度不变特征转换(Scale-invariant feature transform或SIFT)算法

> SIFT算子是把图像中检测到的特征点用一个128维的特征向量进行描述，因此一幅图像经过SIFT算法后表示为一个128维的特征向量集，该特征向量集具有对图像缩放，平移，旋转不变的特征，对于光照、仿射和投影变换也有一定的不变。
> SIFT算法的流程分别为：
> i. 尺度空间极点检测；
> ii. 关键点精确定位；
> iii. 关键点的方向确定；
> iv. 特征向量的生成。
> 具体可以看看该博客：
> https://blog.csdn.net/lhanchao/article/details/52345845

*哈希算法包括均值哈希、感知哈希和差值哈希，OpenCV均提供算法，支持多种语言，另外还有一个javaCV功能也是很强大。

# java-opencv学习笔记

官方文档：https://docs.opencv.org/3.4.1/javadoc/overview-summary.html

