# Mirror To CodeHub 
[![Mirror to CodeHub](https://github.com/mars167/mirror_to_codehub/actions/workflows/main.yml/badge.svg)](https://github.com/mars167/mirror_to_codehub/actions/workflows/main.yml)
1. 在CodeHub上创建一个空仓库
> 不要选择gitignore文件  
> 不要选择`允许生成README文件`  

![image](https://user-images.githubusercontent.com/29228178/195547639-ae60c6b9-eadf-441c-a95b-0ed42aa33183.png)
2. 在需要同步的仓库中添加一个Github Action, 将文件中的目标仓库地址替换为自己的
```yml 
on:
  push:
    branches:
    - '**'
name: Push to CodeHub
jobs:
  push_to_codehub:
    name: Push to CodeHub
    if: "!github.event.deleted"
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2-beta
      with:
        fetch-depth: 0 # Zero indicates all history
        ref: main
    - name: Push to mirror
      run: |
        git fetch -pu +refs/*:refs/* 
        git push --mirror -f https://${{ secrets.CODEHUB_USER }}:${{ secrets.CODEHUB_PASSWORD }}@codehub.devcloud.huaweicloud.com/foo/bar.git
```
3. 在`Settings->Secrets->Actions`添加`CODEHUB_USER`和`CODEHUB_PASSWORD`
> `CODEHUB_USER` (在此查看)[https://devcloud.huaweicloud.com/codehub/https], 注意需要将用户明包含的'/'转义成'%2F'，`CODEHUB_PASSWORD`也同样需要转义
