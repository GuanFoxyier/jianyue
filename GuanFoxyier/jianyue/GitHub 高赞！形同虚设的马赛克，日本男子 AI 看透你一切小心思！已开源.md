> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/diHH6pOsII-SuKYoc6Ks_Q)

点击上方 “Github 中文社区”，关注  

看遍 Github，每天提升

```
第054期分享 编辑:huber 来自机器之心

```

#####   

hello 大家好啊，我是 huber！

**“来 P 个图吧！” “好呀，不过这段话得打码，不然就麻烦了！”**

如果现在告诉你，”打码 “已经不再安全，你所想保护的信息，已然如” 皇帝的新衣“，你会作何想？

不，这不是耸人听闻，最近一个名为 Depix 的 GitHub 项目火了，上线仅仅三天，star 量就已经高达 7K 。截止发文，此项目已经火速达到现在高达 24K 的 star 量：

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJPUpMSZsjPMkPKsG9PmG8KgXxeUibvuaicKaQIibUkh14dOWWq3icVZW4PLw/640?wx_fmt=png)

而就是这项技术，能够解码被打上马赛克的文字，你的所有努力，甚至有了” 欲盖弥彰 “的效果。

   手机涂鸦如同 “徒劳”，外行也能轻易恢复隐藏信息  

前段时间，网络上爆出，使用手机涂鸦对图片所进行的操作，其实可以轻易被恢复：

简单拿微信聊天截屏的文字涂鸦来说：

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJPGUOsn9bjPEHl9hmo9by8pX836tSAWM1FfsMsRiaCTeQia2T8VNJqYOLA/640?wx_fmt=png)

我们身边太多的人，都可能会用这种涂鸦技术，遮盖自己想保密的信息。

看似很安全，对不对？

其实在有心人看来，你的操作可以马上成为徒劳：

只需要再次利用手机的图片编辑功能，将曝光、鲜明度、高光、阴影、亮度等参数全部调至 + 100，然后再将对比度参数调至 - 100，然后，神奇的事情发生了：

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJP7kW2hiazSQaQcepLItM1XtxsflwibvAicHzgwJFsaJXQSUp8ibVPIrmTzw/640?wx_fmt=png)  

行家都知道，相比于涂鸦，马赛克却无法被修复和逆转，令人非常的安心。

可是，放在现在，在 AI 面前，修复厚码图片中隐藏的内容，也成为了可能：

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJPXlmNyic5Xd2asOwCwzGHjBg5a3vsFkrXsicdepG0RXicPTLrk9IKm6rEw/640?wx_fmt=png)

Depix——修复厚码文字内容，现已开源

Depix 的主要功能，就是利用 AI 算法，将被像素化的文本内容从马赛克中还原出来。其适用于用线性盒过滤器创建的像素化图像。

其目的不是去马赛克，而是做文字恢复使用。虽说这可能令一些宅男失望，但其作用依旧强大且有意义。

此项目是由信息安全顾问 Sipke Mellema 开发的，目前仅支持英文字母、数字和英文标点符号。

而任何此个开源项目的使用者，简简单单使用以下指令，就可以恢复你想 “窥探” 的文字内容：

`python depix.py -p images/testimages/testimage3_pixels.png -s images/searchimages/debruinseq_notepad_Windows10_closeAndSpaced.png -o output.png`

**完整操作如下：**

*   从截图中剪出像素化的方块，作为一个矩形。
    
*   将 De Bruijn 序列粘贴到编辑器中，使用相同的字体设置 (文本大小、字体、颜色、hsl)。
    
*   制作序列的截屏。如果可能的话，使用同样的截图工具来创建像素化的图像。
    
*   运行 run python depix.py -p [pixelated rectangle image] -s [search sequence image] -o output.png
    

算法原理简单：分割小块，德布鲁因序列字符库助力像素匹配

Depix 的原理是将马赛克区域的内容分割成许多个小块，然后将每个小块都和预先设置好的字符库（德布鲁因序列（De Bruijn sequence））进行像素匹配。

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJPIkFXql8CMV15Zgy6MCjFKQpDzC4c24071GQvbpx40fNaqRBQl8MSRw/640?wx_fmt=png)  

**具体算法流程如下：**

该算法利用了线性盒滤波器，来分别处理每个块的特性。对于每个块，它对搜索图像中的所有块进行像素化，以检查是否直接匹配。

对于大多数像素化的图像，Depix 设法找到单一匹配的结果。它假设这些都是正确的。然后，将周围的多匹配块的匹配在几何上与像素化图像中的相同距离进行比较。匹配也被视为正确。这个过程要重复几次。

当正确的块没有几何匹配时，它将直接输出所有正确的块。对于多匹配块，它输出所有匹配的平均值。

开发这个 AI 项目，Mellema 并不是为了窃取信息，而是利用 ECB 和明文攻击的模式，提高信息保护技术。

在他看来，不知道如何破坏当前的保护模式，是信息安全中的常见陷阱。

Depix 主要是针对打码文字的处理，而说到修复马赛克像素级别图片的技术，我们不得不提杜克大学的 AI 算法 PULSE：

  宅男福利？渣画质修复还要看杜克 PULSE

   

杜克大学的 AI 算法 PULSE（Photo Upsampling via Latent Space Exploration），可以将像素渣到马赛克级别的图片修复：

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJPVOOiceBXXWl2j4CiaeRia9z75TXrxN7tZTiaHfN6ibSerdtYtloYAxaRYRg/640?wx_fmt=png)  

该算法可以将模糊、无法识别的人脸图像转换成计算机生成的图像，并且具有比之前任何时候都更加精细、逼真的细节。

按照之前的方法，想要把一张模糊的大头照变清晰，最多只能将这张照片缩放到原始分辨率的八倍。

而 PULSE，可以仅在几秒钟内，就可以把 16x16 像素的低分辨率小图，放大 64 倍，变成 1024 x 1024 像素的高分辨率图像。

这种将像素放大 64 倍级别的，绝对是业界首次。

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJPibWEwzXS9QKccZrQPjHcPbv0D3tcHtulakWPE7jyWzCfotD4cy1tHrA/640?wx_fmt=png)

原本低分辨率照片中无法看到的细节，比如毛孔、细纹、睫毛、头发和胡茬等，经过 PULSE 算法处理后，都能看得一清二楚：

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJP7hOAMnnWU3iaNwqJddBFkH5swfLxZoo1HMDBHiasqCSxbWPBiaTSlt3nw/640?wx_fmt=png)

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJPboFS0UHLnWhEAriaQOAQMz69hZrAgUb2YM9LAploWdE5MAXicjFgZmDw/640?wx_fmt=png)

![](https://mmbiz.qpic.cn/mmbiz_png/UicQ7HgWiaUb0k88hOv0Mnu6RUK5380BJPBG9GyibB2kwUwkYeFPhKTkQKukC07K9mib0fcIeIcG6b7DkvupibiaJG2Q/640?wx_fmt=png)

涉及到实际应用方向上，论文的共同作者 Sachit Menon 介绍称：

「在这些研究中，我们只是用面部作为概念验证。

但从理论上讲，该技术是通用的，从医学、显微镜学到天文学和卫星图像，都可以通过该技术改善画质。」

与此类似的，还有谷歌的超强像素递归方案，感兴趣的朋友可以自行探索。

传送门
===

最后附上 Depix，PULSE 的项目链接：

Depix 项目地址：https://github.com/beurtschipper/Depix

PULSE 项目地址：https://github.com/adamian98/pulse

  

参考链接：

https://www.maxiaobang.com/6570.html

https://github.com/beurtschipper/Depix

https://github.com/adamian98/pulse

  

![图片](https://mmbiz.qpic.cn/mmbiz_png/s3scpabv7NUwIqj3icxFZ56a6wpYgYh6rHvv8UXict7nEhJd1z6LlP0iaGteSYGfRHwIuQ1KwQZdSIIvKQCOD9j9w/640?wx_fmt=png)

  

OK！到这就是这期分享

  

如果觉得文章有用，请点在看，收藏，分享。

  

  

历史分享

  

[★](http://mp.weixin.qq.com/s?__biz=MzU3ODMwNTk1NA==&mid=2247484512&idx=1&sn=296f520b1cec9e9600ab268578cda1d3&chksm=fd762eadca01a7bbd1f37b8ca61b1c0f3ea85da68e7a31099932b6fb1a09ece7d9739b6baea7&scene=21#wechat_redirect) [98 后小哥出的校招黑名单火了！标星 4K，校招生有福了！](http://mp.weixin.qq.com/s?__biz=MzU3ODMwNTk1NA==&mid=2247496001&idx=1&sn=608632ec6928f9cf8b542f1dc2181b15&chksm=fd75db8cca02529abf62d0059f8177fefce2a32bbe63919d5d0e1302ba1ea11d4da747333eb4&scene=21#wechat_redirect)

[★](http://mp.weixin.qq.com/s?__biz=MzU3ODMwNTk1NA==&mid=2247484512&idx=1&sn=296f520b1cec9e9600ab268578cda1d3&chksm=fd762eadca01a7bbd1f37b8ca61b1c0f3ea85da68e7a31099932b6fb1a09ece7d9739b6baea7&scene=21#wechat_redirect) [GitHub 上 100 个优质前端项目整理，非常全面！](http://mp.weixin.qq.com/s?__biz=MzU3ODMwNTk1NA==&mid=2247496001&idx=2&sn=496187cc547d69d6e954965acb8b300c&chksm=fd75db8cca02529a816eadbb5ba5f49e950d11f649ade79d790ec9f9d4ddd63cfa5f29be20fb&scene=21#wechat_redirect)

[★](http://mp.weixin.qq.com/s?__biz=MzU3ODMwNTk1NA==&mid=2247484512&idx=1&sn=296f520b1cec9e9600ab268578cda1d3&chksm=fd762eadca01a7bbd1f37b8ca61b1c0f3ea85da68e7a31099932b6fb1a09ece7d9739b6baea7&scene=21#wechat_redirect) [B 站 CEO 的身份证被上传到 GitHub 了？官方回应...](http://mp.weixin.qq.com/s?__biz=MzU3ODMwNTk1NA==&mid=2247495794&idx=1&sn=ff5d57865e0c67e5a46439701d35c560&chksm=fd75dabfca0253a94aa1a93b0ccb8f6012aa54cf02150cd17dc81143fb8428ee5852b8c0203e&scene=21#wechat_redirect)

[★](http://mp.weixin.qq.com/s?__biz=MzU3ODMwNTk1NA==&mid=2247484512&idx=1&sn=296f520b1cec9e9600ab268578cda1d3&chksm=fd762eadca01a7bbd1f37b8ca61b1c0f3ea85da68e7a31099932b6fb1a09ece7d9739b6baea7&scene=21#wechat_redirect) [热度飙升: 8.5K ！GitHub 上的这个开源游戏彻底火了！](http://mp.weixin.qq.com/s?__biz=MzU3ODMwNTk1NA==&mid=2247495913&idx=1&sn=6f454639b46306332f9c2baf5bbb5d4d&chksm=fd75da24ca025332ea4b8b089baf7ce2316414f02f8ed095b94d4bc342fcec621d68bbc0d5dc&scene=21#wechat_redirect)

  

![](https://mmbiz.qpic.cn/mmbiz_png/s3scpabv7NWicXutI5FhSyp29Nic7hKPagyic9Vk5JpUUFbbmxvbJGVWlHSYQk5jKjFCTnLUKrSUoxcIibpMjsmA5g/640?wx_fmt=png)

![](https://mmbiz.qpic.cn/mmbiz_png/s3scpabv7NWicXutI5FhSyp29Nic7hKPagJ5mk7JcB34vv8xKdZ3jtcHWRlMPjjicTU5ZzMNZdibUvfgxKFSjibjSjg/640?wx_fmt=png)

  

点个在看呗！