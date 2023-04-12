## themes

- `maupassant`
  - https://github.com/tufu9441/maupassant-hexo

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

```
$ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
$ npm install hexo-renderer-pug --save
```

```
npm uninstall hexo-renderer-sass
npm i --save hexo-renderer-sass-next
```

4、hexo三连

```
hexo clean
hexo g
hexo s
```
