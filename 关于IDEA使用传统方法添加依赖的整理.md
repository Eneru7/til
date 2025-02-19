Servlet实际上是sun公司制定的一系列web相关的Java接口，实际开发中一般用自定义的类去实现`HttpServlet`。
Servlet相关的类位于`servlet-api.jar`文件中。该文件也是Tomcat的lib目录下就携带的众多jar包之一。一般Servlet相关的开发引用这个包就足够了。
对于在IDEA中使用这个包，需要做一些特殊说明，以下的操作都没有硬性规定，用任何一种方式都可以达到目的。但也是由于没有硬性规定，导致不同的操作方式让人产生疑惑。

首先，在使用IDEA创建项目进行初始开发时，是没有引用任何外部依赖的。在IDEA的`setting -> Build -> Application Servers`中，可以通过选择电脑中Tomcat的目录的方式为整个工程添加一个Library，默认命名方式是`Tomcat [version]`(Tomcat 9.0.93)。一个Library可以看作是一个依赖的集合体，一般这个集合体中只包含`jsp-api.jar`和`servlet-api.jar`两个文件。

同时，这个名为Tomcat 9.0.93的Library是存放在全局的`Application Servers Libraries`中。在`Project structure`中为模块以Library选项添加依赖时，可以看到。这个依赖默认的Scope是**Provided**。意味着构建时**servlet-api.jar**不会放入Web/lib目录下。把这个web_exploded项目放到Tomcat的webapps下是可以正常运行的，因为Tomcat\lib下自带有**servlet-api.jar**文件，可以全局共享给需要的webapp。

同时也要介绍常规的为lib项目添加依赖的方法，一般是在web下新建lib目录，将jar文件放在lib目录中，同时为所属模块添加Library。或者执行`Add as Library`动作。

借此还要介绍在IDEA的工程结构->工程设置->模块->Dependencies中添加依赖的三种方式的区别。
1. JARs or Directories
	1. 如果在资源管理器中选择的是JAR文件，那么JAR文件就被作为依赖添加到External Libraries中，以单个JAR文件的形式呈现。
	2. 如果在资源管理器中选择的是文件夹Directory，那么该文件夹下的JAR文件全部添加到External Libraries中，也是以JAR文件的形式呈现。
2. Library
	1. 如果全局的Application Servers Libraries中存在依赖集合，那么可以选择该Library加入External Libraries。已Library 文件夹的形式呈现。
	2. 也可以新建Library。一般在资源管理器中选择一个包含多个JAR文件的文件夹。加入External Libraries，已Library 文件夹的形式呈现。
3. Module Dependency
	1. 添加已有的模块作为依赖，类似Maven的效果。