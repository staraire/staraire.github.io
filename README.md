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
