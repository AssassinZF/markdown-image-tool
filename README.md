# markdown-image-tool
用GitHub做免费图床，配合脚本工具，实现一键上传图片并获取URL链接

## 准备工作
首先在你的GitHub上创建一个仓库，然后什么都不用做。

然后本地打开终端输入：

```bash
$ mkdir xxx //创建一个目录用来存放你的图片
$ cd xxx 
$ git init 
$ git remote add origin xxx // 这里的xxx 是你上面创建仓库的 SSH Key 链接， 不要用HTTPS那个地址，那个每次需要输入密码，这个是要关联你的GitHub远程仓库用的
```
到这里其实就已经把云图床存储搭建好了。

绑定之后基本就完成了图床的云存储了，免费又方便。如果你随便上传一张图片到上面你创建的厂库，然后你在GitHub网页端点击图片文件，会发现右上角显示一个 Download 按钮 ，然后点击下载按钮，会继续打开此图片，然后地址栏上面链接就是这个图片的URL，拿到url就能在markdown里面使用了。
```
//Markdown 插入图片方法
![](url)
```

示例图：

*点击你上传的image文件*
![](https://raw.githubusercontent.com/AssassinZF/MDImageRepo/master/2018-11/jietu1.png)

*跳转到下载页,然后点击 Download*
![](https://raw.githubusercontent.com/AssassinZF/MDImageRepo/master/2018-11/jietu2.png)

之后再URL地址栏中可以看到图片的链接URL。

```
https://raw.githubusercontent.com/yourgithubname/你刚创建的仓库名/master/2018-11/testimage.jpg
```

这里的日期是我为了把图片分类增加的一个层级。

👌，到这里我们就能拿到远程的图片URL了，但是不能每次上传图片之后再到浏览器里去点击去取URL，再到markdown 里面填写上，这个过程太繁琐了，下面就我们使用工具简化流程。

## 工具介绍

先拿到你的URL仔细观察下，https://raw.githubusercontent.com/yourname/repo_name/image_name.jpg 你会发现其实前面都是固定的吗，只有后面一部分是根据图片名称来变化的，那我们只要在本地拼接好这个URL然后自动上传图片就可以了,上传图片我们是Git管理，现在问题是我们如何选中一个一键点击就能自动拼接好上面我们要的URL，并且Git把图片也push 到GitHub 这一系列流程呢？

废话少说~看代码
 
我们会使用Applescript语言来写一这个自动话脚本，有点小伙伴可能对Applescript比较陌生，其实他是Apple自己弄得一个脚本语言，用来在Mac上做workflow很方便，感兴趣的朋友可以了解下，这里不再详细说，自己已经写好的一个脚本，先看一键上传的脚本，一键获取下面会讲 [自动上传图片代码](https://github.com/AssassinZF/Markdown-Image-tool)（用Mac自带的`脚本编辑器`软件打开）：

代码下载之后用脚本编辑器打开，需要改几个地方：

第一处修改：

```bash
--设置GitHub仓库对应的本地本地仓库目录 （-- 这个是Applescript语言的注释方式）
set resourcePath to "/Users/xxx/ImageRepo" 
```
要确保你的这个仓库是和你GitHub仓库是关联的，就是在这个目录可以执行 git add / git push 这些命令的。

第二处修改：

```bash
// 下面的 URL 就要换成上面讲的，前面不会变，只有后面会变的 图片URL ，其实这就是个拼接语句，后面的 currentDatePath 和  uploadFileName
// 分别是时间和图片名称。然后拼接上前面的固定URL，
set sourceUrl to ("https://raw.githubusercontent.com/yourgithubname/仓库名/master/" & currentDatePath & "/" & uploadFileName)
display dialog "上传成功后获取到的链接" default answer sourceUrl buttons {"复制", "关闭"} default button 1 with title "提示" with icon note
```

最后一个地方： 如果你用的是 iTerm 终端 就不用改了，如果不是那就。。。 算了我感觉如果你用的Mac那就装一下iTerm （终端神器）替代系统的终端程序吧！
因为我们提交代码的时候是打开的 iTerm 的。如果是就忽略。
```bash
if application "iTerm" is running then
	tell application "iTerm"
		--set newWindow to (create window with default profile)
		
```
好了到这里脚本就修改完毕了！ 但是怎么执行呢？

这时候要用到另一个Mac自带的软件，中文名称“自动操作”就是那个白色机器人拿一个火箭筒的那个😆。

![](https://raw.githubusercontent.com/AssassinZF/MDImageRepo/master/2018-11/Jietu20181121-200555.jpg)

打开之后按图操作：

![](https://raw.githubusercontent.com/AssassinZF/MDImageRepo/master/2018-11/Jietu20181121-200717.jpg)

![](https://raw.githubusercontent.com/AssassinZF/MDImageRepo/master/2018-11/Jietu20181121-200932.jpg)

![](https://raw.githubusercontent.com/AssassinZF/MDImageRepo/master/2018-11/Jietu20181121-201059.jpg)


如果一些顺利那么你就可以试试随便在你的文件夹中选择一个图片邮件点击，选择最下面的服务，会看到你刚才在保存脚本的时候起的名字 “一键上传GitHub”，然后点击他，这个时候你的ITerm终端会自动打开并且 cd 到你的和GitHub仓库对应的本地仓库哪里，自动执行了git 上传的一些操作，这个你不用管，屏幕上会弹出一个连接，这个就是你要用的URL，复制之后就获取了URL，（至于终端那个可以手动关掉，因为我试了如果用脚本关掉，会导致上传不成功，因为GitHub上传也是需要时间的，不过这个不影响）赶紧去markdown 里面试试吧😆。

![](https://raw.githubusercontent.com/AssassinZF/MDImageRepo/master/2018-11/Jietu20181122-085531.jpg)

如果没有出现，那么点击`系统设置`-> `键盘`->`快捷键`->`服务` 看下右侧是否有你的 “一键上传GitHub”命名的action,如果前面打勾了，就OK没有就勾上。

## 自动获取已经上传的图片链接

假如你的图片已经上传了，但是你忘记了URL地址，那你下载这个[一键获取GitHub图片链接](https://github.com/AssassinZF/Markdown-Image-tool)脚本，这个和上面是一样的安装方式，都是打开自动操作，复制进去代码，然后保存，就会在你右键图片，服务项里面出现你命名的 “一键获取GitHub图片链接” 操作，点击看看吧！

如果能帮到你，给个赞👍

- [GitHub 代码下载](https://github.com/AssassinZF/markdown-image-tool)✨


