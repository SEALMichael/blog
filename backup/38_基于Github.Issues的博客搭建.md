# [基于Github Issues的博客搭建](https://github.com/superleeyom/blog/issues/38)

基于 `Github Issues` 博客的搭建教程，整体的过程非常简单，后端参考了 [@superleeyom](https://github.com/superleeyom) 的 [gitblog](https://github.com/superleeyom/gitblog) 项目，发布` issues`，做数据备份，前端参考了[@LoeiFy](https://github.com/LoeiFy) 的 [Mirror](https://github.com/LoeiFy/Mirror) 项目，用于做前端可视化界面，感谢二位大佬的开源精神，所有的服务全部免费（ps：如果你需要自定义域名，自定义域名需要自己付费购买），感谢 Github！

## 利用 Github Actions 做数据备份

首先需要 fork 我的项目 [blog](https://github.com/Thomas-Lv/blog)，也可以 fork [@superleeyom](https://github.com/superleeyom) 的 [gitblog](https://github.com/superleeyom/gitblog) 项目，都差不多，然后修改 `generate_readme.yml`文件，这个文件是触发自动备份的 CI/CD 配置文件，修改如下的地方：

```yml
env:
  GITHUB_NAME: thomas-lv
  GITHUB_EMAIL: xxx@126.com
```

改成你自己的 Github 用户名和邮箱，接着在你自己的这个 blog 仓库下，创建 `Environment secrets`环境变量 `G_T`：

![](https://raw.githubusercontent.com/superleeyom/blog/main/img/20210906215624.png)

![](https://raw.githubusercontent.com/superleeyom/blog/main/img/20210906220916.png)

这个`G_T`是 Github 的访问授权 Token，注意保密，不要泄漏，Token 的获取如下图，scope 如果不知道选啥，全部勾上：

![](https://raw.githubusercontent.com/superleeyom/blog/main/img/20210906215515.png)

修改 `main.py `脚本，修改你自己的定制化的 `README.md` 的 header：

```python
MD_HEAD = """**<p align="center">[Leeyom's Blog](https://blog.leeyom.top)</p>**
**<p align="center">用于记录一些幼稚的想法和脑残的瞬间~</p>**
## 联系方式
- Twitter：[@thomas_lv](https://twitter.com/thomas_lv)
- Telegram：[@thomas_lv](https://t.me/thomas_lv)
- Email：[thomas_lv@126.com](mailto:thomas_lv@126.com)
- Blog：[https://thomas-lv.github.io](https://thomas-lv.github.io)
"""
```

在 `issues` 列表，提前建好 `labels` 标签，后面一旦提交了新的 `issues`，或者你修改了 `issues`，`README.md` 会根据你给当前 `issues` 所打的` labels` 标签进行分类，比如我建了如下的几个` labels`，其中`TODO`和`Top`，用于生成 TODO List 和置顶，比如你给这个 `issues` 加上了 `Top` 标签，那么他会出现在 `README.md` 置顶分类里面，这两个建议加上，剩下的按你自己的意愿加：

![](https://raw.githubusercontent.com/superleeyom/blog/main/img/20210906220109.png)

![](https://raw.githubusercontent.com/superleeyom/blog/main/img/20210906220253.png)

**如果你是 fork 我的项目，建议先把 backup 文件夹下里面的 md 文件删除！因为那是我的 blog 备份文件！**

有了这个项目，我们就可以通过 `Github Actions`，只要有 `issues` 发布或者修改，都会触发自动构建，备份` issues `生产 md 文件，然后刷新 `README.md` 文件。

后期你要发布文章，只需要创建一个 `issues`，然后打好标签，点击发布即可，剩下的都是自动化构建，不需要人为参与。

## 利用 Github Pages 做可视化界面

- 首先你得先创建一个 `github用户名.github.io`的仓库，必须是公共仓库，比如我的：[thomas-lv.github.io](https://github.com/Thomas-Lv/thomas-lv.github.io)，然后你把我这个仓库 [thomas-lv.github.io](https://github.com/Thomas-Lv/thomas-lv.github.io) 的文件全部拷贝到你刚创建的仓库里面，删除 `Archive`文件夹，这个是我以前备份的 md 文件，对你没啥用！

- 修改 `docs` 目录和根目录下的两个 `CNAME` 文件，里面的内容是你的自定义的域名，比如我的自定义域名是：`blog.leeyom.top`，如果没有自定义域名，默认填：`github用户名.github.io`。

- 修改 docs 目录下的 `index.html` 文件，比如我的：

  ```js
  window.config = {
    organization: false,// 默认是 false，如果你的项目是属于 GitHub 组织 的，请设置为 true
    order: 'CREATED_AT',// 文章排序，以 创建时间 或者 更新时间，可选值 'UPDATED_AT'，'CREATED_AT'
    title: "Thomas' Blog",// 博客标题
    user: 'thomas-lv',// GitHub 用户名，必须
    repository: 'blog',// GitHub 项目名，指定文章内容来源 issues，必须
    authors: 'Thomas_Lv',// 博客作者，以 ',' 分割，GitHub 用户名默认包含在内
    ignores: '',// 文章忽略的 issues ID
    hash: 'ghp_VkKID%Qnlgg$SXfIt!UmH&uCLCtHFU$XJHK^YmxvZy5sZWV5b20udG9w',// hash，必须
    perpage: 5,// 分页数量
  }
  ```

  其中关于 hash 的获取，参考：[「获取 hash」](https://github.com/LoeiFy/Mirror/wiki/%E8%8E%B7%E5%8F%96-hash)

- 去阿里云或者腾讯云，申请一个新的域名，将域名解析到你自己刚创建的 github 仓库 `github用户名.github.io`，不需要自定义域名的可忽略这一步：

  ![](https://raw.githubusercontent.com/superleeyom/blog/main/img/20210906222239.png)

- 在 `github用户名.github.io` 仓库下，将 page 的 source 指向到 docs 目录，指定你的自定义域名即可，若没有自定义域名，`Custom domain` 可以不用设置：

  ![](https://raw.githubusercontent.com/superleeyom/blog/main/img/20210906222539.png)

- 最后改下你的 `README.md`，里面的内容是我自己瞎写的，换成你自己的，然后等个 5 分钟，访问你的域名，比如我的是：[https://blog.leeyom.top](https://blog.leeyom.top)（没有自定义域名默认的是：`https://github用户名.github.io`），看是否能正常访问，如果不能正常访问，请在当前 issues 下留言，我看到会回复。

## 总结

虽然 Github Issues 的定位就不是为博客而生的，这也注定了它有诸多不足之处，比如无法限制别人发 issue ，但是对于那些不想折腾，内容才是王道的程序员朋友来说，免费、Markdown、代码高亮、标签、评论、图床、备份、Github大厂背书，Github Issues 也不乏是个与自己和他人沟通的好地方。

## 参考资料

- [这个博客开源了](https://github.com/superleeyom/gitblog/issues/177)
- [Mirror中文教程](https://github.com/LoeiFy/Mirror/wiki/%E4%B8%AD%E6%96%87%E6%95%99%E7%A8%8B)