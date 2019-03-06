# Layui-select 下拉框实现拼音全拼匹配/首字母模糊搜索

<!-- 版权声明：本文为博主原创文章，未经博主允许不得转载。https://blog.csdn.net/sinat_39571186/article/details/80275578 -->

## Layui
layui（谐音：类UI) 是一款采用自身模块规范编写的前端 UI 框架。亲测很好用，很好看。 
</br>
官网：http://www.layui.com/</br>
github：https://github.com/sentsin/layui

插播一条相关博客：<a href="https://blog.csdn.net/sinat_39571186/article/details/80671422" target="_blank">Layui-select 修复搜索之后上下键的bug</a>

<div style="margin: 24px 0;border-bottom:1px solid #ddd;"></div>

## 研究select搜索功能的实现
layui的select组件自带搜索功能，只要在select标签里面添加属性search=""即可。 
但是如果选项非常多，很多人会想，如果能够用拼音快速地搜索匹配就更好了，我只要输入拼音前面几个字母，就能够快速匹配到我想要的选项。但是这个功能Layui的select没有帮我们实现。 
没关系，我们来读读它的源代码，很快就能找到搜索功能是怎么实现的，我们在这里自己加上我们的代码就可以实现啦。

首先毫无疑问，select组件的功能就是放在form.js里面啦。 
我们可以看到select的搜索功能中，调用了一个叫notOption的函数来判断是否有匹配的选项。

<img src="https://img-blog.csdn.net/20180510212500220?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM5NTcxMTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" title="">

<p style="margin:24px 0;">我们找到这个函数之后，发现判断输入内容与各选项是否匹配的是这个语句：</p>

<img src="https://img-blog.csdn.net/20180510212720261?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM5NTcxMTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" title="">

<p style="margin:24px 0">那我们就可以从这个语句入手啦。</p>

<div style="margin: 24px 0;border-bottom:1px solid #ddd;"></div>

## 开始动手解决
之前在使用easyUI框架的时候，我也动手实现过它的select下拉框拼音搜索功能。因此，结合上一次的实现，这一次根据layui的select组件的dom实现特点做一些改动。（有机会写一个easyUI的实现方法，只是easyUI的解决方法还是挺多 
的，可以根据自己的实际需求做改动）

## 一、引进插件
首先我们要先引进一个pinyin.js文件，它是一个将汉字转换为拼音的JavaScript插件，并且支持多音字，直接使用ConvertPinyin(text)获取。http://www.uedsc.com/pinyin-js.html 这个网址是这个插件的介绍。 
还要引进一个initials.js文件，它是提取汉字首字母的，返回的是一个大写字母字符串组成的数组，当然这个数组只有一个元素。获取时应正确使用：makePy(text)[0]。 
为了方便，以上两个已上传到我的码云和github，CSDN资源库也有，可前往下载：

<div>码云：<a href="https://gitee.com/onionoo/pinyin.js" target="_blank">https://gitee.com/onionoo/pinyin.js</a></div>
<div>github：<a href="https://github.com/onionooO/ChineseToPinyinAndInitials" target="_blank">https://github.com/onionooO/ChineseToPinyinAndInitials</a></div>
<div>CSDN资源：全拼：<a href="https://download.csdn.net/download/sinat_39571186/10417956" target="_blank">https://download.csdn.net/download/sinat_39571186/10417956</a></div>
<div>首字母：<a href="https://download.csdn.net/download/sinat_39571186/10417947" target="_blank">https://download.csdn.net/download/sinat_39571186/10417947</a></div>

## 二、修改代码
我们将输入值是否与选项匹配的not变量用一个函数myFilter来实现。在下图这个位置改动一下代码，同时添加一个获取选项value的变量id： 

<img src="https://img-blog.csdn.net/20180513002335120?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM5NTcxMTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" title="">

<p style="margin:24px 0">接下来就是myFilter函数的实现了。在notOption函数上面加一个函数myFilter：</p>

<img src="https://img-blog.csdn.net/20180516142042964?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM5NTcxMTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" title="">

<p style="margin:24px 0">重点是在画框框的这个判断语句，可以根据自己的需求来实现。 
ConvertPinyin(text).substring(0, len) === value 这句语句是为了实现拼音从头到尾地匹配，这就要求你要全拼。匹配效果如下：</p>

<img src="https://img-blog.csdn.net/20180513003248770?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM5NTcxMTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" title="">

<p style="margin:24px 0">makePy(text)[0].toLowerCase().substring(0, len) === value 这句语句是实现首字母从头到尾匹配，匹配效果如下：</p>

<img src="https://img-blog.csdn.net/20180516142235858?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM5NTcxMTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" title="">

<p style="margin:24px 0">这样基本就能够实现拼音全拼和首字母匹配了。（因为可能涉及到公司一些东西，所以就打了码，能看懂就好。）</p>

<div style="margin: 24px 0;border-bottom:1px solid #ddd;"></div>

## 注意
顺便一提，我们可以发现，Layui的select组件和普通的select并不一样，它是用定义列表dl、dd来排列选项的。 

<img src="https://img-blog.csdn.net/20180512171818502?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM5NTcxMTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" title="">

<p style="margin:24px 0">这也是notOption函数中的dds的来历以及对dds进行遍历的原因，同时也是我选择在源代码里面做改动的原因。</p>

<p style="margin:24px 0">我本来想要另外重写一下那个notOption函数，尽量不要去改动源代码的。然后我发现dds的获取好像挺有讲究的，在函数前面老远老远的地方，看图： </p>

<img src="https://img-blog.csdn.net/20180513004739475?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzM5NTcxMTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" title="">

<p style="margin:24px 0">然后我就放弃重写了，除了我懒之外，还怕破坏一些东西，就直接在源代码里面修改了。</p>

<p style="margin:24px 0">最后一点但也是重点：我们从官网下载下来的js文件都是经过压缩的，所以你如果直接打开form.js文件进行解读并修改你会晕倒到电脑前的。所以你可以前往github拷贝下form.js文件进行修改，github上的文件都是源文件哦。</p>

补充：layui-select搜索出来之后存在一个上下键搜索的bug，解决请看另一篇博客：<a href="https://blog.csdn.net/sinat_39571186/article/details/80671422" target="_blank">Layui-select 修复搜索之后上下键的bug</a>

<div style="margin: 24px 0;border-bottom:1px solid #ddd;"></div>

## 最后
还是贴一下代码吧。建议还是读一下源代码，或看一下上面的内容，下次作者再次更新代码的时候，就知道在哪个地方做修改了。 
注意，第一步引入两个文件，pinyin.js和initials.js。第二步，如果你引用的是layui.all.js文件，那么你就要改一下了，要改成引用layui.js文件哦。

```
//搜索过滤器
var myFilter = function(value, text, id) {
    var result;
    if (escape(value).indexOf("%u") != -1) { //汉字
        result = text.indexOf(value) > -1;
    } else {
        var len = value.length
        result = ConvertPinyin(text).substring(0, len) === value || makePy(text)[0].toLowerCase().substring(0, len) === value || text.toLowerCase().indexOf(value) > -1 || (id === undefined ? false : id.indexOf(value) > -1);
    }
    if (result == true) {
        return false;
    } else {
        return true;
    }
};

//检测值是否不属于 select 项
var notOption = function(value, callback, origin) {
    var num = 0;
    layui.each(dds, function() {
        var othis = $(this)
            ,text = othis.text()
            ,id = othis.attr("layui-value")
            // ,not = text.indexOf(value) === -1
            ,not = myFilter(value, text, id);
            //value为input框输入的值，
            //text是当前dd的值，即当前被遍历到的选项，
            //id为当前dd的layui-value属性
        if (value === '' || (origin === 'blur') ? value !== text : not) num++;
        origin === 'keyup' && othis[not ? 'addClass' : 'removeClass'](HIDE);
    });
    var none = num === dds.length;
    return callback(none), none;
};
```
<div style="margin: 24px 0;border-bottom:1px solid #ddd;"></div>
