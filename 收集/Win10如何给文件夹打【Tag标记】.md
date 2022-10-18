# 怎样给win10文件夹添加标记

1. 下载bat文件

   [下载链接](https://drive.google.com/file/d/1ielPA2kVfVDe4GAjprqtuO5dcX3qJyz3)

2. 将文件放到一个不常操作的地方，我放到了C盘-用户-下载-Documents下面

   ![image-20221018211104787](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20221018211104787.png)

3. 复制文件地址

   ![image-20221018211326700](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20221018211326700.png)

4. WIN+R运行`regedit`，打开注册表编辑器，找到以下位置

   `计算机\HKEY_CLASSES_ROOT\Directory\Background\shell`

   在`shell`下新建**项**`Tag Folder`，再在`Tag Folder`下新建**项**`command`，如图所示：

   ![image-20221018211608889](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20221018211608889.png)

   ![image-20221018211825321](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20221018211825321.png)

   点击确定之后，就配置好标记功能了。

5.  去到任意一个文件夹下，右键菜单里多了一项打标记的选项`Tag Folder`

6. 输入内容，点击OK

    ![image-20221018212215211](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20221018212215211.png)

   ![image-20221018212443121](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20221018212443121.png)



**这样就有了一个Tag标记系统，可以对一些不方便改名的文件夹自定义添加一些说明，方便自己查阅。**