演示将lib打包成aar并上传到本地maven的例子

步骤：
1、在library的build.gradle文件中添加以下代码，然后Sync。

// 发布到本地库 ----begin-----------------------------------------
apply plugin: 'maven'
uploadArchives{
    repositories {
        mavenDeployer {
            repository(url: uri('../repository'))
            pom.groupId = "com.lib"
            pom.artifactId = "sdk"
            pom.version = "1.0.1" //改动代码要升级版本号，否则不会重新打包
        }
    }
}
// 发布到本地库 ---- end -----------------------------------------

2、执行uploadArchives, gradlew uploadArchives
uploadArchives是gradle的一个task的实例，只需要执行这个task，就会先将library打成aar包，并且上传到我们配置的文件夹中。

3、在其它app中import上传的aar

3.1 在project的build.gradle文件中添加仓库url：

allprojects {
    repositories {
        google()
        jcenter()

        //新增本地仓库路径，绝对路径或相对路径都可以
        maven { url "../repository" }
        // 如果url为 user_home/.m2/repository 则可以使用
        // mavenLocal()
    }
}

3.2 在app的build.gradle文件中添加依赖：
dependencies{
 // ... 其它依赖
 implementation 'com.lib:sdk:1.0.1' // 与uploadArchives task中配置的groupId、artifactId、version 一一对应
}