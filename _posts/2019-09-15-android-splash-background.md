---
title: (android) 로딩 화면(splash background) 적용 하기
categories:
- 개발
tags:
- android
- 안드로이드
- 로딩
- 인트로
- splash
- background
---

Android app에 로딩 화면을 넣는 방법을 알아보겠습니다.

이미지 파일을 하나 추가하고, 앱 실행 시 이미지를 몇초간 띄운 뒤 앱으로 진입하는 예제를 만들어 보겠습니다.

# 1. res/drawable에 image 추가

res/drawable 폴더에 로딩화면에서 사용할 이미지 파일을 추가합니다.

![screenshot](https://apt-info.github.io/images/2019-09-15-android-splash-background/1.png)

# 2. res/drawable에 xml 추가

image파일을 보여줄 resource file을 아래처럼 만들어줍니다.

res/drawable/splash_background.xml

* item/color: 배경색상
* item/bitmap: image 파일

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list
    xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <color android:color="#FFFFFF" />
    </item>
    <item>
        <bitmap
            android:src="@drawable/ic_splash"
            android:gravity="center"/>
    </item>
</layer-list>
```

# 3. Splash Theme item 추가

res/values/style.xml 을 열어 style item을 추가합니다.

item항목은 **@drawable/{위에서만든 res이름}** 입니다.

```xml
<style name="SplashTheme" parent="Theme.AppCompat.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_background</item>
</style>
```

# 4. activity 추가

splash 화면을 보여줄 activity class를 생성합니다.

AppCompatActivity 를 상속받아 구현합니다.

SplashActivity.java

```java
package com.aptinfo.splash;

import android.content.Intent;
import android.os.Bundle;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class SplashActivity extends AppCompatActivity {
    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        try {
            Thread.sleep(2000); // splash 화면 대기 시간
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        startActivity(new Intent(this, MainActivity.class)); // splash 화면이 끝난 뒤 띄울 Activity

        finish();
    }
}
```

# 5. manifest 수정

AndroidManifest.xml 을 열어 앱 실행시 사용할 activity를 MainActivity에서 SplashActivity로 변경합니다.

## 5.1. 변경 전

```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

## 5.2. 변경 후

```xml
<activity android:name=".SplashActivity" android:theme="@style/SplashTheme">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
<activity android:name=".MainActivity" />
```

# 6. 실행

실행해보면 지정한 이미지가 2초간 띄워진 뒤 MainActiviy에 진입하는걸 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-15-android-splash-background/2.png)

# 7. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/Splash) 에서 확인하실 수 있습니다.
