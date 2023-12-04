````c
//代码提交
git add .
git commit -m "Z220008-3382 #comment ***"
git push
    
//
git remote update origin --prune
    
//代码commit后撤销
git reset --soft HEAD^
    
//初始化本地代码
git init
git remote add origin url
git pull origin master
//安装依赖包
yarn install
//更改git 作者信息
git config --global user.name "王邱婷"
git config --global user.email "wangqiuting@cmbyc.com"
````



