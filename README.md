Ghost 0.5.2 with Upyun Support From https://github.com/sanddudu/ghost-0.5.2-upyun
=================

介绍
------
Ghost 原版只维护了本地储存，该版本将其修改为上传至又拍云。  
本版本基于 Ghost 0.5.2 Stable。  
又拍云 Storage 代码修改自 [GhostChina 发布的 Ghost 0.4.2 又拍云集成](https://github.com/ghostchina/Ghost-0.4.2-upyun) 。  
又拍云上传代码来自又拍云 Node.js SDK。

修改部分
------
* 修改了 `core/server/storage/index.js` 使其只支持又拍云上传  
  （由于 Storage 只会被初始化一次，配置文件内的内容尚未被被读取，无法判断配置是否存在）
* 修改了 `core/server/config/index.js` 判断又拍云配置是否存在，将读取到的又拍云配置插入 config 对象  
  （新版的配置载入并非直接载入所有配置，而且第一次初始化时并不会读取到又拍云配置）

配置
------
以下为示例
```
upyun: {
    bucketname: 'my-first-bucket', //空间名称
    username: 'somebody', //操作员名称
    password: 'secret', //密码
    root: '/images/', //文件存储在哪个目录。可以设置为 `/` 表示存储在根目录
    prefix: 'http://cdn.my-domainname.com'  //上传的文件的 URL 前缀，可以是你自己绑定的二级域名或者又拍云默认分配的二级域名。
}
```

----

##如何部署：

* 注册登录[Coding.net](http://coding.net)
* Fork https://coding.net/u/tayuo/p/Ghost-On-Coding-Paas-with-Upyun/
* 进入`演示` - `开始检测` - 勾选已阅读...并点`开启功能` - 设置`访问域名`，点保存

![](http://dn-tucdn.qbox.me/Px.png)

* 进入`服务管理` - 添加服务 - 选择`Mysql`并勾选`绑定创建后的服务到此项目(可选)`
* 创建成功后点`连接信息`，你需要把`hostname` `name` `password` `username`记录下来

* 回到`代码`找到`config.js`并点进去，点右边的`编辑`，把刚才记录的数据填入`connection`对应的位置，修改`url`为你设置的值，然后点`保存到Master分支`

![](http://dn-tucdn.qbox.me/Mx.png)

* 再回到`演示` - 在`部署版本`中填入`master`，点`一键部署`，稍等1分钟左右，当`当前状态`变为绿灯`正在运行`的时候，Ghost博客就部署完毕了！点`马上访问`就可以看到你的Ghost了，在网址后面加上`/ghost`注册成为你博客的主人吧！

----

上传文件在又拍云上的存储结构
------
首先计算上传文件的 md5 值，然后取 md5 值的第1位字符作为一级目录名称，取第2和3位作为二级目录名称，剩余的字符作为文件名。例如：

某上传文件的 md5 值为：6fb2a38dc107eacb41cf1656e899cf70；扩展名为 .jpg ；目录及文件组织结构为：6/fb/2a38dc107eacb41cf1656e899cf70.jpg

注意
------
* 该版本在上传文件后**不会**删除源图片（即被上传到 content/images 的图片），如果您希望删除，请在 `core/server/storage/upyun.js` 的第 33 行，第 7 个字符后插入。
```
  
    .then(function(） {
        return Promise.promisify(fs.unlink)(savedpath);
    }
    
```
**由于测试时因不明原因导致错误，请在测试后再放入生产环境**
  
* `content/images` 目录必须设置正确的写权限。
* 该版本**仅在 Ghost 0.5.2 下测试通过**，理论上兼容 0.5.x ，请自行测试
* 如果您在启动时自行定义配置文件地址（即以 Node 模块启动后传入 config 对象），请不要使用该版本，该版本仅兼容非配置文件自定义的 Ghost。


版权和许可证
------
* Ghost 代码版权为 Ghost 基金会 所有，遵循 [MIT 许可证](https://github.com/TryGhost/Ghost/blob/master/LICENSE) 发布。
* 该版本代码中由本人修改的部分版权为本人所有，遵循 [MIT 许可证](https://github.com/sanddudu/ghost-0.5.2-upyun/blob/master/LICENSE) 发布。
* [GhostChina 发布的 Ghost 0.4.2 又拍云集成](https://github.com/ghostchina/Ghost-0.4.2-upyun) 代码中由 Ghost中文网 编写的代码版权为 Ghost中文网 所有，遵循 [MIT 许可证](https://github.com/ghostchina/Ghost-0.4.2-upyun/blob/master/LICENSE) 发布
* 又拍云 SDK 版权为 又拍云存储 所有，无明确许可证
