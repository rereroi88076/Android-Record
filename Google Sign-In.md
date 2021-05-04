# Google Sign-In


## Gradle
```
1.allprojects -> repositories -> 加入 google()
2.dependencies -> implementation 'com.google.android.gms:play-services-auth:19.0.0'
```



## 於google登錄App
[點此跳轉](https://developers.google.com/identity/sign-in/android/start-integrating)

![](https://i.imgur.com/HO7fuzG.jpg)
```
1.點選"配置項目"來登錄App
2.SHA-1(產生方式)->
Android專案->右邊Gradle->
<App Name>/App/Tasks/android/signingReport
```

## 憑證
[點此跳轉](https://console.cloud.google.com/apis/credentials)

![](https://i.imgur.com/KeCQ4K2.jpg)

```
1.OAuth client點選編輯查看package name & SHA-1
2.Web client (Auto-created for Google Sign-in) ※右邊複製
3.專案string Table(新增)->
<string name="server_client_id">Web client</string>
```

## Sign-In Button
```
    <androidx.appcompat.widget.AppCompatButton
        android:id="@+id/btn_google_sign_in"
        android:layout_width="@dimen/dp_263"
        android:layout_height="wrap_content"
        android:layout_marginTop="@dimen/dp_30"
        android:background="@drawable/main_social_login_button_background"
        android:stateListAnimator="@null"
        android:text="@string/main_google_sign_in"
        android:textAllCaps="false"
        android:textSize="@dimen/dp_14"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/btn_sign_up"
        tools:ignore="SpUsage" />
```

## Activity & onActivityResult
```
private val GOOGLE_SIGN_IN: Int = 2
       ⋮
       ⋮
binding.btnGoogleSignIn.setOnClickListener {
  val signInIntent = viewModel.didGoogleLogin(this)
  startActivityForResult(signInIntent, GOOGLE_SIGN_IN)
}
       ⋮
       ⋮
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
   super.onActivityResult(requestCode, resultCode, data)

   if (requestCode == GOOGLE_SIGN_IN) {
      val task: Task<GoogleSignInAccount> =GoogleSignIn.getSignedInAccountFromIntent(data)
            viewModel.handleSignInResult(task)
        }
    }

```

## ViewModel
```
fun didGoogleLogin(activity: Activity): Intent? {
    val gso = GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
            .requestIdToken(activity.getString(R.string.server_client_id))
            .requestEmail()
            .build()

    val googleSignInClient = GoogleSignIn.getClient(activity, gso)
    return googleSignInClient.signInIntent
    }
       ⋮
       ⋮
fun handleSignInResult(completedTask: Task<GoogleSignInAccount>) {
    try {
        val account: GoogleSignInAccount = completedTask.getResult(ApiException::class.java)!!
        Log.d("message", "${account.idToken}")
        val acct = GoogleSignIn.getLastSignedInAccount(context)
        if (acct != null) {
            Log.d("message", acct.displayName.toString())
            Log.d("message", acct.email.toString())
            Log.d("message", acct.id.toString())
            Log.d("message", acct.photoUrl.toString())
        }
            updateUI(account)
        } catch (e: ApiException) {
            e.printStackTrace()
            Log.w("message","signInResult:failed code=" + e.statusCode)
            updateUI(null)
        }
}
       ⋮
       ⋮
private fun updateUI(account: GoogleSignInAccount?) {
    if (account != null) {
        toastMsgLiveData.value = "登入成功"
    } else {
        toastMsgLiveData.value = "登入失敗"
    }
}
```

