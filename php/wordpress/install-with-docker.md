# Docker安装WordPress

## 基础安装

**默认Docker已安装**

### 下载wordpress镜像
```
docker pull wordpress
```

### 启动wordpress
这里采用了已有的外部mysql数据库
```
docker run --network host -v ~/wordpress:/var/www/html --name mywordpress -e WORDPRESS_DB_HOST=<host>:<port> -e WORDPRESS_DB_USER=<database username> -e WORDP
RESS_DB_PASSWORD=<database password> -e WORDPRESS_DB_NAME=<db table> -d wordpress
```
镜像启动后，将自动替换wordpress框架中的wp-config.php文件中数据库的配置项。通过挂载的`~/wordpress`文件夹可以查看和修改代码（docker容器中没有安装编辑器）。  
到此为止，wordpress就算启动起来了。

### 执行wordpress安装程序
通过IP或域名访问wordpress站点，根据安装步骤完成站点基础配置。 

## 更多配置
### 安装语言包
根据官方说法4.1以后在后台可配置语言，但是进入管理后台，在“setting”菜单下竟然没有language的配置项。两种安装语言包方式：
* 方法1： 自行下载中文语言包安装后，将下载的语言包解压后放入`wp-content/languages`目录中
* 方法2： 参考[workdpress中文站](https://cn.wordpress.org/switching/)，修改`wp-config.php`文件，修改配置项后，管理后台的Dashboard中会提示升级并自动下载安装语言包
```
define('WPLANG', 'zh_CN');
```

### 无FTP上传皮肤
1. 设置文件夹权限
```
chmod -R 777 wp-content 
```
2. 在wp-config.php末尾加入如下配置：
```
define("FS_METHOD", "direct");
define("FS_CHMOD_DIR", 0777);
define("FS_CHMOD_FILE", 0777);
```

### 重新发布镜像
```
docker commit  <wordpress-container-name> <new-image-name>[:<tag>]
```
### 后续将直接启动容器即可
```
docker start mywordpress
```

## 常见错误处理
### 未能与站点联系来检查致命错误，因此PHP修改已被回滚。您需要采用其他方式（如SFTP）上传您修改的PHP文件
打开wp-admin/includes/file.php程序文件，删除505-616行的如下代码：
```
if ( $is_active && 'php' === $extension ) {
//省略代码
}
```
[参考](https://zmingcx.com/unable-to-communicate-back-with-site-to-check-for-fatal-errors.html)

## 参考资料
[WordPress Docker Library](https://github.com/docker-library/wordpress)  
[WordPress Dockerfile](https://github.com/docker-library/wordpress/tree/master/php7.4/apache)
