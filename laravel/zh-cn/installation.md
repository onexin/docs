# 安装

- [安装](#installation)
    - [服务器要求](#server-requirements)
    - [安装Laravel](#installing-laravel)
    - [配置](#configuration)- [Web服务器配置](#web-server-configuration)
    - [目录配置](#directory-configuration)
    - [漂亮的网址](#pretty-urls)

## 安装 {#installation}
### 服务器要求 {#server-requirements}
Laravel框架有一些系统要求。[Laravel Homestead](/docs/master/homestead)虚拟机可以满足所有这些要求，因此强烈建议您将Homestead用作本地Laravel开发环境。
但是，如果您不使用Homestead，则需要确保您的服务器满足以下要求：
<div class="content-list">

- PHP&gt;=7.3
- BCMath PHP扩展
- Ctype PHP扩展
- Fileinfo PHP扩展名
- JSON PHP扩展
- Mbstring PHP扩展
- OpenSSL PHP扩展
- PDO PHP扩展
- Tokenizer PHP扩展
- XML PHP扩展

</div>
### 安装Laravel {#installing-laravel}
Laravel利用[Composer](https://getcomposer.org)管理其依赖项。因此，在使用Laravel之前，请确保已在计算机上安装了Composer。
#### 通过Laravel安装程序
首先，使用Composer下载Laravel安装程序：
```
composer global require laravel/installer
```
请确保将Composer的系统范围的供应商bin目录放置在您的`$PATH`中，以便系统可以定位laravel可执行文件。根据您的操作系统，该目录位于不同的位置；但是，一些常见的位置包括：
<div class="content-list">


- macOS：`$HOME/.composer/vendor/bin`
- Windows：`%USERPROFILE%\AppData\Roaming\Composer\vendor\bin`
- GNU /Linux发行版：`$HOME/.config/composer/vendor/bin`或`$HOME/.composer/vendor/bin`

</div>
您还可以通过运行`composer global about`并从第一行开始查找，从而找到作曲家的全局安装路径。
安装后，`laravel new`命令将在您指定的目录中创建全新的Laravel安装。例如，`laravel new blog`将创建一个名为`blog`的目录，其中包含一个全新的Laravel安装，其中已经安装了所有Laravel的依赖项：
```
laravel new blog
```
#### 通过Composer创建项目
或者，您也可以通过在终端中发出Composer`create-project`命令来安装Laravel：
```
composer create-project --prefer-dist laravel/laravel blog
```
#### 本地开发服务器
如果您在本地安装了PHP，并且想使用PHP的内置开发服务器来服务您的应用程序，则可以使用`serve`Artisan命令。此命令将在`http://localhost:8000`：处启动开发服务器。
```
php artisan serve
```
可通过[Homestead](/docs/master/homestead)和[Valet](/docs/master/valet)获得更强大的本地开发选项。
### 配置 {#configuration}
#### 公共目录
安装Laravel之后，您应该将Web服务器的文档/Web根目录配置为`public`目录。此目录中的`index.php`充当进入您应用程序的所有HTTP请求的前端控制器。
#### 配置文件
Laravel框架的所有配置文件都存储在`config`目录中。每个选项都有文档记录，因此可以随时浏览文件并熟悉可用的选项。
#### 目录权限
安装Laravel之后，您可能需要配置一些权限。 Web服务器应该可以写入`storage`和`bootstrap/cache`目录中的目录，否则Laravel将无法运行。如果您正在使用[Homestead](/docs/master/homestead)虚拟机，则应该已经设置了这些权限。
#### 应用密钥
在安装Laravel之后，您应该将应用程序密钥设置为随机字符串。如果您通过Composer或Laravel安装程序安装了Laravel，则`php artisan key:generate`命令已经为您设置了此密钥。
通常，此字符串应为32个字符长。可以在`.env`环境文件中设置密钥。如果尚未将`.env.example`文件复制到名为`.env`的新文件，则应立即执行此操作。**如果未设置应用程序密钥，则您的用户会话和其他加密数据将不安全！**
#### 其他配置
Laravel几乎不需要其他任何配置。您可以自由地开始开发！但是，您可能希望查看`config/app.php`文件及其文档。它包含几个选项，例如`timezone`和`locale`，您可能希望根据自己的应用程序进行更改。
您可能还想配置Laravel的一些其他组件，例如：
<div class="content-list">


- [缓存](/docs/master/cache#configuration)
- [数据库](/docs/master/database#configuration)
- [会话](/docs/master/session#configuration)

</div>## Web服务器配置 {#web-server-configuration}
### 目录配置 {#directory-configuration}
Laravel应该始终在为您的Web服务器配置的&quot;web目录&quot;的根目录之外提供。您不应尝试从&quot;web目录&quot;的子目录中提供服务Laravel应用程序。尝试这样做可能会暴露应用程序内存在的敏感文件。
### 漂亮的网址 {#pretty-urls}
#### Apache
Laravel包含`public/.htaccess`文件，该文件用于提供URL，而路径中没有`index.php`前控制器。在为Apache提供Laravel之前，请确保启用`mod_rewrite`模块，以便服务器可以使用`.htaccess`文件。
如果Laravel随附的`.htaccess`文件不适用于您的Apache安装，请尝试以下替代方法：
```
Options +FollowSymLinks -Indexes
RewriteEngine On

RewriteCond %{HTTP:Authorization} .
RewriteRule .* -[E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php[L]
```
#### Nginx
如果您使用的是Nginx，则网站配置中的以下指令会将所有请求定向到`index.php`前端控制器：
```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```
使用[宅基](/docs/master/homestead)或[代客](/docs/master/valet)时，会自动配置漂亮的URL。
