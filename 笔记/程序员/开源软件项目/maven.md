[Maven工程打jar包的N种方式_Hugh_Guan的博客-CSDN博客_maven打jar包命令](https://blog.csdn.net/Hugh_Guan/article/details/110224621)

# 构建 \<build>
```bash
<resources> <resource> 资源文件管理
<targetPath> 资源打包路径，该路径相对于target/classes目录
<filtering> 是否将项目中资源文件的${XXX}替换为值，默认false
<directory> 存放资源的目录，相对于pom路径
<includes> <include> 包含的模式列表
<excludes> <exclude> 排除的模式列表

<plugins> <plugin>设置构建的插件
```
为什么我们需要插件？
1.需要某个特殊的jar包，但是有不能直接通过maven依赖获取，或者说在其他环境的maven仓库内不存在，那么如何将我们所需要的jar包打⼊我们的⽣产jar包中。
2.某个jar包内部包含的⽂件是我们所需要的，或者是我们希望将它提取出来放⼊指定的位置，那么除了复制粘贴，可以通过maven插件实现。

# properties 和 dependencyManagement
在通常情况下，由于我们的模块很多，我们需要⼀个itoken-denpendencies的项⽬来管理⼦项⽬的公共的依赖。为了项⽬的正确运⾏，必须让所有的⼦项⽬使⽤依赖项的统⼀版本，必须确保应⽤的各个项⽬的依赖项和版本⼀致，才能保证测试的和发布的是相同的结果。在我们项⽬顶层的POM⽂件中，我们会看到dependencyManagement元素。通过它元素来管理jar包的版本，让⼦项⽬中引⽤⼀个依赖⽽不dependencyManagement⽤显⽰的列出版本号。

>这样做的好处就是：如果有多个子项目都引用同一样依赖，则可以避免在每个使用的子项目里都声明一个版本号，这样当想升级或切换到另一个版本时，只需要在顶层父容器里更新，而不需要一个一个子项目的修改 ；另外如果某个子项目需要另外的一个版本，只需要声明version就可。

dependencyManagement里只是声明依赖，并不实现引入，因此子项目需要显式的声明需要用的依赖。

# POM之间的关系
## 依赖关系
```xml
<dependencies> <dependency>
<groupId>依赖项的组织名
<artifactId>依赖项的id
<version>依赖项的版本
<type>依赖类型，默认jar，可取值：jar，war，ejb-client，test-jar
<scope>依赖项的使用范围，可取值：compile，runtime，test，system，provided
<optional>true/false，可选依赖，依赖本项目的项目需要对此依赖显式引用
<exclusions>不依赖的项目，排除依赖冲突时使用               
<exclusion>                    
<artifactId>xx</artifactId>                    
<groupId>xx</groupId>                
</exclusion>     
```

## 继承关系
```xml
<parent>
<groupId>父项的组织名
<artifactId>父项的id
<version>父项的版本
<relativePath>父项的pom.xml路径，默认为../pom.xml
```

## 聚合关系
```xml
<modules> <module>
一条命令就能构建多个模块，注意子模块路径
<packaging>必须为pom，否则无法构建
```

# ${属性值}
## MAVEN内置属性
${basedir} 项目的根目录（包含pom.xml文件的目录）
${version} 项目版本

## POM属性
${project.build.sourceDirectory} 项目主源码目录，默认src/main/java
${project.build.directory} 项目构建输出目录，默认为target

## 自定义属性
\<properties\>
通过${xxx}来使用

## Settings属性
${settings.offline} 是否每次编译都去查找远程中心库，默认为false


## Java系统属性
${java.home} java的安装目录
${java.vm.name} java虚拟机实现名称
${os.version} 操作系统名称

## 环境变量属性
${env.JAVA_HOME} JAVA_HOME环境变量的值

# 基本信息
```xml
<modelVersion> 声明项目描述符遵循的POM模型版本，很少改变
<groupId> 组织的唯一标识
<artifactId> 项目id
<version> 版本号
<packaging> 打包类型，可取值：pom，jar，maven-plugin，ejb，war，ear，rar，par等
<name> 项目名称
<url> 项目主页
...
```

# 头信息 \<project\>\</project\>
xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd"
xmlns 命名空间
xmlns:xsi xml遵循的标签规范