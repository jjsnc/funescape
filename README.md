1.1、初始化 npm 项目

npm init --save

1.2、包权限管理 

# 查看模块 owner, 其中 demo 为模块名称
$ npm owner ls demo

# 添加一个发布者, 其中 xxx 为要添加同学的 npm 账号
$ npm owner add xxx demo

# 删除一个发布者
$ npm owner rm xxx demo


1.3、发布版本

1.3.1、发布稳定版本
更新版本号共有以下选项（major | minor | patch | premajor | preminor | prepatch | prerelease) ，注意项目的git status 必须是clear，才能使用这些命令。

# major 主版本号，并且不向下兼容  1.0.0 -> 2.0.0
$ npm version major

# minor 次版本号，有新功能且向下兼容  1.0.0 -> 1.1.0
$ npm version minor

# patch 修订号，修复一些问题、优化等  1.0.0 -> 1.0.1
$ npm version patch

# premajor 预备主版本  1.0.0 -> 2.0.0-0
$ npm version premajor

# preminor 预备次版本  1.0.0 -> 1.1.0-0
$ npm version major

# prepatch 预备修订号版本  1.0.0 -> 1.0.1-0
$ npm version major

# prerelease 预发布版本  1.0.0 -> 1.0.0-0
$ npm version major

版本号更新后，我们就可以进行版本的发布


$ npm publish

1.3.2、预发布版本


很多时候一些新改动，并不能直接发布到稳定版本上（稳定版本的意思就是使用 npm install demo 即可下载的最新版本），这时可以发布一个 “预发布版本“，不会影响到稳定版本。

# 发布一个 prelease 版本，tag=beta
$ npm version prerelease
$ npm publish --tag beta



比如原来的版本号是 1.0.1，那么以上发布后的版本是 1.0.1-0，用户可以通过 npm install demo@beta  或者 npm install demo@1.0.1-0  来安装，用户通过 npm install demo 安装的还是 1.0.1 版本。



1.3.3、将 beta 版本设置为稳定版本


# 首先可以查看当前所有的最新版本，包括 prerelease 与稳定版本
$ npm dist-tag ls

# 设置 1.0.1-1 版本为稳定版本
$ npm dist-tag add demo@1.0.1-1 latest


这时候，latest 稳定版本已经是 1.0.1-1 了，用户可以直接通过 npm install demo 即可安装该版本

1.3.4、将 beta 版本移除


# 将 beta 版本移除
$ npm dist-tag rm demo beta


1.3.5、将 tag 推送到 Git 远程仓库中


# 当我们发布完对应的版本，可以通过以下命令将版本号推送到远程仓库, 其中 xxx 为对应分支
$ git push origin xxx --tags


1.4、查看版本信息
可以通过 npm info 来查看模块的详细信息。

$ npm info


二、使用 typescript

$ npm i typescript -D

2.2、增加配置文件 tsconfig.json


根目录新建 tsconfig.json，配置项具体的意义可以参考 ts 官方文档


2.3、安装 @types/node

安装 @types/node 让 node 的核心包具备类型提示


$ npm i @types/node -D


2.4、新建入口文件


在根目录新建 src 目录，用于存放所有的 TypeScript 源文件，然后在 src 下新建 index.ts 作为入口文件

// src/index.ts

console.log('hello npm-sdk!');


2.5、安装 ts-node-dev


在开发阶段为了能直接执行并且监听 ts 文件的变化，安装 ts-node-dev

$ npm i ts-node-dev -D


在 package.json 中定义一个启动脚本

"scripts": {
    "start": "ts-node-dev --respawn --transpile-only src/index.ts"
}


三、使用 eslint 校验

3.1、安装 eslint

$ npm i eslint -D

3.2、eslint 初始化

$ ./node_modules/.bin/eslint --init


根据交互命令提示对应生成配置文件如下所示，可以根据团队的代码风格进行对应的配置 .eslintrc.js


3.3、添加忽略文件 .eslintignore



3.4、script 命令配置

可以通过在 package.json 中配置对应的校验命令和修复命令，如下所示

  "scripts": {
    "lint": "eslint --ext .ts .",
    "lint:fix":"eslint --fix --ext .ts ."
  },


3.5、提交校验

利用 commitlint 和 husky 工具进行代码提交时拦截验证，安装如下


$ npm i @commitlint/cli @commitlint/config-conventional husky lint-staged --D

在 package.json 中进行对应的配置，当 commit 代码时，如果代码中存在 eslint 错误，那么就会进行报错提示


四、编译

我们可以增加对应的 typescript 编译命令，如下所示


"scripts": {
  "build:cjs": "tsc --outDir lib",
  "build:es": "tsc -m esNext --outDir esm",
  "build": "rd /s /q lib esm && npm run build:cjs && npm run build:es",
},


配置对应的入口地址，其中 module 和 main 的区别是，module 主要在 tree shaking 时会用到。


配置对应的入口地址，其中 module 和 main 的区别是，module 主要在 tree shaking 时会用到


  "main": "lib/index.js",
  "module": "esm/index.js",


五、本地调试

可以通过 npm link 在正式项目中进行调试，在我们的包目录中安装完发布的线上包后，可以执行以下命令将当前项目 node_modules 底下安装的对应包关联到本地全局 npm 目录的 node_modules 目录下，命令如下


$ npm link npm-sdk@1.0.1-0




