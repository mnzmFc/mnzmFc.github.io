---
layout: post
title: My New Post
date: 2023-03-01 17:15 +0800
---
git clone -b分支 url
进入项目目录 git config credential.helper store 免密
git pull 更新代码
npm install --unsafe-perm=true --allow-root 安装依赖(以root身份)
或者yarn install
或者npm i -g yarn
打包
如果使用了[jsencrypt](https://github.com/travist/jsencrypt)这个依赖打包报错ERROR in ./node_modules/jsencrypt/lib/index.js 1:0-40，webpack配置里加这个
```
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.m?js$/,
        resolve: {
          fullySpecified: false, // disable the behaviour
        },
      },
    ],
  },
};
```
deploy.sh
```
check(){
        if [ $? -eq 1 ];then
                echo "上个命令执行失败，退出"
                exit 1
        fi
}
echo "开始打包"
rm -rf /var/lib/jenkins/jobs/front/yunping/prod/fh-cloud-screen-inner-h5/build
cd /var/lib/jenkins/jobs/front/yunping/prod/fh-cloud-screen-inner-h5
git pull
/usr/local/bin/yarn install
/usr/local/bin/yarn build:test
check

echo "开始发包"
originPath=/var/lib/jenkins/jobs/front/yunping/prod/fh-cloud-screen-inner-h5/build
scpPath=/www/wwwroot/10.2.175.25/h5
scpPort=22
scpUser=root
scpIp=10.2.175.25
\cp -f $originPath/*.zip ./history
unzip $originPath/*.zip -d $originPath/pakage
scp -r -P$scpPort $originPath/pakage/* $scpUser@$scpIp:$scpPath
rm -rf $originPath/pakage $originPath/*.zip
check

echo "成功"
```

