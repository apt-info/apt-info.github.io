---
title: (android) Layout에 그림자 효과 넣기
categories:
- 개발
tags:
- android
- 안드로이드
- Layout
- shadow
- 그림자
---

---

지금까지 공부한 것

* [(android) webview 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-webview/)
* [(android) 로딩 화면(splash background) 적용 하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-splash-background/)
* [(android) TabLayout (+ViewPager) 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-tablayout/)
* [(android) TabLayout 화면 꽉 채우도록 설정](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-tablayout-fill/)
* [(android) Layout에 click event 넣기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-layout-click/)

---

이번에는 Layout에 그림자 효과를 넣는 방법을 알아보겠습니다.

LinearLayout에 TextView를 하나 넣고, 그 Layout에 그림자 효과를 넣어보겠습니다.

최종 만들게 될 결과물은 아래와 같습니다.

![screenshot](https://apt-info.github.io/images/2019-10-06-android-layout-shadow/1.jpg)


# 1. layer list 추가

layout 배경에 사용될 layer list를 추가합니다.

사각형 두개로 그림자 효과를 낼 것입니다.

- drawable/layout_background.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:left="3dp"
        android:top="3dp">
        <shape android:shape="rectangle">
            <solid android:color="#DDDDDD" />
        </shape>
    </item>
    <item
        android:right="3dp"
        android:bottom="3dp">
        <shape android:shape="rectangle">
            <solid android:color="#FFFFFF" />
        </shape>
    </item>
</layer-list>
```

처음에 짙은 회색으로 사각형을 그리고, 흰색으로 사각형을 한번 더 그립니다.

첫 사각형은 left, top조건으로 인해 '오른쪽 하단'에 그려지고, 두번째 사각형은 right, bottom 조건으로 '왼쪽 상단'에 그려집니다.

# 2. LinearLayout 및 TextView 생성

- activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#EEEEEE"
    tools:context=".MainActivity">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:layout_margin="6dp"
        android:background="@drawable/layout_background"
        android:padding="6dp">
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textAppearance="@style/TextAppearance.AppCompat.Large"
            android:text="Sample" />
    </LinearLayout>
</LinearLayout>

```

layout과 배경사이에 떨어진? 느낌을 주기 위해 margin을 추가하였습니다.

뒷 배경색을 흰색보다는 어둡게, 그림자(#DDDDDD)보다는 밝은 값으로 넣었습니다.

# 3. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/LayoutShadow) 에서 확인하실 수 있습니다.
