题       目：    基于MATLAB的汽车出入库计时系统           

所修课程名称：           计算机仿真技术           

------

<div align=right>徐浩宇
<div align=right>川师电气2018级
<div align=right>2022.1.20

------

@[TOC](目录)

------





<div align=center>基于MATLAB的汽车出入库计时系统



# 一、设计要求

1、使用MATLAB设计一个车库进出车辆的计时系统。
2、能自动识别进入车辆的车牌。
3、可以对车辆出入时间进行计时计费。
4、提交设计报告。

# 二、题目分析

&emsp;&emsp;随着我国经济快速发展，人口规模不断扩大，汽车保有量不断增加等因素引发了大量的停车需求。停车场指的是提供汽车停放的地方，它的主要功能就是帮助车主保管汽车，因此，停车场对维护城市交通秩序的重要性不言而喻。  

  &emsp;&emsp;目前停车场设施随处可见，包括各种路障、立柱以及亭岗等，停车场设施已经是非常普遍的停车场管理工具了，对城市交通的发展也起到了很大的作用。进入21世纪，中国经过了30年的快速发展，城镇化水平越来越高，人们的生活水平显著提高，汽车拥有量突飞猛进，停车设施的功能也更加完善。如今的停车场集收费管理、车辆图像识别、出入口道闸自动控制等多种功能于一身，进一步满足了人们的停车需求。

&emsp;&emsp;随着汽车数量的增加，城市交通状况日益受到人们的重视，如何进行有效的交通管理更是成为了人们关注的焦点。智能交  通系统通过车辆检测装置对过往的车辆实施检测，提取有关交通数据，达到监控、管理和指挥交通的目的。因此，它已成为世界交通领域研究的重要课题。 车牌识别系统作为智能交通系统的一个重要组成部分，已在高速公路、城市交通和停车场等项目的管理中占有无可取代的重要地位。它在不影响汽车状态的情况下，由计算机自动完成车牌的识别，从而降低交通管理工作的复杂度。

&emsp;&emsp;拟定的课题名称为基于MATLAB 的汽车入库计时系统，带有丰富的人机交互GUI 界面。在传统的车牌识别基础上有所创新。加入出、入库识别，并且实行计算停车时间以及停车费用。整个设计在一个GUI 界面上完成。

# 三、总体方案论证

&emsp;&emsp;车牌识别技术通常包含三大处理流程，它们分别是车牌定位、字符分割和字符识别，本章将对车牌定位和字符分割进行研究。车牌定位是车牌识别系统的重要组成部分，它是要从整幅图像中获得车牌所在的位置，并把它提取出来，以此保证后续的处理--字符分割能顺利完成。字符分割也是车牌识别的重要组成部分，它是对经过车牌定位并经过一系列图像处理后得到的新图像进行字符分割，其任务是将车牌的7个字符分别分割出来。<div align=center>![请添加图片描述](https://img-blog.csdnimg.cn/b5befcc09be846e59a1f8f4c4ae05098.png)<div align=center>图 3.1方案流程图



&emsp;&emsp;车牌定位是指在经过图像预处理之后的灰度图像上进行行列扫描或对彩色图像进行像素统计，来最终确定车牌的有效区域。字符分割指的是在图像中定位出车牌区域后，首先对该区域进行灰度化、二值化等图像处理，然后根据车牌字符的先验知识分割车牌字符。字符识别是对车牌分割后得到的字符进行缩放、特征提取，处理后的字符图像通过模板匹配或神经网络等方法获取到计算机可以存储并处理的编码字符，最终将其以文本的形式输出。

# 四、系统总体设计

&emsp;&emsp;<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/27adf5ad2669458aa0a6de38e88d5c52.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGFubWFudWVzcg==,size_20,color_FFFFFF,t_70,g_se,x_16)<div align=center>图4.1总体设计框图



# 五、各部分定性说明以及定量计算

## 5.1图像采集

&emsp;&emsp;图像采集是该系统的第一步，照片质量的好坏直接关系到系统识别的精度,故选择好的摄像设备,设置好的摄像角度是关键。随着现代社会的发展，数码照相机的分辨率已越来越高，可使用红外传感器来控制照相机的开启与关闭，照相机通过串口通信来传递图片信息给计算机。

## 5.2图像的预处理

&emsp;&emsp;在整个车牌识别系统中，由于采集进来的图像为真彩图，再加上实际采集环境的影响以及采集硬件等原因，图像质量并不高，其背景和噪声会影响字符的正确分割。和识别，所以在进行车牌分割和识别处理之前，需要先对车牌图像进行图像预处理操作。预处理的具体操作是规整大小、噪声滤波，规整为统一大小便于后续处理的参数设置，提高定位精确度及识别正确率。规整大小函数：imresize (I,[row,col] )接着进行图像平滑滤波。RGB 图像的平滑滤波，需要将R，G，B 三个色道分别提取出，分别滤波。这里采用3*3 的中值滤波算子，对三个色道分别滤波，然后使用cat 函数将三色道整合起来。

## 5.3车牌定位

&emsp;&emsp;目前,从事车牌定位有两类技术路线。第一类是在预处理后的灰度图像中实施检测,灰度图像相对于人的视觉而言，是以灰度等级来体现图像的深浅度。车辆图像的背景通常很复杂，背景可能包括生活中的人、树木、建筑物等，然而车牌为一个含有字符的固定区域，其内部含有丰富的边缘，因而车牌部分呈现出规则的纹理特征。第一类包括的主要方法有基于纹理特征法、基于数学形态学法、基于小波分析法等方法。车牌定位的第二类是在彩色图像中检测，由于人的视觉感受更倾向于彩色图像，且彩色图像能够提供更多有用的信息，因此人类可很好地区分上万种不同的颜色。随着计算机技术和图像处理技术的发展，计算机处理彩色图像的能力越来越高，通过彩色图像定位车牌成为近年来车牌定位的研究热点。第二类包括的主要方法有基于RGB 颜色法、基于神经网络法等。

### 5.3.1基于灰度图像的车牌区域定位方法

&emsp;&emsp;目前，通过摄像机、数码相机或手机采集到的图像通常是彩色图像。彩色图像中最常见的是RGB 图像，即每个像素由R、G、B 分量构成的图像，R 表示红色分量，G 表示绿色分量，B 表示蓝色分量，每个分量的取值范围都是0~255 。

&emsp;&emsp;如图所示的RGB 彩色模型，图中的每-一点表示一种颜色，每个点有三种组成部分，分别为红色分量、绿色分量和蓝色分量，分量的亮度值归一化到0到1之间。从模型中可看出，青色中含有绿色和蓝色，不含红色;品红中包含红色以及蓝色，不含绿色;黄色中含有绿色和红色，不含蓝色。在模型中黑与白之间的连线上，所有点的红、绿、蓝三种分量是相等的，从黑色逐渐变为白色。通过红、绿、蓝三分量的不同组合，形成了这个彩色空间。<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/fdc27d2c67cb4efebd3f3ddd029255e5.png)

<div align=center>图5.3.1  RGB 彩色模型

&emsp;&emsp;当R、G、B 三个颜色分量的值相等时，该图像只能表示出--种灰度颜色。灰度图像是指图像中的每个像素只有一个采样颜色的图像。灰度图像中的每个采样像素为8 位，共计256 种灰度，这种精度保留了图像中的大多数特征，因此车牌识别系统中常采用基于灰度图像的车牌区域定位方法。一方面，灰度图像一般占用较少的存储单元;另一方面也降低了对处理器计算能力的要求。在灰度图中仅含亮度信息，不包含彩色信息"5)。通常要实现图像灰度化有如下的三种方法:

（1）像素平均值法。某点的灰度值等于该点的R，G，B 之和的平均值即：
<div align=center>F（i,j）=[R(i,j)+G(i,j)+B(i,j)]/3

（2）像素最大值法。某点的灰度值等于该点的R，G，B值中的最大值，即：
<div align=center>F(i,j)=max{R(i,j),G(i,j),B(i,j)}

（3）加权平均值法。给R，B，G 分别赋予不同的权值a，b，c；a，b，c 的取值范围均为[0.1] ，即：
<div align=center>F(i,j)=a*R(i,j)+b*G(i,j)+c*B(i,j)

&emsp;&emsp;如图是将一张含有车牌的图像用像素平均值法灰度化后的对比效果图<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/8c11a71692c94a2d802c087def86ded3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGFubWFudWVzcg==,size_18,color_FFFFFF,t_70,g_se,x_16)

<div align=center>图5.3.2图像灰度化



### 5.3.2基于彩色图像的车牌区域定位方法

&emsp;&emsp;相比于基于灰度图像的车牌定位，基于彩色图像的车牌定位则无需做大量的图像预处理，比如图像的灰度化、边缘检测、数学形态学处理等。采用彩色图像定位车牌是利用车牌区域的颜色特征，先定位到与车牌颜色相关的区域，然后再利用纹理特征或者结构特征对相关区域进行分析和进一步判断，最终确定车牌区域。该方法不同于大多数的车牌定位方法，因为它对车牌的大小以及汽车在图像中的位置限制较少"。车牌除了具有颜色方面的特征之外，还具有一定的字频统计特征和几何结构特征，只有满足特定的字频统计特征和长宽比特征的区域才是真正的车牌区域。目前根据普通车牌的国家标准规定汽车牌照是有规则的长方形,且尺寸标准为:长440mm ,宽140mm ,长宽比约为3.14 。颜色信息的使用可以提高车牌定位的成功率，本文采用基于RGB  颜色分量的方法进行车牌定位研究。

&emsp;&emsp;传统的方法往往只考虑蓝色分量来对蓝色车牌进行定位，并未考虑其他分量，这样做的结果是找到的车牌区域往往会比较大。本文采用三分量实现对蓝色车牌定位，实验证明三分量获取的车牌区域比较理想。图2-7 所示为基于三分量的车牌定位流程图。<div align=center>![在这里插入图片描述](https://img-blog.csdnimg.cn/64bc317f7c1f4262853306b6a4180bfe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGFubWFudWVzcg==,size_11,color_FFFFFF,t_70,g_se,x_16)



<div align=center>图5.3.3  基于三分量的车牌定位流程图

&emsp;&emsp;参看图5.3.3 ，基于彩色图像的车牌区域定位方法的基本流程为:首先确定蓝色车牌背景色的RGB 各自对应的灰度范围。之后由彩色像素点统计法在行方向上统计出此范围内的蓝色像素点个数，同时设定一个合理的阙值，进而确定出行方向上车牌边界。在列方向上统计出上述灰度范围内的蓝色像素点个数，也设定一个较为合理的阈值，以此确定出列方向上的车牌边界。通过这种扫描的方式确定出车牌的上下左右边界，并对该边界范围内的子图像进行切割，最终分割出待定的车牌区域图像。待定区域是否为最终的车牌定位区域还要结合车牌形状方面的先验知识，即字频统计特征和几何结构特征。因此除了采用三分量进行车牌定位外，还通过车牌的几何结构及字频统计特征对车牌区域进行再判断。车牌定位时将长宽比的范围设置在[1.6,5] 之间(标准值为 3.14 )，预留有一定的裕度，同时将字符面积与车牌区域面积的比例设置在[0.12,0.6] ，即字符面积与车牌区域面积比例的最小值不超过0.12 ，最大值不超过0.6 。结合车牌的字频统计特征、几何结构特征和颜色特征，定位出的车牌区域合理，便于后续处理。<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/d73f2ece841641979aa333051268c10e.png)



<div align=center>图5.3.4 基于彩色图像的车牌定位方法定位到的车牌区域

## 5.4字符分割

&emsp;&emsp;字符分割模块也是整个识别系统中的重要模块之一，字符分割的好坏直接对后续字符识别产生重要的影响，甚至影响字符识别的成败,在车牌识别中起着承上启下的作用。车牌字符分割就是把车牌照图像中的汉字、字母以及阿拉伯数字分割成为一个个独立的单字符，为下一步做好充分的准备。对于清晰的图像，字符分割准确率可以达到90%以上，但是由于摄像机或者照相机在实际拍摄车牌图像时会因存在这样或者那样的问题，进而影响字符分割的准确性。在进行字符分割前还需要对车牌区域进行一定的预处理，比如车牌校正和旋转等。

&emsp;&emsp;首先对车牌进行水平投影，去除水平边框；再对车牌进行垂直投影。通过对车牌进行投影分析可知，与最大值峰中心对应的为车牌中第二个字符和第三个字符的间隔，与第二大峰中心距离对应的即为车牌字符的宽度，并以此为依据对车牌进行分割。

### 5.4.1车牌矫正

&emsp;&emsp;通常情况下，由于拍摄角度不同，致使拍摄的图像有一定的倾斜，特别是车牌照区域发生了定倾斜。具有一定倾斜的图像对后续的字符分割与字符识别都将会造成严重的影响。即使倾斜角度很小，这也将会加大字符分割的难度，而且也会造成一定的偏差，此时系统还可以较准确的去定位车牌区域，但是仍对字符识别构成一定的影响。当倾斜角度较大时，将会分割出不完整的车牌区域，甚至分割出错误的车牌区域，致使整个系统识别失败。因此，在字符分割前，首先对车牌图像进行图像倾斜校正。常用的车牌校正算法有基于Hough 变换的车牌图像倾斜校正算法和基于Radon 变换的车牌图像倾斜校正算法。本次课程设计采用基于Radon 变换的车牌图像倾斜校正算法。

&emsp;&emsp;常用的车牌校正算法有基于Hough 变换的车牌图像倾斜校正算法和基于Radon 变换的车牌图像倾斜校正算法。本次课程设计采用基于Radon 变换的车牌图像倾斜校正算法。





### 5.4.2插值算法

&emsp;&emsp;倾斜校正之后还需要对图像进行旋转，当图像旋转之后会出现许多空洞点，对这些空洞点必须进行填充处理，否则画面效果不好，通常称这种操作为插值处理。常采用的插值方法有最近邻插值法、双线性插值法和三次插值法R 。



（1）双线性插值
&emsp;&emsp;在双线性插值算法当中,用原图像中点(x0,y0) 临近的四个网格点(m,n),(m+1,n),(m,m+1) 按照如所示的公式来计算归一化图像中点（x1,y1） 处的灰度值P(x1,y1) 。

<div align=center>p(x1,y1)=f(m.n)×(1-a)×(1-b)+f(m+1,n)×a×(1-b)

<div align=center>f(m,n+1)×(1-a)×b+f(m+1,n+1)×a×b

式中，m,n 分别为x0,y0 取整之后的值，a=x0-m,b=y0-m,f(m.n) 为点（m,n） 处的像素值，f(m+1,n) 为点（m+1,n） 处的像素值，f(m,n+1) 为点(m.n+1) 处的像素值，f(m+1,n+1) 为点(m+1,n+1) 处的像素值。

（2）三次插值
&emsp;&emsp;在三次插值算法当中,用原图像中点(x0,y0) 邻近的 16个网格点来近似归一化图像中点（x1,y1） 处p(x1,y1) 的灰度值。虽然此算法精度相当高，但是此算法相当复杂，而且计算量相当大。

（3）临近插值
&emsp;&emsp;在邻近插值算法中,用原图像中点(x,y) 邻近的四个网格点(m, n), ( m +1, n), ( m,n +1) ,( m+1, n+1) 最接近归一化图像中（x1,y1） 点处的灰度值f(x0',y0') 赋值给p(x1,y1) ，它的数学表达式如下所示。

<div align=center>p(x1,y1)=f(x0',y0')

&emsp;&emsp;式中点(x0,y0) 表示点(x0,y0) 与其相邻近的四个网格点最近的那个网格点。从系统的执行速度、算法的复杂度、系统运行的时间以及实际的效果方面综合考虑，本文采用双线性插值法，该方法实现简单而且能满足实际的效果要求，图2-9所示为图2-8经过Radon 变换和双线性插值后的车牌校正图像。<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/716e6afbb16942d29837b0a74fe045b2.png)<div align=center>图5.4.1车牌矫正图像

## 5.5车牌识别

&emsp;&emsp;经过前面的车牌定位与分割以后，车牌的字符已经被分割成了多个单一字符，现在到了最为关键的一步——字符识别，它处理的好坏将会直接影响到整个车牌识别的识别效果。字符识别是将字符通过计算机识别出来，并标明其所属类别的问题，属于模式识别。模式识别技术是用机器来模拟人的各种识别能力，图像模式识别是用机器对图像、图片等模式信息加以处理和识别。它综合了人工智能、数字图像处理等学科，是当今的研究热点。目前常用的字符识别方法有：模板匹配字符识别方法、人工神经网络字符识别方法、基于字符特征的字符识别方法和基于统计分类器的字符识别方法。

&emsp;&emsp;字符识别需要有相应的模板库保存下分割后的字符图像，开始进行字符识别。第一个字符为行省简称，其余六个字符为数字与字母的组合，据此分别构建行省简称字符模板与数字、字母字符模板，供识别匹配备用。字符模板制作统一为40*20 的二值图像，利用模字制作软件，制模后保存在同一个文件夹中以备用识别过程中，首先建立标准字库，再将分割所得到的字符进行归一化，将归一化处理后的字符与标准字库里的字符逐一比较，最后把误差最小的字符作为结果显示出来。<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/46b45a7929924025be6082c569df92b7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGFubWFudWVzcg==,size_17,color_FFFFFF,t_70,g_se,x_16)
<div align=center>图5.5.1二值图像

### 5.5.1车牌字符识别的特点

车牌字符识别的特点有:

（1）针对特定字符集

&emsp;&emsp;国内车辆牌照一般为中文字符、英文字符（不包含大写字母“O”和“I”)以及数字字符所组成，字符集里含有的元素只有几十个。

（2）待识别的字符图像相对小
&emsp;&emsp;由于车牌的大小通常只占采集图像的小部分，而在我国车牌的第一个字符为中文字符，由于中文字符结构相对复杂，形状多变，字符的像素较少就会给识别带来了更大的难度，甚至导致字符识别的失败。

（3）干扰噪声多

&emsp;&emsp;由于天气、光线的变化较多，会导致车牌识别系统受到的噪声干扰较多，拍摄的图像很可能非常模糊甚至是破坏，这就要求车牌字符识别具有更好的抗噪声能力，能够根据周围的环境做出改变。

（4）存在结构特征相似的字符

&emsp;&emsp;由于车牌字符会同时出现英文字符和数字字符，某些数字字符和英文字符具有相同的特征，从而增加了的字符识别的难度。比如英文字符"D" 和数字字符”0” ，数字字符"8" 和英文字符"B" ，数字字符"5" 和英文字符"S" ，英文字符"Z" 和数字字符"2" 等。



### 5.5.2基于模板匹配的车牌字符识别方法

&emsp;&emsp;模板匹配法是一种直观的分类算法，就是用不同字符的标准字形建立模板，然后把要识别的字符与其逐一比较，相似程度最大的作为识别结果，模板匹配法与人工神经网络字符识别方法相比具有识别速度快、方法简单等优点。为了实现模板匹配，首先要选择一些样本字符图像建立相关的字符模板库，当进行字符判定时，将每个待识别的二值化车牌字符图像进行归一化处理，将待识别字符图像缩放到模板字符同样的大小，然后计算待识别字符与模板库中样本字符特征点之间的距离，找出特征距离最小的模板，其所对应的输出值就是期望的识别结果。

&emsp;&emsp;模板匹配法算法简单，容易实现，对图像质量、模板大小比较敏感，实际系统中通常采用一个字符的多个模板或面积较大的模板以提高匹配识别的精度，但模板面积和数量的增加就会降低匹配效率。



## 5.6汽车出入库计时计费

&emsp;&emsp;<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/1dcf9807b197413e8ea5c2fbcbca1f7e.png#pic_center)

<div align=center>
图5.6.1 出入库计时计费流程图



 1、记录汽车入库时间，车库位数减一；
2、记录汽车出库时间，车库位数加一；
3、计算停车时长，按标准计算停车费用；





# 六、系统软件设计

&emsp;&emsp;停车场管理系统软件设计部分包括图形用户界面设计、数据库设计。图形用户界面作为一个传达信息的工具，如计算机软件界面以及网页设计等等。“人机界面”的范围很宽泛，凡事人与机器进行信息交流的一切领域都应归于人机界面的范畴。界面设计是开发中非常重要的环节，优秀的界面设计通常是设计者预见的过程，设计的界面是开发者需要根据用户需求而设计的。同时优秀的界面通常简单且是用户乐于接受的。<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b24615a54bf4c6490a900eb3aec7cae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGFubWFudWVzcg==,size_9,color_FFFFFF,t_70,g_se,x_16)
<div align=center>
图6.1 软件总体编写框架



<div align=center>若采用SQL Server 数据库作为数据库，系统可以需要记录车辆的车牌号、进场时间、出场时间、单价、应收费用。但本课程设计考虑到时间问题，并未采用数据库作为数据存放点。作为理想情况处理，只实现单独一辆车的识别进出计时计费。



## 6.1 MATLAB GUI简介

&emsp;&emsp;MATLAB 是 Mathworks 公司开发的，主要用于科学计算，它具有运行稳定、可靠、使用方便等优点，是科学界研究工作的得力助手。MATLAB 不仅拥有强大的科学计算能力，而且还具有界面设计开发的功能。MATLAB GUI (Graphic User InterFace) 就是内置于MATLAB 的进行图形界面开发的模块。如果用户想要快捷地设计出满足要求的图形用户界面，就必须遵循MATLAB GUI 的设计原则和一般步骤，并且明确GUI 实现方式。



## 6.2 GUI的设计原则

&emsp;&emsp;在MATLAB 2016b 中，GUI 的设计原则与-一般的动态界面设计原则基本上是一致的，它们都强调以下几点:

(1）功能优先

&emsp;&emsp;GUI 坚持将实现界面程序的功能放在第一位，GUI 更多的是关注用户的需求，设计的任务便于用户理解，其次才是为了表示方便或者界面美观而削弱部分功能。

(2）隐藏功能的实现细节

&emsp;&emsp;图形用户界面的特点是避免用户直接接触到繁琐原代码，只需通过简单的点击或输入即可实现界面的功能，GUI 在这方面和VB 比较类似，均没有公布程序具体的执行细节。

(3)用户任务简单

&emsp;&emsp;这一原则主要是希望GUI 设计者不要把任务设计得过大或过于复杂，因为，这将不利于用户快速了解任务的功能。

(4)保持用户使用习惯的--致性

&emsp;&emsp;图形用户界面程序的一致性主要指的是图形界面的控件的位置、大小设置、以及按钮设置等风格与用户的使用习惯相一致。

(5）设计应满足响应需求

&emsp;&emsp;整个GUI 设计要满足程序的需求，程序不能忽视或设计成某个控件没有响应，而且响应必须是和预期一致的，出现的响应必须是--致对应的结果，保持程序的健壮性。



## 6.3 GUI设计的一般步骤

图形用户界面设计的步骤一般包括以下几点:

1、设计用户界面大致风格。
2、添加用户界面程序需要的组件。
3、设置各组件的属性。
4、编写回调函数。
5、调试程序。



## 6.4 GUI实现

&emsp;&emsp;采用MATLAB 自带 GUI 设计工具GUIDE 的好处是容易入手，风格有点像VB ，相关的控件可以用拖拽的方式选出，它们的位置和大小也可以像拖Windows 一样方便。GUIDE 生成的是一个fig  文件，同时还会生成一个包含fig 中放置控件的相关回调函数的M 脚本。使用全脚本的好处是能够在使用过程中了解MATLAB 里句柄函数的参数传递，可以更直观而快速地掌握GUI 设计的技巧。使用的M 文件代码可以重复使用，也可以生成非常复杂的界面，可将创建对象代码和动作执行代码很好的结合起来。



## 6.5 GUI设计图

&emsp;&emsp;<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/a7697aad18124971ae0945f992d47fbc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGFubWFudWVzcg==,size_18,color_FFFFFF,t_70,g_se,x_16)

 

<div align=center>
图6.5.1  GUI 操作界面



&emsp;&emsp;左侧为入库信息列表，入库时识别车牌信息，并显示当前时间日期，即作为入库的时间。右侧为出库信息列表，出库时识别车牌信息，并显示当前时间日期，作为出库的时间。出库时间减去入库时间，即作为计费依据。<div align=center>
![在这里插入图片描述](https://img-blog.csdnimg.cn/f6559ef779a44e0f8f3a6abaff6962e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGFubWFudWVzcg==,size_18,color_FFFFFF,t_70,g_se,x_16)



<div align=center>
图6.5.2  GUI 界面实际工作图



&emsp;&emsp;为了方便现场延时，初始设计计费价格为5元每分钟，不足1分钟不计费。

# 七、设计心得体会

经过这次课程设计，我觉得自己学到了不少东西。归纳起来，主要有以下几点:

&emsp;&emsp;1、设计需要用到的都是学过的知识。在做设计的过程中需要的是耐心和细心。我们在发现问题的时候需要的是认真观察程序，有时候会不小心输入中文的符号，这样就会导致错误，但是有不容易发现错误的所在。本次程序设计的时候需要用到的最重要的是多重图像的识别，计时工具和计算工具的调用。总之，本次的实验需要我们认真，这就教会了我:我们要认真做好每一件事，出现错误时也不要着急，要认真检查，一步一个脚印地发现错误。

&emsp;&emsp;2、MATLAB不仅具有强大的数值运算和符号计算功能，同时还具有非常强大的二维和三维绘图功能，尤其擅长于各种科学运算结果的可视化界面的展示。计算的可视化可以将杂乱的数据通过图形表示来从中观察出其内在的关系。由于某些版本的MATLAB可能与电脑不兼容，所以在安装MATLAB时应该注意设置好电脑对此软件的兼容性。由于MATLAB函数众多，而且课本上提供的都是最基本的函数功能，自己不仅要去图书馆借这方面的专业书籍来阅读，而且许多函数的编写都要用到C语言，对C语言也有一定的要求。

&emsp;&emsp;3、通过本次课程设计，使自己对MATLAB GUI  设计流程有了比较深刻的体会，同时也了解了一般软件设计的过程。在设计过程中碰到了很多的问题，通过这些问题，使自己分析问题，解决问题的能力得到了较大的提高。我们也明白了学无止尽的道理，在我们所查的很多参考书中，很多知识是我们从没有接触过的，我们对它的了解还仅限于皮毛，对它的很多功能以及函数还不是很了解，所以在这个学习的过程中我们穿越在知识的海洋中，一点一点吸取着它的知识。在MATLAB 编程中需要很多的参考书，要尽量多的熟悉MATLAB 自带的函数及其作用，因为 MATLAB 的自带函数特别多，基本上能够满足一般的数据和矩阵的计算，所以基本上不用你自己编函数。这一点对程序非常有帮助，可以使程序简单，运行效率高，可以节省很多时间。本次课设中用了很多MATLAB 自带的函数，使程序变得很简单。

# 参考文献:

[1] 杨渝，陈瑶，苏昆等.公路路面无人值守超限车辆自动识别系统设计[J].机械与电子，2017，35（4）：61-64

[2] 张小凤，刘向阳.基于图像超像素分析的图像分割方法[J].计算机技术与发展，2018，28（7）：25-28

[3] 杨丹，赵海滨，龙哲等.MATLAB图像处理实例详解[M].北京：清华大学出版社，2013.7

[4] 阳平华，吴丽镐，李菁，詹涌强，阳彩霞.MATLAB R2018基础与实例教程[M].北京：机械工业出版社，2019.2

[5] 马婉捷.车牌识别系统中字符分割的研究与实现[D].复旦大学，2009

[6] 聂洪印.车牌识别系统中关键算法的研究与实现[D]. 山东大学，2009

# 附录：

该课程设计gui.m 部分程序源码及注释如下：
	附录：

```
该课程设计gui.m 部分程序源码及注释如下：
function varargout = gui(varargin)
gui_Singleton = 1;
gui_State = struct('gui_Name',       mfilename, ...
                   'gui_Singleton',  gui_Singleton, ...
                   'gui_OpeningFcn', @gui_OpeningFcn, ...
                   'gui_OutputFcn',  @gui_OutputFcn, ...
                   'gui_LayoutFcn',  [] , ...
                   'gui_Callback',   []);
if nargin && ischar(varargin{1})
    gui_State.gui_Callback = str2func(varargin{1});
end
if nargout
    [varargout{1nargout}] = gui_mainfcn(gui_State, varargin{});
else
    gui_mainfcn(gui_State, varargin{});
end
%结束初始化
function gui_OpeningFcn(hObject, eventdata, handles, varargin)
handles.output = hObject;
guidata(hObject, handles);
function varargout = gui_OutputFcn(hObject, eventdata, handles) 
varargout{1} = handles.output;
% ======================输入图像===============================
function pushbutton1_Callback(hObject, eventdata, handles)
[filename pathname]=uigetfile({'.jpg';'.bmp'}, 'File Selector');
I=imread([pathname '' filename]);
handles.I=I;
guidata(hObject, handles);
axes(handles.axes1);
imshow(I);title('原图');

% ======================图像处理===============================
function pushbutton2_Callback(hObject, eventdata, handles)
I=handles.I;
I1=rgb2gray(I);
I2=edge(I1,'roberts',0.18,'both');
axes(handles.axes2);
imshow(I1);title('灰度图');
axes(handles.axes3);
imshow(I2);title('边缘检测');
se=[1;1;1];
I3=imerode(I2,se);%腐蚀操作
se=strel('rectangle',[25,25]);
I4=imclose(I3,se);%图像聚类，填充图像
I5=bwareaopen(I4,2000);%去除聚团灰度值小于2000的部分
[y,x,z]=size(I5);%返回15各维的尺寸，存储在x,y,z中
myI=double(I5);
tic      %tic计时开始，toc结束
Blue_y=zeros(y,1);%产生一个y1的零针
for i=1y
    for j=1x
        if(myI(i,j,1)==1)%如果myI图像坐标为（i，j）点值为1，即背景颜色为蓝色，blue加一
            Blue_y(i,1)=Blue_y(i,1)+1;%蓝色像素点统计
        end
    end
end
[temp MaxY]=max(Blue_y);
%Y方向车牌区域确定
%temp为向量yellow_y的元素中的最大值，MaxY为该值得索引
PY1=MaxY;
while((Blue_y(PY1,1)=5)&&(PY11))
    PY1=PY1-1;
end
PY2=MaxY;
while((Blue_y(PY2,1)=5)&&(PY2y))
    PY2=PY2+1;
end
IY=I(PY1PY2,,);
%X方向车牌区域确定
Blue_x=zeros(1,x);%进一步确认x方向的车牌区域
for j=1x
    for i=PY1PY2
        if(myI(i,j,1)==1)
            Blue_x(1,j)=Blue_x(1,j)+1;
        end
    end
end
PX1=1;
while((Blue_x(1,PX1)3)&&(PX1x))
    PX1=PX1+1;
end
PX2=x;
while((Blue_x(1,PX2)3)&&(PX2PX1))
    PX2=PX2-1;
end
PX1=PX1-1;%对车牌区域的矫正
PX2=PX2+1;
dw=I(PY1PY2-8,PX1PX2,);
t=toc;
axes(handles.axes4);imshow(dw),title('定位车牌');
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
imwrite(dw,'dw.jpg');%将彩色车牌写入dw文件中
a=imread('dw.jpg');%读取车牌
b=rgb2gray(a);%将车牌图像转换为灰度图
imwrite(b,'灰度车牌.jpg');%将灰度图写入文件
g_max=double(max(max(b)));
g_min=double(min(min(b)));
T=round(g_max-(g_max-g_min)3);%T为二值化的阈值
[m,n]=size(b);
d=(double(b)=T);%d二值图像
imwrite(d,'二值化.jpg');
%均值滤波前
%滤波
h=fspecial('average',3);
%建立预定义的滤波算子，average为均值滤波，模板尺寸为33
d=im2bw(round(filter2(h,d)));%使用指定的滤波器h对h进行d即均值滤波
imwrite(d,'均值滤波.jpg');
%某些图像进行操作
%膨胀或腐蚀
se=eye(2);%单位矩阵
[m,n]=size(d);%返回信息矩阵
if bwarea(d)mn=0.365%计算二值图像中对象的总面积与整个面积的比是否大于0.365
    d=imerode(d,se);%如果大于0.365则进行腐蚀
elseif bwarea(d)mn=0.235%计算二值图像中对象的总面积与整个面积的比值是否小于0.235
    d=imdilate(d,se);%%如果小于则实现膨胀操作
end
imwrite(d,'膨胀.jpg');
%寻找连续有文字的块，若长度大于某阈值，则认为该块有两个字符组成，需要分割
d=qiege(d);
[m,n]=size(d);
k1=1;
k2=1;
s=sum(d);
j=1;
while j~=n
    while s(j)==0
        j=j+1;
    end
    k1=j;
    while s(j)~=0 && j=n-1
        j=j+1;
    end
    k2=j-1;
    if k2-k1=round(n6.5)
        [val,num]=min(sum(d(,[k1+5k2-5])));
        d(,k1+num+5)=0;%分割
    end
end
%再切割
d=qiege(d);
%切割出7个字符
y1=10;
y2=0.25;
flag=0;
word1=[];
while flag==0
    [m,n]=size(d);
    left=1;
    wide=0;
    while sum(d(,wide+1))~=0
        wide=wide+1;
    end
    if widey1 %认为是左干扰 
        d(,[1wide])=0;
        d=qiege(d);
    else
        temp=qiege(imcrop(d,[1 1 wide m]));
        [m,n]=size(temp);
        all=sum(sum(temp));
        two_thirds=sum(sum(temp([round(m3)2round(m3)],)));
        if two_thirdsally2
            flag=1;word1=temp;%word1
        end
        d(,[1wide])=0;d=qiege(d);
    end
end
%分割出第二至七个字符
[word2,d]=getword(d);
[word3,d]=getword(d);
[word4,d]=getword(d);
[word5,d]=getword(d);
[word6,d]=getword(d);
[word7,d]=getword(d);
[m,n]=size(word1);
%商用系统程序中归一化大小为4020
word1=imresize(word1,[40 20]);
word2=imresize(word2,[40 20]);
word3=imresize(word3,[40 20]);
word4=imresize(word4,[40 20]);
word5=imresize(word5,[40 20]);
word6=imresize(word6,[40 20]);
word7=imresize(word7,[40 20]);
axes(handles.axes5);imshow(word1),title('1');
axes(handles.axes6);imshow(word2),title('2');
axes(handles.axes7);imshow(word3),title('3');
axes(handles.axes8);imshow(word4),title('4');
axes(handles.axes9);imshow(word5),title('5');
axes(handles.axes10);imshow(word6),title('6');
axes(handles.axes11);imshow(word7),title('7');
imwrite(word1,'1.jpg');
imwrite(word2,'2.jpg');
imwrite(word3,'3.jpg');
imwrite(word4,'4.jpg');
imwrite(word5,'5.jpg');
imwrite(word6,'6.jpg');
imwrite(word7,'7.jpg');
liccode=char(['0''9' 'A''Z' '辽粤豫鄂鲁陕京津']);%京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼
SubBw2=zeros(40,20);
l=1;
for I=17;
    ii=int2str(I);
    t=imread([ii,'.jpg']);
    SegBw2=imresize(t,[40 20],'nearest');
    SegBw2=double(SegBw2)20;
    if l==1 %第一位汉字识别
        kmin=37;
        kmax=43;
    elseif l==2 %第二位字母识别
        kmin=11;
        kmax=36;
    else l=3   %第三位后字母或数字识别
        kmin=1;
        kmax=36;
    end
    for k2=kminkmax
        fname=strcat('字符模板',liccode(k2),'.jpg');
        SamBw2=imread(fname);
        SamBw2=double(SamBw2)1;
        for i=140
            for j=120
                SubBw2(i,j)=SegBw2(i,j)-SamBw2(i,j);
            end
        end
        %相当于两幅图相减得第三幅图
        Dmax=0;
        for k1=140;
            for l1=120
                if(SubBw2(k1,l1)0  SubBw2(k1,l1)0)
                    Dmax=Dmax+1;
                end
            end
        end
        Error(k2)=Dmax;
    end
    Error1=Error(kminkmax);
    MinError=min(Error1);
    findc=find(Error1==MinError);
    Code(l2-1)=liccode(findc(1)+kmin-1);
    Code(l2)=' ';
    l=l+1;
end
axes(handles.axes12);imshow(dw),title(['车牌号码：',Code],'Color','b');
axes(handles.axes13);imhist(I1);title('灰度化直方图');
handles.starttime = datetime;
set(handles.text1,'String',datestr(datetime))
guidata(hObject, handles);

% ==========================退出系统============================
function pushbutton3_Callback(hObject, eventdata, handles)
close(gcf);

% --- Executes on button press in pushbutton4.
function pushbutton4_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton4 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
[filename pathname]=uigetfile({'.jpg';'.bmp'}, 'File Selector');
I=imread([pathname '' filename]);
handles.I=I;
guidata(hObject, handles);
axes(handles.axes25);
imshow(I);title('原图');

% --- Executes on button press in pushbutton5.
function pushbutton5_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton5 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
I=handles.I;
I1=rgb2gray(I);
I2=edge(I1,'roberts',0.18,'both');
axes(handles.axes14);
imshow(I1);title('灰度图');
axes(handles.axes15);
imshow(I2);title('边缘检测');
se=[1;1;1];
I3=imerode(I2,se);%腐蚀操作
se=strel('rectangle',[25,25]);
I4=imclose(I3,se);%图像聚类，填充图像
I5=bwareaopen(I4,2000);%去除聚团灰度值小于2000的部分
[y,x,z]=size(I5);%返回15各维的尺寸，存储在x,y,z中
myI=double(I5);
tic      %tic计时开始，toc结束
Blue_y=zeros(y,1);%产生一个y1的零针
for i=1y
    for j=1x
        if(myI(i,j,1)==1)%如果myI图像坐标为（i，j）点值为1，即背景颜色为蓝色，blue加一
            Blue_y(i,1)=Blue_y(i,1)+1;%蓝色像素点统计
        end
    end
end
[temp MaxY]=max(Blue_y);
%Y方向车牌区域确定
%temp为向量yellow_y的元素中的最大值，MaxY为该值得索引
PY1=MaxY;
while((Blue_y(PY1,1)=5)&&(PY11))
    PY1=PY1-1;
end
PY2=MaxY;
while((Blue_y(PY2,1)=5)&&(PY2y))
    PY2=PY2+1;
end
IY=I(PY1PY2,,);
%X方向车牌区域确定
Blue_x=zeros(1,x);%进一步确认x方向的车牌区域
for j=1x
    for i=PY1PY2
        if(myI(i,j,1)==1)
            Blue_x(1,j)=Blue_x(1,j)+1;
        end
    end
end
PX1=1;
while((Blue_x(1,PX1)3)&&(PX1x))
    PX1=PX1+1;
end
PX2=x;
while((Blue_x(1,PX2)3)&&(PX2PX1))
    PX2=PX2-1;
end
PX1=PX1-1;%对车牌区域的矫正
PX2=PX2+1;
dw=I(PY1PY2-8,PX1PX2,);
t=toc;
axes(handles.axes16);imshow(dw),title('定位车牌');
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
imwrite(dw,'dw.jpg');%将彩色车牌写入dw文件中
a=imread('dw.jpg');%读取车牌
b=rgb2gray(a);%将车牌图像转换为灰度图
imwrite(b,'灰度车牌.jpg');%将灰度图写入文件
g_max=double(max(max(b)));
g_min=double(min(min(b)));
T=round(g_max-(g_max-g_min)3);%T为二值化的阈值
[m,n]=size(b);
d=(double(b)=T);%d二值图像
imwrite(d,'二值化.jpg');
%均值滤波前
%滤波
h=fspecial('average',3);
%建立预定义的滤波算子，average为均值滤波，模板尺寸为33
d=im2bw(round(filter2(h,d)));%使用指定的滤波器h对h进行d即均值滤波
imwrite(d,'均值滤波.jpg');
%某些图像进行操作
%膨胀或腐蚀
se=eye(2);%单位矩阵
[m,n]=size(d);%返回信息矩阵
if bwarea(d)mn=0.365%计算二值图像中对象的总面积与整个面积的比是否大于0.365
    d=imerode(d,se);%如果大于0.365则进行腐蚀
elseif bwarea(d)mn=0.235%计算二值图像中对象的总面积与整个面积的比值是否小于0.235
    d=imdilate(d,se);%%如果小于则实现膨胀操作
end
imwrite(d,'膨胀.jpg');
%寻找连续有文字的块，若长度大于某阈值，则认为该块有两个字符组成，需要分割
d=qiege(d);
[m,n]=size(d);
k1=1;
k2=1;
s=sum(d);
j=1;
while j~=n
    while s(j)==0
        j=j+1;
    end
    k1=j;
    while s(j)~=0 && j=n-1
        j=j+1;
    end
    k2=j-1;
    if k2-k1=round(n6.5)
        [val,num]=min(sum(d(,[k1+5k2-5])));
        d(,k1+num+5)=0;%分割
    end
end
%再切割
d=qiege(d);
%切割出7个字符
y1=10;
y2=0.25;
flag=0;
word1=[];
while flag==0
    [m,n]=size(d);
    left=1;
    wide=0;
    while sum(d(,wide+1))~=0
        wide=wide+1;
    end
    if widey1 %认为是左干扰 
        d(,[1wide])=0;
        d=qiege(d);
    else
        temp=qiege(imcrop(d,[1 1 wide m]));
        [m,n]=size(temp);
        all=sum(sum(temp));
        two_thirds=sum(sum(temp([round(m3)2round(m3)],)));
        if two_thirdsally2
            flag=1;word1=temp;%word1
        end
        d(,[1wide])=0;d=qiege(d);
    end
end
%分割出第二至七个字符
[word2,d]=getword(d);
[word3,d]=getword(d);
[word4,d]=getword(d);
[word5,d]=getword(d);
[word6,d]=getword(d);
[word7,d]=getword(d);
[m,n]=size(word1);
%商用系统程序中归一化大小为4020
word1=imresize(word1,[40 20]);
word2=imresize(word2,[40 20]);
word3=imresize(word3,[40 20]);
word4=imresize(word4,[40 20]);
word5=imresize(word5,[40 20]);
word6=imresize(word6,[40 20]);
word7=imresize(word7,[40 20]);
axes(handles.axes26);imshow(word1),title('1');
axes(handles.axes17);imshow(word2),title('2');
axes(handles.axes18);imshow(word3),title('3');
axes(handles.axes19);imshow(word4),title('4');
axes(handles.axes20);imshow(word5),title('5');
axes(handles.axes21);imshow(word6),title('6');
axes(handles.axes22);imshow(word7),title('7');
imwrite(word1,'1.jpg');
imwrite(word2,'2.jpg');
imwrite(word3,'3.jpg');
imwrite(word4,'4.jpg');
imwrite(word5,'5.jpg');
imwrite(word6,'6.jpg');
imwrite(word7,'7.jpg');
liccode=char(['0''9' 'A''Z' '辽粤豫鄂鲁陕京津']);%京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼
SubBw2=zeros(40,20);
l=1;
for I=17;
    ii=int2str(I);
    t=imread([ii,'.jpg']);
    SegBw2=imresize(t,[40 20],'nearest');
    SegBw2=double(SegBw2)20;
    if l==1 %第一位汉字识别
        kmin=37;
        kmax=43;
    elseif l==2 %第二位字母识别
        kmin=11;
        kmax=36;
    else l=3   %第三位后字母或数字识别
        kmin=1;
        kmax=36;
    end
    for k2=kminkmax
        fname=strcat('字符模板',liccode(k2),'.jpg');
        SamBw2=imread(fname);
        SamBw2=double(SamBw2)1;
        for i=140
            for j=120
                SubBw2(i,j)=SegBw2(i,j)-SamBw2(i,j);
            end
        end
        %相当于两幅图相减得第三幅图
        Dmax=0;
        for k1=140;
            for l1=120
                if(SubBw2(k1,l1)0  SubBw2(k1,l1)0)
                    Dmax=Dmax+1;
                end
            end
        end
        Error(k2)=Dmax;
    end
    Error1=Error(kminkmax);
    MinError=min(Error1);
    findc=find(Error1==MinError);
    Code(l2-1)=liccode(findc(1)+kmin-1);
    Code(l2)=' ';
    l=l+1;
end
axes(handles.axes23);imshow(dw),title(['车牌号码：',Code],'Color','b');
axes(handles.axes24);imhist(I1);title('灰度化直方图');

stoptime = datetime;
parktime = stoptime-handles.starttime;
if minutes(parktime)0.5
    money=0;
else 
    money=(round(minutes(parktime)))5;
end
set(handles.text2,'String',datestr(datetime))
set(handles.text3,'String',strcat(num2str(money),' 元'))
guidata(hObject, handles);
```