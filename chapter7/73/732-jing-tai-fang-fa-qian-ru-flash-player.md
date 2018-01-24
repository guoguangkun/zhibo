如何使用SWFObject的静态方法嵌入Flash Player内容？
　　
步骤1：使用符合标准的标记嵌入Flash内容和替代内容
　　SWFObject的基础标记使用嵌套对象的方法（用专有的IE条件注释），确保仅标记手段的最优化跨浏览器支持，而作为符合标准和配套的替代内容。
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
  <head>
    <title>SWFObject - step 1`</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
  </head>
  <body>
    <div>

      <object classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" width="780" height="420">
        <param name="movie" value="myContent.swf" />
        <!--[if !IE]>-->
        <object type="application/x-shockwave-flash" data="myContent.swf" width="780" height="420">
        <!--<![endif]-->
          <p>Alternative content</p>
        <!--[if !IE]>-->
        </object>
        <!--<![endif]-->
      </object>

    </div>
  </body>
</html>
注：嵌套对象方法需要一个双对象定义（外部对象针对Internet Explorer和内针对所有其他的浏览器的对象），你需要定义你的对象属性和嵌套的param元素两次。
　　
需要的属性：
classid（只用于外层元素，值一直是： clsid:D27CDB6E-AE6D-11cf-96B8-444553540000）
type（只用于内层元素，值一直是： application/x-shockwave-flas）
data（只用于内层元素，定义swf的路径：data="myContent.swf"）
width（定义swf的宽度，内外都用到）
height（定义swf的高度，内外都用到）
需要的参数：
movie（只用于内层元素，定义swf的路径：<param name="movie" value="myContent.swf" />）
注意：我们建议不要使用codebase属性指向Adobe Flash插件安装程序的URL的服务器，因为这样是违法的，他限定了只能当前域来访问。我们建议在替代内容中放一个提示信息，这样用户会有更好的体验而不是下载flash。

怎么使用HTML来配置Flash内容？
你能在标签中加下面的属性:
id
name
class
align 
可用下面的参数:
play
loop
menu
quality
scale
salign
wmode
bgcolor
base
swliveconnect
flashvars
devicefont (more info)
allowscriptaccess (more info here and here)
seamlesstabbing (more info)
allowfullscreen (more info)
allownetworking (more info)

为什么使用替代内容？
　　对象元素允许你在他里面放替换元素，在flash没有安装或者不被支持的时候会显示。他的内容同样也会被搜索引擎抓到，他是一个用于创建搜索引擎友好的内容的非常好的工具。总而言之，你应该使用替换内容，当你想让你创建的内容可以被没有装插件的用户访问，对搜索引擎友好，告诉访问者对应提示，这样会有更好的用户体验而不是直接去下载插件。
步骤2：包括JavaScript库在你的HTML页面的头部
　　SWFObject库是一个外部JavaScript文件。SWFObject一被加载就会被执行，一但dom元素加载完了就会执行dom操作，所有浏览器都支持他（  IE, Firefox, Safari and　Opera 9+），或者是onload事件被触发。
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
  <head>
    <title>SWFObject - step 2</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />

    <script type="text/javascript" src="swfobject.js"></script>

  </head>
  <body>
    <div>
      <object classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" width="780" height="420">
        <param name="movie" value="myContent.swf" />
        <!--[if !IE]>-->
        <object type="application/x-shockwave-flash" data="myContent.swf" width="780" height="420">
        <!--<![endif]-->
          <p>Alternative content</p>
        <!--[if !IE]>-->
        </object>
        <!--<![endif]-->
      </object>
    </div>
  </body>
</html>
步骤3：用SWFObject库注册您的Flash内容，告诉SWFObject做什么

首先添加一个唯一的ID外部对象标记定义您的Flash内容，第二添加swfobject.registerObject的方法：
1.第一个参数（字符串，必需）指定标记中使用的ID。
2.第二个参数（字符串，必需）指定了你内容发布所需要的flash版本。它会激活Flash版本检测，以确定是否显示Flash内容或通过dom操作强制显示替代内容。Flash的版本号通常由major.minor.release.build四部分组成，但是SWFObject只识别前三个数字，所以 "WIN 9,0,18,0" (IE) 或者 "Shockwave Flash 9 r18" (其他浏览器)都会被翻译成 "9.0.18". 如果你只想测试主要版本，你可以省略次要和发行版本号，如“9”，而不是“9.0.0”。
3.第三个参数（字符串，可选），可用于启动Adobe快速安装，并指定您的快速安装SWF文件的URL。快速安装会显示一个标准化的Flash插件下载对话框来替代你的Flash内容，当所需的插件版本不可用。一个默认的expressInstall.swf文件被一起打包在项目中。它也包含了相应的expressInstall.fla和AS文件（在SRC目录），让你创建自己的自定义快速安装体验。请注意，快速安装只会触发一次（他第一次被调用），他只被支持在win平台和mac平台的 Flash Player 6.0.65以上的版本，他会要求一个最小的尺寸是  310x137px。
4.第四个参数（js函数，可选）用来定义一个回调函数，当插件创建成功或者失败都可以调用该函数来处理一些事。
<script type="text/javascript">
  swfobject.registerObject("myId", "9.0.115", "expressInstall.swf");
</script>

小提示：
1.用SWFObject HTML和js生成器来帮助你编写代码。
2.重复步骤1和3，把多个SWF文件嵌入到一个HTML页面
3.引用活动对象元素的最简单的方法是使用JavaScript API：swfobject.getObjectById（objectIdStr