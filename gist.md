# gist 的使用
Gists是 GitHub 推出的基于 Git 的代码片段服务。Gists页面提供JavaScript代码，可以将 Gist 嵌入到其他站点。但是很多站点粘贴 JavaScript 无效，这时候你可以在 Gist URL 后附加.pibb，得到一个纯 HTML 的版本，然后就可以复制粘贴 HTML 源码到其他网站了。例如 https://gist.github.com/tiimgreen/10545817.pibb
```
html里面 把代码引入即可
<script src="https://gist.github.com/tiimgreen/10545817.js"></script>
效果如下图
```
![1](gist.png)



### 实在不行用 iframe html框架

```
<iframe src="https://gist.github.com/tiimgreen/10545817.pibb" name="iframe_a"  width="500" height="100"></iframe>
```
```
最终效果如下
```

<iframe src="https://gist.github.com/tiimgreen/10545817.pibb" name="iframe_a"  width="500" height="100"></iframe>
<iframe src="https://gist.github.com/z007/9ac2cbcb29c734216749.pibb" name="iframe_a"  width="500" height="100"></iframe>
参考
[segmentfault.com](http://segmentfault.com/a/1190000000475547)
[infoq](http://www.infoq.com/cn/news/2008/07/gist-versioning-for-pasting )
[oschina](http://my.oschina.net/swuly302/blog/152008)
[官方参考文档](https://developer.github.com/v3/gists/comments/#custom-media-types)

补充
[Gistbox](https://app.gistboxapp.com/)
[Gist仓库](http://www.v2ex.com/t/146245)
