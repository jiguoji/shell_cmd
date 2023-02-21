# how to add pic
现在我想引用一张图片：
step1 首先我要将在本地目录中添加一张图片，考虑到大家以后可能会经常使用图片，所以建立了一个名为images-folder的文件夹，并统一把图片放到这个文件夹里。
step2 将本地目录的修改push到自己的仓库。
step3 此时要引用的图片已经放在仓库的文件夹images-folder里面
step4 现在，我要在当前md文件里链接这张图片，我必须知道github 图片链接格式，即 ![](img_url)，即 叹号!+方括号[ ]+圆括号( ) 其中圆括号里是图片的URL，方括号里可以不填。
step5 来看看例子吧(大家记得在给文件命名时必须加上 .md 否则打开文件不能看到图片)
step6 现在我用转义字符将完整的引用格式附在右边，大家在实际使用中应该是看不到下边这条文字信息的：

\![](https://github.com/jiguoji/shell_cmd/blob/main/add%20people%20permission/TeamLogo.png)

![](https://github.com/jiguoji/shell_cmd/blob/main/add%20people%20permission/TeamLogo.png)

