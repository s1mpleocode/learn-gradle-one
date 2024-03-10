# Gradle学习笔记

## Gradle和Maven目录结构区别

![image-20240310131901029](https://markdownresource.oss-cn-beijing.aliyuncs.com/markdown/202403101319195.png)

## 自动化构建工具对比

![image-20240310141453318](https://markdownresource.oss-cn-beijing.aliyuncs.com/markdown/202403101414436.png)

## Gradle的几种依赖配置

### 1.implementation

会将指定的依赖添加到编译路径，并且会将该依赖打包到输出，如apk中，但是这个依赖在编译时不能暴露给其他模块，例如依赖此模块的其他模块。这种方式指定的依赖在编译时只能在当前模块中访问。模块A依赖模块B，B依赖库C，模块B在编译时能够访问到库C，但是模块A无法访问库C。

### 2. api

使用api配置的依赖会将对应的依赖添加到编译路径，并将依赖打包输出，但是这个依赖是可以传递的，模块A依赖模块B，B依赖库C，模块B在编译时能够访问到库C，但是与implemetation不同的是，在模块A中库C也是可以访问的。

### 3.complileOnly

compileOnly修饰的依赖会添加到编译路径中，但是不会打包到apk中，因此只能在编译时访问，且compileOnly修饰的依赖不会传递。如果A依赖B，B compileOnly C，A不可以使用C的接口。

比如定义一个接口使用compileOnly，编译时使用不会报错，不提供具体实现，打包成apk供其他开发者自己实现接口

### 4.compileOnlyApi

与compileOnly相同，compileOnlyApi修饰的依赖会添加到编译路径中，但是不会打包到apk中，且compileOnlyApi修饰的依赖会传递。如果A依赖B，B compileOnlyApi C，A也使用C的接口。

### 5.runtimeOnly

与compileOnly相反，它修饰的依赖不会添加到编译路径中，但是被打包到apk中，运行时使用。

比如mysql数据库驱动，编译时不需要它，运行时操作数据库时需要

### 6.annotationProcessor

用于注解处理器的依赖配置

比如如果使用了Lombok，它可以自动生成 getter、setter、equals、hashCode 和 toString 等方法，就要配置注解处理器

```groovy
dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.0'
    //lombok的注解处理器
    annotationProcessor 'org.projectlombok:lombok:1.18.0'
}
```

### 7.testImplementation

用于声明项目的测试编译和运行时所需的依赖。这些依赖仅在测试编译和测试运行时被包含，而在主应用程序的编译和运行时则不会被包含。

```groovy
dependencies {
    //junit作为测试依赖
    testImplementation 'junit:junit:4.12'
}
```

### 8.testCompileOnly

仅在测试编译时需要的依赖，而不需要在测试运行时

### 9.testRuntimeOnly

仅在测试运行时需要的依赖，而不需要在测试编译时