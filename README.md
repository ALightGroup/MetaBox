## 项目介绍
MetaApp是一个工具类集合软件，主要特点有：
1、完全的material3风格的实现
2、jetpac最佳实践
3、每个工具都是一个单独的插件

## 工程配置

1、clone
```sh
git clone git@github.com:ALightGroup/MetaBox.git
git submodule update --init --recursive
```

2、目录结构
+ MetaBoxApp  （主app）
+ MetaPlugins （插件目录）
  - WeakNetPlugin
  - FileManagerPlugin 
  - ...

3、版本依赖配置
a、发布版本管理插件[VersionManager](https://github.com/ALightGroup/VersionManager)，执行下面命令发布通用版本到本地仓库
```
./gradlew build publishToMavenLocal
```

b、发布基础组件插件[MetaFram](https://github.com/ALightGroup/MetaFrame)
```
./gradlew build publishToMavenLocal
```

## 创建一个插件
1、创建一个普通的安卓项目
2、在`setting.gradle`添加统一版本管理插件
```groovy
pluginManagement {
  repositories {
    gradlePluginPortal()
    google()
    mavenCentral()
    mavenLocal()
  }
}
enableFeaturePreview('VERSION_CATALOGS')
dependencyResolutionManagement {
  repositories {
    google()
    mavenCentral()
    mavenLocal()
  }

  versionCatalogs {
    libs {
      from("com.alg.plugin.version:catalog:0.0.1")
    }
  }
}
```

3、在`build.gradle`文件添加以下命令，让`catalogs`支持自动补全
```groovy
buildscript {
  ext {
    compose_version = '1.1.1'
  }
  dependencies {
    classpath files(libs.class.superclass.protectionDomain.codeSource.location)
  }
}
```

4、将插件仓库添加到`MetaApp`的插件中
```sh
git submodule add <url> <path>
```
其中，url为子模块的路径，path为该子模块存储的目录路径。
执行成功后，git status会看到项目中修改了.gitmodules，并增加了一个新文件（为刚刚添加的路径）
`git diff --cached`查看修改内容可以看到增加了子模块，并且新文件下为子模块的提交hash摘要


## 其它

本工程使用的代码风格是[java-code-style](https://github.com/square/java-code-styles)，提交pr之前请使用该风格格式化代码。
