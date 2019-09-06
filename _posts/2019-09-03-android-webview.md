---
title: (android) webview 사용하기
categories:
- 개발
tags:
- android
- 안드로이드
- webview
- browser
---

Android 에서 webview를 이용해 앱 안에서 웹 페이지를 띄우는 예제를 만들어보겠습니다.

# 1. 프로젝트 생성

Android studio를 열고 Empty Activity project를 생성합니다.

# 2. activity_main.xml 에 webview추가

res/layout/activity_main.xml 을 열어 기본 생성된 TextView를 제거하고, WebView를 추가합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

# 3. webview 설정

MainActivity class에서 webview객체를 가져와 웹페이지를 띄우기 위한 설정을 합니다.

```java
public class MainActivity extends AppCompatActivity {
    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // webView 가져오기
        webView = (WebView)findViewById(R.id.webView);

        // java script 활성화
        webView.getSettings().setJavaScriptEnabled(true);

        // WebChromeClient 설정 - 브라우저 이벤트 구현을 위해 필요
        webView.setWebChromeClient(new WebChromeClient());

        // WebViewClient 설정 - 새 페이지를 띄울 때 새 창이 아닌 현재 view에서 띄우기 위해 필요
        webView.setWebViewClient(new WebViewClient() {
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                // 새 페이지를 띄울 때 현재 view에서 띄우도록 설정
                view.loadUrl(url);
                return true;
            }
        });

        // 웹 페이지 띄우기
        webView.loadUrl("https://github.com/apt-info");
    }
}

```

# 4. WebView에서 페이지를 띄우기 위해서 네트워크에 접속해야 하므로 App에 INTERNET permission이 있어야 합니다.

AndroidManifest.xml

```xml
<manifest ..>
    <application>
        ...
    </application>
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>
```

# 5. 실행 결과

Web 페이지가 잘 열리는 것을 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-03-android-webview/1.png)

# 6. Titlebar 제거

웹 페이지 위에 Titlebar 없이 전체 화면으로 웹 페이지를 띄우는게 더 깔끔하게 보일 것 같습니다.

Titlebar 제거를 위해 styles.xml 을 수정합니다.

res/values/styles.xml 을 열고 windowsNoTitle item을 추가합니다.

```xml
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="windowNoTitle">true</item>
    </style>

</resources>
```

# 7. back key 설정

한가지 더 욕심을 부리자면, 폰의 back key가 눌렸을 때 웹 브라우저 '뒤로가기' 효과가 나면 좋을 것 같습니다.

MainActivity에 onKeyDown event를 override하여 해당 기능을 구현합니다.

```java
public class MainActivity extends AppCompatActivity {
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (keyCode != KeyEvent.KEYCODE_BACK)
            return super.onKeyDown(keyCode, event);

        if (webView.canGoBack()) {
            webView.goBack();
            return true;
        }

        return super.onKeyDown(keyCode, event);
    }
    ...
}
```

# 8. 실행 결과

실행해보면 Titlebar없이 전체화면으로 Web 페이지가 출력되고, back key를 눌렀을 때에도 뒤로가기가 잘 되는것을 확인할 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-03-android-webview/2.png)

# Code

전체 코드는 [https://github.com/apt-info/samples](https://github.com/apt-info/samples/tree/master/android/SimpleWebView) 에서 확인하실 수 있습니다.