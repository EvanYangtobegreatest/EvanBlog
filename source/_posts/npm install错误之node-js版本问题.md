---
title: npm install错误之node版本问题
date: 2022-05-22 09:11:27
author: Evan
categories: 笔记
tags:


---

## npm install时报错

```cmd
npm ERR! code 1
npm ERR! path C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\node-sass
npm ERR! command failed
npm ERR! command C:\Windows\system32\cmd.exe /d /s /c node scripts/build.js
npm ERR! Building: D:\work\nodejs\node.exe C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\node-gyp\bin\node-gyp.js rebuild --verbose --libsass_ext= --libsass_cflags= --libsass_ldflags= --libsa
ss_library=
npm ERR! gyp info it worked if it ends with ok
npm ERR! gyp verb cli [
npm ERR! gyp verb cli   'D:\\work\\nodejs\\node.exe',
npm ERR! gyp verb cli   'C:\\Users\\Evan\\Downloads\\vhr-master\\vhr-master\\vuehr\\node_modules\\node-gyp\\bin\\node-gyp.js',
npm ERR! gyp verb cli   'rebuild',
npm ERR! gyp verb cli   '--verbose',
npm ERR! gyp verb cli   '--libsass_ext=',
npm ERR! gyp verb cli   '--libsass_cflags=',
npm ERR! gyp verb cli   '--libsass_ldflags=',
npm ERR! gyp verb cli   '--libsass_library='
npm ERR! gyp verb cli ]
npm ERR! gyp info using node-gyp@3.8.0
npm ERR! gyp info using node@16.15.0 | win32 | x64
npm ERR! gyp verb command rebuild []
npm ERR! gyp verb command clean []
npm ERR! gyp verb clean removing "build" directory
npm ERR! gyp verb command configure []
npm ERR! gyp verb check python checking for Python executable "python2" in the PATH
npm ERR! gyp verb `which` failed Error: not found: python2
npm ERR! gyp verb `which` failed     at getNotFoundError (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:13:12)
npm ERR! gyp verb `which` failed     at F (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:68:19)
npm ERR! gyp verb `which` failed     at E (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:80:29)
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:89:16
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\isexe\index.js:42:5
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\isexe\windows.js:36:5
npm ERR! gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21)
npm ERR! gyp verb `which` failed  python2 Error: not found: python2
npm ERR! gyp verb `which` failed     at getNotFoundError (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:13:12)
npm ERR! gyp verb `which` failed     at F (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:68:19)
npm ERR! gyp verb `which` failed     at E (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:80:29)
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:89:16
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\isexe\index.js:42:5
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\isexe\windows.js:36:5
npm ERR! gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21) {
npm ERR! gyp verb `which` failed   code: 'ENOENT'
npm ERR! gyp verb `which` failed }
npm ERR! gyp verb check python checking for Python executable "python" in the PATH
npm ERR! gyp verb `which` failed Error: not found: python
npm ERR! gyp verb `which` failed     at getNotFoundError (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:13:12)
npm ERR! gyp verb `which` failed     at F (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:68:19)
npm ERR! gyp verb `which` failed     at E (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:80:29)
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:89:16
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\isexe\index.js:42:5
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\isexe\windows.js:36:5
npm ERR! gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21)
npm ERR! gyp verb `which` failed  python Error: not found: python
npm ERR! gyp verb `which` failed     at getNotFoundError (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:13:12)
npm ERR! gyp verb `which` failed     at F (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:68:19)
npm ERR! gyp verb `which` failed     at E (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:80:29)
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\which\which.js:89:16
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\isexe\index.js:42:5
npm ERR! gyp verb `which` failed     at C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\isexe\windows.js:36:5
npm ERR! gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21) {
npm ERR! gyp verb `which` failed   code: 'ENOENT'
npm ERR! gyp verb `which` failed }
npm ERR! gyp verb could not find "python". checking python launcher
npm ERR! gyp verb could not find "python". guessing location
npm ERR! gyp verb ensuring that file exists: C:\Python27\python.exe
npm ERR! gyp ERR! configure error
npm ERR! gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
npm ERR! gyp ERR! stack     at PythonFinder.failNoPython (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\node-gyp\lib\configure.js:484:19)
npm ERR! gyp ERR! stack     at PythonFinder.<anonymous> (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\node-gyp\lib\configure.js:509:16)
npm ERR! gyp ERR! stack     at callback (C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\graceful-fs\polyfills.js:295:20)
npm ERR! gyp ERR! stack     at FSReqCallback.oncomplete (node:fs:198:21)
npm ERR! gyp ERR! System Windows_NT 10.0.16299
npm ERR! gyp ERR! command "D:\\work\\nodejs\\node.exe" "C:\\Users\\Evan\\Downloads\\vhr-master\\vhr-master\\vuehr\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cfl
ags=" "--libsass_ldflags=" "--libsass_library="
npm ERR! gyp ERR! cwd C:\Users\Evan\Downloads\vhr-master\vhr-master\vuehr\node_modules\node-sass
npm ERR! gyp ERR! node -v v16.15.0
npm ERR! gyp ERR! node-gyp -v v3.8.0
npm ERR! gyp ERR! not ok
npm ERR! Build failed with error code: 1

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\Evan\AppData\Local\npm-cache\_logs\2022-06-30T02_19_44_135Z-debug-0.log

```

### 出错原因

node版本过高导致，可以通过node -v查看版本

### 解决方法

重新安装node.js

**1.找到node.js，卸载**

**2.到官网重新下载版本更低的node.js**

[官网地址](https://nodejs.org/zh-cn/download/releases/)

### 新的错误

这时候重新运行npm install，又出现新的报错

```cmd
npm ERR! cb() never called! 
npm ERR! This is an error with npm itself. 
```

### 解决方法

**1.删除项目文件的node_modules**
**2.删除package-lock.json文件**
 **3.清除npm缓存，执行npm cache clean --force**
 **4.再运行npm install**

## 成功

