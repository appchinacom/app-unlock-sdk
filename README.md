# 应用汇付费游戏解锁 SDK

应用汇付费游戏解锁 SDK 为您的游戏提供一次性购买功能，增强游戏变现能力

集成了解锁 SDK 的游戏在运行时会先请求应用汇客户端申请解锁，用户可以在应用汇客户端内完成购买以及解锁操作，解锁成功后方可继续游戏

### 开始使用

#### 1.注册 APP

接入者需要先到[应用汇开发者网站][appchina_dev_site]注册 APP，注册后你会得到 app key、app 公钥 和 app 私钥，后面申请解锁时会用到

#### 2.导入 SDK

#### 3.编码申请解锁

在 APP 的入口 Activity 的 onCreate 方法中申请解锁，如下：

```java
public class MainActivity ... {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    
        /* 先用第一步注册后得到的几个 key 构建 AppInfo */

        String appKey = ...;
        String appRsaPubKey = ...;
        String appRsaPriKey = ...;
        AppInfo appInfo = new AppInfo(appKey, appRsaPubKey, appRsaPriKey);

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

        @Override
        public void onFailed(int failedCause) {
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
