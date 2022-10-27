代码即脚本，Gradle 脚本采用 Groovy 书写，build.gradle，setting.gradle
- 添加Maven仓库

```groovy
repositories {
	//google()
	// 使用 Maven 远程仓库
	maven { url "http://repo.mycompany.com/maven2" }
	// 使用 Maven 中央仓库
    mavenCentral()
}
```

- 添加依赖
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

# 引入插件方式
- plugins必须在Top Level（父项目或子项目的gradle.build顶级），直接使用:
```groovy
plugins {
    id 'org.springframework.boot' version '2.7.2'
    id 'io.spring.dependency-management' version '1.0.12.RELEASE'
    id 'java'
}
```

- apply与buildscript结合
```groovy
buildscript {
    ext {
        springBootVersion = "2.3.3.RELEASE"
    }
}
apply plugin: 'java' //核心插件，无需事先引入
apply plugin: 'org.springframework.boot' //社区插件，需要事先引入，不必写版本号
```

- 父项目的 `subprojects` 块中指定子项目中使用
```groovy
// 此处也可与buildscripts和apply plugin结合
plugins {
    id 'java'
    id 'org.springframework.boot' version '2.4.2'
}
//所有的子项目，都应用了以下插件
subprojects {
    apply plugin: 'java' //核心插件，无需事先引入
    apply plugin: 'org.springframework.boot' //社区插件，需要事先引入，不必写版本号
}
```

- 在父项目中指定了插件的情况下（父项目中使用了`plugins`或者`buildscript+apply`）,此时，可以直接在子项目中直接使用apply
```groovy
//这是子项目的位置
apply plugin: 'java' //核心插件，无需事先引入
apply plugin: 'org.springframework.boot' //社区插件，需要事先引入，不必写版本号
```

# configurations
配置依赖项的作用范围
