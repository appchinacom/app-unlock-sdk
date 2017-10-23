# 应用汇付费游戏解锁 SDK

应用汇付费游戏解锁 SDK 为您的游戏提供一次性购买功能，增强游戏变现能力

集成了解锁 SDK 的游戏在运行时会先请求应用汇客户端申请解锁，用户可以在应用汇客户端内完成购买以及解锁操作，解锁成功后方可继续游戏

### 开始使用

#### 1.注册 APP

接入者需要先到[应用汇开发者网站][appchina_dev_site]注册 APP，注册后你会得到应用汇公钥和开发者私钥，后面申请解锁时会用到

#### 2.导入 SDK

##### Android Studio
只需在你的 app module 的 build.gradle 文件中加入编译依赖即可，如下：

```groovy
compile 'com.appchina:app-unlock:$last_version'
```

请自行替换 `$last_version` 为最新的版本号（蓝色框框内的就是版本号）： [ ![Download][download_badge_icon]][download_page]

注意：
* 如果你使用的是 3.0 以上版本 Android Gradle 插件，请将 `compile` 替换为 `implementation`

##### Eclipse

如果你使用的是 Eclipse，且现阶段无法替换为 Android Studio，请参考文章 [【Android】1分钟不用改任何代码在Eclipse中使用AAR][aar_to_library_url] 将 AAR 转成 Eclipse Library 使用，[点我去下载 AAR 文件][download_page]

#### 3.编码申请解锁

在 APP 的入口 Activity 的 onCreate 方法中申请解锁，如下：

```java
public class MainActivity ... {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    
        /* 先用第一步注册后得到的 key 构建 AppInfo */
        String acPubKey = ...;// 应用汇公钥
        String devPriKey = ...;// 开发者私钥
        AppInfo appInfo = new AppInfo(acPubKey, devPriKey);

        // 然后申请解锁
        AppUnlocker.unlock(this, appInfo, new UnlockCallbackImpl(this));
    }
    
    private static class UnlockCallbackImpl implements UnlockCallback {
        WeakReference<MainActivity> activityWeakReference;

        UnlockCallbackImpl(MainActivity activity) {
            this.activityWeakReference = new WeakReference<>(activity);
        }

        @Override
        public void onSucceed() {
            MainActivity activity = activityWeakReference.get();
            if (activity != null) {
                // 到这里就解锁成功了，可以在这里加载游戏资源
            }
        }

        @Override
        public void onCanceled() {
            // 退出游戏
            MainActivity activity = activityWeakReference.get();
            if (activity != null) {
                activity.finish();
            }
        }
    }
}
```

[appchina_dev_site]: http://dev.appchina.com/dev/index
[download_badge_icon]: https://api.bintray.com/packages/ac-android/maven/app-unlock/images/download.svg
[download_page]: https://bintray.com/ac-android/maven/app-unlock/_latestVersion
[aar_to_library_url]: http://www.jianshu.com/p/ccf306e08d5b
