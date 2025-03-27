## themes

## hexo入门

用hexo创建博客

```bash
hexo init myblog
cd myblog
npm install
```

可以看到工程目录下

- **.github** github相关
  - `dependabot.yml` 定期扫描项目的依赖文件
- **node_modules** 依赖包(npm自动生成)
- **scaffolds** 模板
  - `post.md` 文章模板
  - `page.md` 页面模板
  - `draft.md` 草稿模板
- **source** 源文件
  - `_posts` 文章存放文件夹
    - `hello-world.md` 默认文章
- **themes** 主题
  - `.gitkeep` git上传占位用
- `_config.landscape.yml` 默认主题配置
- `_config.yml` 博客配置
- `.gitignore` git忽略文件
- `package-lock.json` 依赖包版本 由nmp自动生成 会锁定版本细节
  - 最好不要删除 删之前做好备份
- `package.json` 依赖包 指定插件及版本

### 默认主题

hexo初始化后默认主题是`landscape`, 根目录下的`_config.yml`配置是这样，还有`_config.landscape.yml`也是这个主题的配置(如不用这个主题可删掉)

### 初始插件

可以看到package.json中的初始插件
```
    "hexo": "^7.3.0",
    "hexo-generator-archive": "^2.0.0",
    "hexo-generator-category": "^2.0.0",
    "hexo-generator-index": "^4.0.0",
    "hexo-generator-tag": "^2.0.0",
    "hexo-renderer-ejs": "^2.0.0",
    "hexo-renderer-marked": "^7.0.0",
    "hexo-renderer-stylus": "^3.0.1",
    "hexo-server": "^3.0.0",
    "hexo-theme-landscape": "^1.0.0"
```

## 本地部署

### 准备环境

1、安装nodejs：版本18

https://zhuanlan.zhihu.com/p/105715224

2、安装hexo：

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

```bash
npm install hexo-cli -g
```

### 从零搭建

> 若没有工程文件，只有一些关键的文件，从零恢复工程

- 初始化工程

```bash
hexo init myblog
cd myblog
```

- 拷贝关键文件和删除不需要的文件
  - 拷贝替换博客配置文件`_config.yml`到`myblog`目录下
  - 拷贝替换依赖包`package.json`到`myblog`目录下
  - 拷贝替换`scaffolds` 模板文件到`myblog`目录下
  - 拷贝替换`source` 博客文件夹到`myblog`目录下
  - 拷贝`reamde.md`到`myblog`目录下
  - 删除`_config.landscape.yml`
  - 删除`package-lock.json`
  - 删除`themes/.gitkeep`

- 下载主题文件
  - next主题：`git clone https://github.com/next-theme/hexo-theme-next.git themes/next`
  - maupassant主题: `git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant`

- 执行 `npm install`


- hexo三连

```
hexo clean
hexo g & hexo s
```




### 快速部署

> 有完整的工程，可以快速部署

- 安装依赖

执行 `npm install`
若报错，删掉`package-lock.json`
重新执行 `npm install`

- hexo三连

```
hexo clean
hexo g & hexo s
```



### 一些报错问题

- 换设备

先将package.json里的
```
    "hexo-renderer-pug": "^3.0.0",
    "hexo-renderer-sass-next": "^0.1.3",
```

再执行
```
$ npm install hexo-renderer-pug --save
$ npm install hexo-renderer-sass-next --save
```


## 主题安装

- 安装主题
  - `next` 主题
    - `git clone https://github.com/next-theme/hexo-theme-next themes/next`
  - `maupassant` 主题
    - https://github.com/tufu9441/maupassant-hexo
    - https://www.haomwei.com/technology/maupassant-hexo.html

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

## 插件

以下是这些 Hexo 依赖项的详细功能解析，按用途分类说明：


### **一、核心框架**
| 包名 | 版本 | 作用 |
|------|------|------|
| `hexo` | `^6.3.0` | Hexo **核心引擎**，提供博客生成、部署等基础功能 |


### **二、生成器（Generators）**
| 包名 | 版本 | 作用 |
|------|------|------|
| `hexo-generator-index` | `^3.0.0` | **生成首页文章列表**（`index.html`），支持分页 |
| `hexo-generator-archive` | `^2.0.0` | 生成**归档页**（按时间排序的所有文章列表） |
| `hexo-generator-category` | `^2.0.0` | 生成**分类页**（如 `/categories/`） |
| `hexo-generator-tag` | `^2.0.0` | 生成**标签页**（如 `/tags/`） |
| `hexo-auto-category` | `^0.2.2` | **自动根据 `_posts` 子目录名设置文章分类** |



### **三、渲染器（Renderers）**
| 包名 | 版本 | 作用 |
|------|------|------|
| `hexo-renderer-marked` | `^6.0.0` | 将 Markdown 渲染为 HTML（支持表格、代码块等） |
| `hexo-renderer-ejs` | `^2.0.0` | 渲染 `.ejs` 模板文件（主题布局常用） |
| `hexo-renderer-pug` | `^3.0.0` | 渲染 `.pug` 模板文件（部分主题使用） |
| `hexo-renderer-stylus` | `^2.1.0` | 编译 `.styl` 样式文件（旧版主题常用） |
| `hexo-renderer-sass-next` | `^0.1.3` | 编译 `.scss/.sass` 样式文件（现代主题常用） |



### **四、功能增强**
| 包名 | 版本 | 作用 |
|------|------|------|
| `hexo-server` | `^3.0.0` | 启动本地服务器（`hexo server`）实时预览 |
| `hexo-generator-search` | `^2.4.3` | 生成**静态搜索索引**（供前端搜索功能使用） |
| `hexo-generator-searchdb` | `^1.4.1` | 将搜索数据存入数据库（与搜索插件配套使用） |
| `hexo-wordcount` | `^6.0.1` | **统计文章字数/阅读时长**（用于主题显示） |


### **五、主题相关**
| 包名 | 版本 | 作用 |
|------|------|------|
| `hexo-theme-next` | `^8.22.0` | **NexT 主题**（流行的 Hexo 主题框架） |


### **六、关键协作流程**
1. **内容生成**  
   - `hexo-generator-*` 系列插件生成不同页面（首页/分类/标签/归档）。  
   - `hexo-auto-category` 自动从目录结构提取分类。  

2. **内容渲染**  
   - Markdown → `hexo-renderer-marked` → HTML  
   - 模板文件 → `hexo-renderer-ejs/pug` → 最终页面  

3. **功能扩展**  
   - 搜索：`hexo-generator-search` + `hexo-generator-searchdb`  
   - 统计：`hexo-wordcount`  

4. **本地开发**  
   - `hexo-server` 提供实时预览  


### **七、常见问题**
1. **为什么需要多个渲染器？**  
   - 不同主题可能使用不同模板引擎（如 EJS/Pug），需匹配对应渲染器。

2. `hexo-generator-search` 和 `searchdb` 的区别？  
   - `search` 生成静态 JSON 索引，`searchdb` 提供数据库支持（适合大量内容）。

3. **如何禁用某个生成器？**  
   在 `_config.yml` 中设置：  
   ```yaml
   skip_render:
     - categories/**  # 禁用分类页生成
   ```


通过合理配置这些模块，你可以实现从内容创作到页面生成的完整工作流。


## 其他问题

> 1、github在push pull的时候connect timeout?

先执行
```bash
git config --global http.proxy
git config --global https.proxy
```
确保没有输出，如果有输出执行下面
```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```
然后设置代理
```bash
git config --global https.proxy 127.0.0.1:*****
git config --global http.proxy 127.0.0.1:*****
```