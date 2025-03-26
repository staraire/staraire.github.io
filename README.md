## themes

- `maupassant`
  - https://github.com/tufu9441/maupassant-hexo
  - https://www.haomwei.com/technology/maupassant-hexo.html

## 本地部署

1、安装nodejs：版本18

https://zhuanlan.zhihu.com/p/105715224

2、安装hexo：

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

```
npm install hexo-cli -g
```

3、安装主题和依赖

- 第一次部署要执行

```
$ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
$ npm install hexo-renderer-pug --save
$ npm install hexo-renderer-sass-next --save
```

```
npm uninstall hexo-renderer-sass
npm i --save hexo-renderer-sass-next
```

- 换设备

先将package.json里的
```
    "hexo-renderer-pug": "^3.0.0",
    "hexo-renderer-sass-next": "^0.1.3",
```
这两个先删除
然后执行 `npm install`
若报错，删掉`package-lock.json`
再执行
```
$ npm install hexo-renderer-pug --save
$ npm install hexo-renderer-sass-next --save
```


4、hexo三连

```
hexo clean
hexo g
hexo s
```

5、其他依赖，选择执行

- 搜本站博文
  - `self_search:true 另外两个搜索(谷歌 百度)关掉`
  - `npm install hexo-generator-search --save`

## 主题安装

- 安装主题
  - next主题
    - `git clone https://github.com/next-theme/hexo-theme-next themes/next`

- 切换主题
  - 在根目录下的`_config.yml`里改写`theme: hexo`

## 两个配置文件

### 站点配置文件

`/_config.yml`

- 博客中的基本信息
  - `title` 网站标题
  - `subtitle` 网站副标题
  - `description` 网站描述
  - `keywords` 网站关键字
  - `auther` 作者
  - `language` 网站使用的语言
  - `timezone` 时区

- 缩短文章链接长度
  - `url` 网址
  - `root` 网站根目录
  - `permalink` 文章的永久链接
  - `permalink_defaults` 永久链接中各部分的默认值
  - `pretty_urls` 改写permalink的值来美化url
    - `trailing_index` 是否在永久链接中保留尾部的index.html 设置为false时去除
    - `trailing_html` 是否在永久链接中保留尾部的html，设置false时去除
- 启用主题
  ```
  # Extensions
  ## Plugins: https://hexo.io/plugins/
  ## Themes: https://hexo.io/themes/
  theme: next
  ```

### 主题配置文件

`themes/next/_config.yml`