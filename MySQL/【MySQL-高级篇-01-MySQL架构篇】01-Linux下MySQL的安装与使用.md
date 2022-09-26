# Linux下MySQL的安装与使用

## 1. 安装前的说明

1. 安装并启动好两台虚拟机：`CentOS7`

2. 掌握克隆虚拟机的操作，克隆后需要进行一下操作

   1. 修改MAC地址

      具体步骤，选择已安装好的虚拟机，如图点击红色框框选的按钮，最后点击确定即可

      ![image-20220925143523734](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220925143523734.png)

      ![image-20220925143608502](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220925143608502.png)

   2. 修改主机名

      1. 进入虚拟机，打开终端
      2. 键入`vim /etc/hostname`（最好掌握Linux下vim的操作）
      3. 按`i`，然后进行修改主机名（为了后续学习主从复制，更好辨别）
      4. 按`Esc`退出编辑，按`:wq`保存并退出
      5. 键入`reboot`，重启，更新主机名

   3. 修改ip地址

      ==需要注意==：如果虚拟机使用的是动态ip分配，那么不需要更改ip，如果想改为静态ip，请修改：

      `vim /etc/sysconfig/network-scripts/ifcfg-ens33`

   4. 修改UUID



## 5. 字符集相关操作

### 5.1 修改MySQL5.7字符集

#### 1. 修改步骤

在MySQL8.0版本之前，默认字符集为==latin1==，utf8字符集指向的是==utf8mb3==。网站开发人员在数据库设计的时候往往将编码修改为==utf8字符集==。如果遗忘修改默认的编码，就会出现乱码的问题。从MySQL8.0开始，数据库的默认编码将改为==utf8mb4==，从而避免上述乱码的问题。

==操作1：查看默认使用的字符集==

```mysql
show variable like 'character%';
# 或者
show variable like 'char%';
```



