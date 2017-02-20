---
title: Android JCenter仓库搭建
date: 2016-12-21 16:22:23
tags: [JCenter, Android, maven仓库]
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=455502271&auto=0&height=66"></iframe>

## JCenter准备
1. 用GitHub账号登陆[bintray](https://bintray.com "bintray")。
2. 新建仓库，Add New Repository

<!--more-->

## 库工程
1. 创建AS项目
2. 工程内创建Module(Android Librarys)
### 以下修改都是在Module内修改的
1. build.gradle
```
apply plugin: 'com.android.library'


buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
    }
}

android {
    compileSdkVersion 25
    buildToolsVersion "25.0.0"

    defaultConfig {
        minSdkVersion 15
        targetSdkVersion 25
        versionCode 1
        versionName "0.0.3"

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"

    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:25.0.1'
}

apply from: "bintrayUpload.gradle"
```

2. Module根文件夹下新建bintrayUpload.gradle，拷贝以下内容
```
apply plugin: 'com.github.dcendents.android-maven'
apply plugin: 'com.jfrog.bintray'

//加载属性文件
Properties properties = new Properties()
File localPropertiesFile = project.file("bintray.properties");
if (localPropertiesFile.exists()) {
    properties.load(localPropertiesFile.newDataInputStream())
}
File projectPropertiesFile = project.file("project.properties");
if (projectPropertiesFile.exists()) {
    properties.load(projectPropertiesFile.newDataInputStream())
}

//读取属性
def projectRepositoryName = properties.getProperty("project.repositoryName")
def projectName = properties.getProperty("project.name")
def projectGroupId = properties.getProperty("project.groupId")
def projectArtifactId = properties.getProperty("project.artifactId")
def projectVersionName = android.defaultConfig.versionName
def projectPackaging = properties.getProperty("project.packaging")
def projectSiteUrl = properties.getProperty("project.siteUrl")
def projectGitUrl = properties.getProperty("project.gitUrl")

def developerId = properties.getProperty("developer.id")
def developerName = properties.getProperty("developer.name")
def developerEmail = properties.getProperty("developer.email")

def bintrayUser = properties.getProperty("bintray.user")
def bintrayApikey = properties.getProperty("bintray.apiKey")
def bintrayOrganizationId = properties.getProperty("bintray.organizationId");

def javadocName = properties.getProperty("javadoc.name")

/*
*这句代码一定要加否则会出现如下错误
* Could not upload to 'https://api.bintray.com/content/coolcode/maven/LibUiBase/1.0.0/CommonLibrary/LibUiBase/1.0.0/LibUiBase-1.0.0.pom': HTTP/1.1 400 Bad Request [
message:Unable to upload files: Maven group, artifact or version defined in the pom file do not match the file path 'CommonLibrary/LibUiBase/1.0.0/LibUiBase-1.0.0.p
om']
* */
group = projectGroupId

// 配置生成POM.xml文件的参数
install {
    repositories.mavenInstaller {
        pom {
            project {
                name projectName
                groupId projectGroupId
                artifactId projectArtifactId
                version projectVersionName
                packaging projectPackaging
                url projectSiteUrl
                licenses {
                    license {
                        name 'The Apache Software License, Version 2.0'
                        url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
                    }
                }
                developers {
                    developer {
                        id developerId
                        name developerName
                        email developerEmail
                    }
                }
                scm {
                    connection projectGitUrl
                    developerConnection projectGitUrl
                    url projectSiteUrl
                }
            }
        }
    }
}

tasks.withType(Javadoc) {
    options.addStringOption('Xdoclint:none', '-quiet')
    options.addStringOption('encoding', 'UTF-8')
    options.addStringOption('charSet', 'UTF-8')
}

//生成sources.jar
task sourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier = 'sources'
}

task javadoc(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
}

//生成javadoc.jar
task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}

artifacts {
    archives javadocJar
    archives sourcesJar
}

//javadoc的配置
javadoc {
    options {
        encoding "UTF-8"
        charSet 'UTF-8'
        author true
        version projectVersionName
        links "http://docs.oracle.com/javase/7/docs/api"
        title javadocName
    }
}

/*
*userOrg为bintray账号信息里面的Organization Id
*repo为你创建的仓库名称
* 如果上述两个字段写错了，则会出现下面类似的错误
*Could not create package 'huangxuanheng/maven/fragmentstack': HTTP/1.1 404 Not Found [message:Repo 'maven' was not found]
*
*
* */
bintray {
    user = bintrayUser
    key = bintrayApikey
    configurations = ['archives']
    pkg {
        userOrg = bintrayOrganizationId
        repo = projectRepositoryName
        name = projectName
        websiteUrl = projectSiteUrl
        vcsUrl = projectGitUrl
        licenses = ["Apache-2.0"]
        publish = true
    }
}
```

3. 在Module根目录下，新建project.properties文件，添加以下内容，并自行修改对应参数
```
#project
#仓库名称，就是在bintray官网建立的仓库的名称
project.repositoryName=仓库名称
#bintray仓库里显示的项目名称
project.name=name
#项目组id，这个取的时候别跟别人的相冲突就行
project.groupId=com.***.***
#项目id,一般同project.name
project.artifactId=name
#打包类型
project.packaging=aar
#项目官方网站地址
project.siteUrl=https://github.com/lianghuiyong/CustomUI
#项目git地址
project.gitUrl=https://github.com/lianghuiyong/CustomUI
#生成的javadoc名称
javadoc.name=CustomUI.Doc
```
4. 在Module根目录下，新建bintray.properties文件，添加对应参数
```
#配置bintray账号相关信息
#bintray用户名,不是登陆邮箱,是个人中心右上角显示的名字
bintray.user=user
#bintray的ApiKey
bintray.apiKey=apiKey
#bintray的Organization Id
bintray.organizationId=organizationId


#配置开发者信息
#昵称
developer.id=lianghuiyong
#姓名
developer.name=lianghuiyong
#邮箱
developer.email=lianghuiyong@outlook.com
```

## 提交

1. gradlew install
2. gradlew bintrayUpload

## 将库添加到公共库
进bintray，打开仓库对应的项目
1. add to JCenter
2. send