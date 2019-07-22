## 平安SDK接入文档

### 1、接入SDK

- 配置仓库

```gradle
allprojects {
    repositories {
        maven { 
            url "https://raw.githubusercontent.com/Denglu1Dev/Denglu1SDK/master" 
        }
    }
}
```
- 配置依赖

```gradle
implementation "com.denglu1.sdk:pingan:1.0.0"
```
- 依赖传递

SDK依赖一些常用Android开发库，这里列出来说明一下。由于依赖传递一般都是默认开启的，所以，只需添加上面一条依赖即可，下面的不用添加。

```gradle
//安卓支持库，一般App都有，版本可以按照自己的配置
implementation "com.android.support:appcompat-v7:27.1.1+"
implementation "com.android.support:support-annotations:27.1.1+"
implementation "com.android.support:design:27.1.1+"
//网络开发库，一般App都有，版本可以按照自己的配置
implementation "com.squareup.retrofit2:retrofit:2.4.0+"
implementation "com.squareup.retrofit2:adapter-rxjava2:2.4.0+"
implementation "com.squareup.retrofit2:converter-gson:2.4.0+"
implementation "com.google.code.gson:gson:2.8.2+"
//rx库
implementation "io.reactivex.rxjava2:rxjava:2.2.6+"
implementation "io.reactivex.rxjava2:rxandroid:2.1.1+"

```
### 2、配置服务器参数

- 在Application子类#onCreate()方法中配置域名，端口，是否是Https,以及证书绑定相关信息。

```java
@Override
public void onCreate() {
    super.onCreate();
    Denglu1Helper.getInstance()
            .setServerHost("www.pingan.com")//域名
            .setServerPort(443)//端口
            .setHttps(true, null);//是否是https,证书绑定参数（如若不设置，填null）

}
```
- 配置证书绑定

如果使用了Https，可以设置证书绑定提高安全性。setHttps第二个参数设置如下，进入SDK界面-->扫码会有网络请求，网络请求会失败，在logcat中输入OkHttp,就可以在log中过滤得到需要的字符串（sha256）数组。

```java
@Override
public void onCreate() {
    super.onCreate();
    Denglu1Helper.getInstance()
            .setServerHost("www.pingan.com")
            .setServerPort(443)
            .setHttps(true, new String[]{});

}
```
- 参考的配置例子如下：

```
Denglu1Helper.getInstance()
            .setServerHost("icore-kbs-stg.pa18.com")
            .setServerPort(10106)
            .setHttps(true, new String[]{
                    "sha256/UC4BKByx58Sph1JCovz7qUZAwupux5uMYm9s0dnVTFI=",
                    "sha256/zUIraRNo+4JoAYA7ROeWjARtIoN4rIEbCpfCRQT6N6A=",
                    "sha256/r/mIkG3eEpVdm+u/ko/cwxzOMo1bk4TyHIlByibiA5E="
            });
```

### 3、设置SDK入口点击事件

这是最后一个步骤。接入App应该预留一个UI入口，比如一个按钮。相应View的点击事件应如下。

```java
//userName、password对应App的账号，密码的hash。密码的hash方式可以自定义，但一开始确定后，就不能再更改。
View sdkEntry = findViewById(R.id.tv_fun_entry);
sdkEntry.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Denglu1Helper.startSdkUI(XXXActivity.this, "username", "password-hash");
    }
});
```
### 4、关于混淆
SDK是部分混淆过的，aar里也包含混淆配置文件，所以，不需要特别处理。如果打包release版出现问题。再联系我们。
