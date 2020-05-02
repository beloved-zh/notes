# Git提交文件到GitHub

**使用git命令操作本地文件到GitHub**

## 1、先在GitHub上面创建仓库

![image-20200224161016352](http://image.beloved.ink/Typora/image-20200224161016352.png)

## 2、git init

进入要上传的文件夹，鼠标右键，选择Git Bash

![image-20200224161337182](http://image.beloved.ink/Typora/image-20200224161337182.png)

**git init    初始化本地git仓库**

**文件夹会有一个.git文件**

![image-20200224161704135](http://image.beloved.ink/Typora/image-20200224161704135.png)

## 3、git remote add origin

**Github上复制仓库地址**

**git remote add origin 仓库地址。绑定远程仓库**

![image-20200224162013864](http://image.beloved.ink/Typora/image-20200224162013864.png)

![image-20200224162138789](http://image.beloved.ink/Typora/image-20200224162138789.png)

## 4、git add

**使用git add 文件名**：将单个文件提交到缓存区

**git add .**   ：将所有文件提交到缓存区

![image-20200224162618006](http://image.beloved.ink/Typora/image-20200224162618006.png)

## 5、git commit

**git commit -m“解释说明”**：将文件提交到本地仓库

![image-20200224162941528](http://image.beloved.ink/Typora/image-20200224162941528.png)

## 6、git push

第一次要使用**git push -u origin master**：使用此命令登录GitHub账号

![image-20200224163106847](http://image.beloved.ink/Typora/image-20200224163106847.png)

==第二次提交不用登录直接git push==

# Git拉去GitHub文件到本地

## 1、git init

**初始化git仓库**

进入文件夹，鼠标右键，选择Git Bash

![image-20200224161337182](http://image.beloved.ink/Typora/image-20200224161337182.png)

**git init    初始化本地git仓库**

**文件夹会有一个.git文件**

![image-20200224161704135](http://image.beloved.ink/Typora/image-20200224161704135.png)

## 2、git remote add origin

**Github上复制仓库地址**

**git remote add origin 仓库地址。绑定远程仓库**

![image-20200224162013864](http://image.beloved.ink/Typora/image-20200224162013864.png)

![image-20200224162138789](http://image.beloved.ink/Typora/image-20200224162138789.png)

### 3、git pull

**git pull --rebase origin master**：将远程仓库文件拉去到本地

![image-20200224164749494](http://image.beloved.ink/Typora/image-20200224164749494.png)

![image-20200224164758041](http://image.beloved.ink/Typora/image-20200224164758041.png)