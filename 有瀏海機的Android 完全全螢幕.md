# 有瀏海機的Android 完全全螢幕
## 完全全螢幕


### style設定

```
<style name="AppTheme.FullScreen" parent="android:Theme.NoTitleBar.Fullscreen">
//windowLayoutInDisplayCutoutMode 處理有瀏海手機用
    <item name="android:windowLayoutInDisplayCutoutMode" tools:targetApi="o_mr1">
        shortEdges
    </item>
</style>
```
    
### Manifest 設定

        <activity
            android:name="你的頁面"
            android:screenOrientation="portrait"
            android:theme="@style/AppTheme.FullScreen">
        </activity>

### code設定
> **SYSTEM_UI_FLAG_IMMERSIVE_STICKY**
與SYSTEM_UI_FLAG_HIDE_NAVIGATION 共同使用，能使NavigationBar完全消失

```
getWindow().getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION |View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY);