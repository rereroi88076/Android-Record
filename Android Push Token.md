# Android FCM 通知推播建立


## **Step 1 添加 google-services**：

```
由推播後台生成一個google-services.json檔案,放入<project name>/app目錄下
```

## **Step 2 添加 Google 服務為構建腳本依賴項目**：
[參考](https://firebase.google.com/docs/android/setup?hl=zh-tw)

### Gradle 文件 ( <project>/build.gradle ) 中
###### 若找無dependencies,可直接於plugins上面新增整段
    

```
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'com.google.gms:google-services:4.3.15'
    }
}
```
    
    
## **Step 3 添加 Google 服務插件**

[參考](https://firebase.google.com/docs/android/setup?hl=zh-tw)

### Gradle 文件（通常是<project>/<app-module>/build.gradle ）中
```
plugins {
    id 'com.android.application'

    // Add the Google services Gradle plugin
    id 'com.google.gms.google-services'
    ...
}
```

## **Step 4 添加 Firebase SDK** 
[參考](https://firebase.google.com/docs/android/setup?hl=zh-tw)
```
//FCM
// Import the BoM for the Firebase platform
implementation platform('com.google.firebase:firebase-bom:31.2.0')
implementation 'com.google.firebase:firebase-messaging:23.1.1'
// Declare the dependencies for the Firebase Cloud Messaging and Analytics libraries
// When using the BoM, you don't specify versions in Firebase library dependencies
implementation 'com.google.firebase:firebase-messaging-ktx:23.1.1'
implementation 'com.google.firebase:firebase-analytics-ktx'
```

## **Step 5 添加 MyFirebaseMessagingService.class** 


## **Step 6 添加 MyFirebaseMessagingService 至Manifest**
[參考](https://firebase.google.com/docs/cloud-messaging/android/client?hl=zh-tw)
```
<service
    android:name=".MyFirebaseMessagingService"
    android:directBootAware="true"
    android:exported="false"
    tools:targetApi="n">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>
```

## **Step 7 添加 預設設定至 Manifest**
[參考](https://firebase.google.com/docs/cloud-messaging/android/client?hl=zh-tw)
```
        <!--        傳入通知未設定icon時,會使用此預設icon-->
        <meta-data
            android:name="com.google.firebase.messaging.default_notification_icon"
            android:resource="@drawable/ic_stat_ic_notification" />

        <!--        傳入通知未設定color時,會使用此預設color-->
        <meta-data
            android:name="com.google.firebase.messaging.default_notification_color"
            android:resource="@color/colorAccent" />

        <!--        Android8.0以上,使用通知通道-->
        <meta-data
            android:name="com.google.firebase.messaging.default_notification_channel_id"
            android:value="@string/default_notification_channel_id" />
```
    
## **Step 8 取得push token**
通常用在登入後
[參考](https://firebase.google.com/docs/cloud-messaging/android/client?hl=zh-tw)
    
```
    private fun getPushToken(tokenId: String) {
        FirebaseMessaging.getInstance().token.addOnCompleteListener(OnCompleteListener { task ->
            if (!task.isSuccessful) {
                Log.w("message_push_token_false", "Fetching FCM registration token failed", task.exception)
                return@OnCompleteListener
            }

            // Get new FCM registration token
            task.result?.let { pushToken ->
                RoomDbHelper.getDatabase(context).getUserConfigDao().insertValue(UserConfigEntity(UserConfigKey.PUSH_TOKEN.tag, pushToken))
    //todo 以下是設定push token api
                viewModelScope.launch(Dispatchers.IO) {
                    setPushToken(tokenId, pushToken)
                }
                Log.d("message_push_token", pushToken)
            }
        })
    }
```

