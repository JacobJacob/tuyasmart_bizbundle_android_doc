# 设备控制业务包

 ## 功能介绍

设备控制业务包是涂鸦智能设备控制面板的核心容器，在涂鸦智能 Android Home SDK 的基础上，提供了设备控制面板的加载和控制的接口封装，加速应用开发过程。主要包括以下功能：
* 面板加载（加载多种设备类型，支持：WIFI、Zigbee、Mesh、BLE）
* 设备控制（支持单设备和群组的控制，不支持群组管理）
* 设备定时

## 业务包集成
### 创建工程

   在 Android Studio 中建立你的工程,接入公版 SDK 并配置完成

### 根目录的 build.gradle 配置

  ``` groovy
  allprojects {
      repositories {
        maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
        maven { url 'https://jitpack.io' }
      }
  }
  ```

### module 的 build.gradle 配置

  ``` groovy
  android {
        defaultConfig {
          ndk {
              abiFilters "armeabi-v7a", "arm64-v8a"
          }
      }

      repositories {
          flatDir {
              dirs 'libs'
          }
      }
      compileOptions {
          sourceCompatibility 1.8
          targetCompatibility 1.8
      }

      packagingOptions {
          pickFirst 'lib/*/libc++_shared.so'
         pickFirst 'lib/*/libgnustl_shared.so'
      }
  
    lintOptions {
          abortOnError false
      }
  }
	dependencies {
      implementation fileTree(dir: 'libs', include: ['*.jar'])
      //panel
      implementation 'com.tuya.smart:panel-sdk:0.5.3'
      //homesdk
      implementation 'com.alibaba:fastjson:1.1.67.android'
      implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
      implementation 'com.tuya.smart:tuyasmart:3.17.0'
      //third
      implementation 'com.weigan:loopView:0.1.1'
      implementation 'com.facebook.infer.annotation:infer-annotation:0.11.2'
      implementation 'javax.inject:javax.inject:1'
      implementation 'com.facebook.soloader:soloader:0.8.2'
      implementation 'com.facebook.fresco:fresco:1.3.0'
      implementation "com.facebook.fresco:imagepipeline-okhttp3:1.3.0"
      implementation 'com.alibaba:fastjson:1.1.67.android'
      implementation 'com.facebook.react:react-native:0.51.1.11'
      implementation 'org.apache.commons:commons-compress:1.9'
      implementation 'com.github.PhilJay:MPAndroidChart:v3.0.3'
      implementation 'io.reactivex.rxjava2:rxandroid:2.0.1'
      implementation 'io.reactivex.rxjava2:rxjava:2.1.7'
      implementation 'com.readystatesoftware.systembartint:systembartint:1.0.3'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
  	}
  ```

  ####  可选配置 根据面板功能依赖

  ##### 高德地图依赖项 发布GooglePlay时请务必去除此依赖

  ``` groovy
  implementation 'com.amap.api:search:6.9.2'
  implementation 'com.amap.api:map2d:5.2.0'
  implementation 'com.tuya.smart:tuyasmart-react-native-amap:1.0.0'
  ```

  ##### GoogleMap 依赖项 发布国内应用市场时请务必去除此依赖

  ``` groovy
  implementation 'com.tuya.smart:tuyasmart-react-native-googlemap:1.0.0'
  implementation('com.google.android.gms:play-services-maps:16.1.0') {
      exclude group: 'com.android.support'
  }
  ```

  ##### QQ 音乐登录模块依赖项

  ``` groovy
  implementation(name: 'qqmusic-innovation-aidl-api-sdk-1.0.0-SNAPSHOT-release', ext: 'aar')
  implementation 'com.tuya.smart.rnplugin:tyrctspeakermanager:1.0.15-open'
  def tvsVer = '2.3.0'
  implementation "com.tencent.yunxiaowei.dmsdk:core:$tvsVer"
  implementation "com.tencent.yunxiaowei.webviewsdk:webviewsdk:$tvsVer"
  ```

  ##### 扫地机依赖项

  ``` groovy
  implementation 'com.tuya.smart:tuyasmart-TuyaRNApi-sweeper:5.26.13-open'
  ```
### Theme 配置
  ``` xml
      <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- default configuration start -->
        <item name="app_bg_color">@color/app_bg_color</item>
        <item name="list_line_color">@color/list_line_color</item>
        <!-- default configuration end -->
    </style>
  ```
### 混淆配置

  ``` 
  # react-native
  -keep,allowobfuscation @interface com.facebook.common.internal.DoNotStrip
  -keep,allowobfuscation @interface com.facebook.proguard.annotations.DoNotStrip
  -keep,allowobfuscation @interface com.facebook.proguard.annotations.KeepGettersAndSetters
  # Do not strip any method/class that is annotated with @DoNotStrip
  -keep @com.facebook.proguard.annotations.DoNotStrip class *
  -keep @com.facebook.common.internal.DoNotStrip class *
  -keepclassmembers class * {
      @com.facebook.proguard.annotations.DoNotStrip *;
      @com.facebook.common.internal.DoNotStrip *;
  }
  -keepclassmembers @com.facebook.proguard.annotations.KeepGettersAndSetters class * {
    void set*(***);
    *** get*();
  }
  -keep class * extends com.facebook.react.bridge.JavaScriptModule { *; }
  -keep class * extends com.facebook.react.bridge.NativeModule { *; }
  -keepclassmembers,includedescriptorclasses class * { native <methods>; }
  -keepclassmembers class *  { @com.facebook.react.uimanager.UIProp <fields>; }
  -keepclassmembers class *  { @com.facebook.react.uimanager.annotations.ReactProp <methods>; }
  -keepclassmembers class *  { @com.facebook.react.uimanager.annotations.ReactPropGroup <methods>; }
  -dontwarn com.facebook.react.**
  -keep,includedescriptorclasses class com.facebook.react.bridge.** { *; }

  #高德地图
  -dontwarn com.amap.**
  -keep class com.amap.api.maps.** { *; }
  -keep class com.autonavi.** { *; }
  -keep class com.amap.api.trace.** { *; }
  -keep class com.amap.api.navi.** { *; }
  -keep class com.autonavi.** { *; }
  -keep class com.amap.api.location.** { *; }
  -keep class com.amap.api.fence.** { *; }
  -keep class com.autonavi.aps.amapapi.model.** { *; }
  -keep class com.amap.api.maps.model.** { *; }
  -keep class com.amap.api.services.** { *; }

  #Google Play Services
  -keep class com.google.android.gms.common.** {*;}
  -keep class com.google.android.gms.ads.identifier.** {*;}
  -keepattributes Signature,*Annotation*,EnclosingMethod
  -dontwarn com.google.android.gms.**

  #MPAndroidChart
  -keep class com.github.mikephil.charting.** { *; }
  -dontwarn com.github.mikephil.charting.**

  -keep class com.tuya.**.**{*;}
  -dontwarn com.tuya.**.**
  ```

### Application 中初始化涂鸦智能 设备控制业务包

  ``` java
  public class TuyaSmartApp extends Application {
      @Override
      public void onCreate() {
          super.onCreate();
          // 必须移除 TuyaHomeSdk.init() 方法，再使用 TuyaPanelSDK.init()
          TuyaHomeSdk.setDebugMode(BuildConfig.DEBUG);
          TuyaPanelSDK.init(this," TUYA_SMART_APPKEY","TUYA_SMART_SECRET");
          //未实现的路由回调
          TuyaWrapper.init(this, new RouteEventListener() {
            @Override
            public void onFaild(int errorCode, UrlBuilder urlBuilder) {
                ToastUtil.shortToast(TuyaPanelSDK.getCurrentActivity(), urlBuilder.originUrl);
            }
          });
          //注册家庭服务，实现设置当前家庭 homeId，BizBundleFamilyServiceImpl为示例代码，用户可以自行实现
          TuyaWrapper.registerService(AbsBizBundleFamilyService.class, new BizBundleFamilyServiceImpl());
      }
  }
  ```
## 功能调用

### 实现家庭服务

通过继承 AbsBizBundleFamilyService 抽象类，实现设置当前家庭 homeId

**示例代码**
``` java
public class BizBundleFamilyServiceImpl extends AbsBizBundleFamilyService {

    private long mHomeId;

    @Override
    public long getCurrentHomeId() {
        return mHomeId;
    }

    @Override
    public void setCurrentHomeId(long homeId) {
        mHomeId = homeId;
    }
}
```

### 设置家庭 HomeId

当获取家庭列表后，通过服务化调用设置家庭 homeId

**示例代码**
``` java
    AbsBizBundleFamilyService service = MicroServiceManager.getInstance().findServiceByInterface(AbsBizBundleFamilyService.class.getName());
    //设置为当前家庭的homeId
    service.setCurrentHomeId(homeBean.getHomeId());
```

### 打开面板

通过家庭 Id 和设备 Id 进入对应设备面板页面，家庭和设备的 Id 需要通过公版 SDK 接口获取

**接口说明**

打开设备面板

``` java
gotoPanelViewControllerWithDevice(Activity activity, long homeId, String devId, ITuyaPanelLoadCallback loadCallback);
```
**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| activity     | 在面板内打开新面板时应使用 TuyaPanelSDK.getCurrentActivity() |
| homeId       | 家庭 Id 通过公版 SDK 接口获取                                |
| devId        | 设备Id 通过公版 SDK 接口获取                                |
| loadCallback | 面板加载回调                                                |

**示例代码**
``` java
// 加载状态回调里界面操作的 Context 应使用 TuyaPanelSDK.getCurrentActivity()
final ITuyaPanelLoadCallback mLoadCallback = new ITuyaPanelLoadCallback() {
    @Override
    public void onStart(String deviceId) {
        ProgressUtil.showLoading(TuyaPanelSDK.getCurrentActivity(), "Loading...");
    }
    
    @Override
    public void onError(String deviceId, int code, String error) {
        ProgressUtil.hideLoading();
        Toast.makeText(getApplicationContext(), "errorCode:" + code + ",errorString:" + error, Toast.LENGTH_LONG).show();
    }
    
    @Override
    public void onSuccess(String deviceId) {
        ProgressUtil.hideLoading();
    }
    
    @Override
    public void onProgress(String deviceId, int progress) {
    }
};
    
//打开面板
TuyaPanelSDK.getPanelInstance().gotoPanelViewControllerWithDevice(TuyaPanelSDK.getCurrentActivity(), mCurrentHomeId, bean.getDevId(), mLoadCallback);
```

### 面板事件回调

#### 点击面板右上角按钮

通过面板右上角按钮回调打开其他页面

**接口说明**

进入面板页面后点击右上角按钮可以获取当前面板设备 Id

``` java
void setPressedRightMenuListener(ITuyaPressedRightMenuListener listener);
```
**参数说明**

| 参数                          | 说明                            |
| ----------------------------- | ------------------------------- |
| ITuyaPressedRightMenuListener | 点击按钮回调获取当前面板设备 Id |
**示例代码**
``` java
TuyaPanelSDK.getPanelInstance().setPressedRightMenuListener(new ITuyaPressedRightMenuListener() {
    @Override
    public void onPressedRightMenu(String deviceId) {
        Toast.makeText(TuyaPanelSDK.getCurrentActivity(), "PanelMore", Toast.LENGTH_SHORT).show();
    }
});
```

#### 获取未实现路由地址
获取未实现路由地址跳转对应页面

**接口说明**

进入面板页面后点击按钮跳转路由，如果有未实现的路由则会调用

``` java
void init(Application app,ITuyaOpenUrlListener listener);
```
**参数说明**

| 参数                 | 说明                     |
| -------------------- | ------------------------ |
| RouteEventListener | 未实现的路由回调 |

**示例代码**
``` java
    TuyaWrapper.init(this, new RouteEventListener() {
        @Override
        public void onFaild(int errorCode, UrlBuilder urlBuilder) {
            // 路由原始地址 urlBuilder.originUrl
            ToastUtil.shortToast(TuyaPanelSDK.getCurrentActivity(), urlBuilder.originUrl);
        }
    });
```
### 释放面板资源

在退出应用的时候调用释放资源

**示例代码**

``` java
TuyaPanel.getInstance().onDestroy();
```

### 清除所有面板缓存

面板文件会存放在当前 app 存储目录下，若需要清理可调用此方法

**示例代码**

``` java
TuyaPanelSDK.getPanelInstance().clearPanelCache();
```

## 错误码

| 错误码 | 描述                  |
| ------ | ---------------------- |
| 1901   | 面板下载失败           |
| 1902   | 多语言包下载失败       |
| 1903   | 设备类型不支持         |
| 1904   | 群组内无设备           |
| 1905   | home id有误            |
| 1906   | 设备DeviceBean为null   |
| 1907   | 找不到可下载的面板资源 |
| 1908   | 固件版本不正确         |
| 1909   | 未知错误               |