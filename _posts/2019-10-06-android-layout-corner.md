---
title: (android) 둥근 모서리 Layout 만들기
categories:
- 개발
tags:
- android
- 안드로이드
- Layout
- corner
- radius
- 모서리
---

---

지금까지 공부한 것

* [(android) webview 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-webview/)
* [(android) 로딩 화면(splash background) 적용 하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-splash-background/)
* [(android) TabLayout (+ViewPager) 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-tablayout/)
* [(android) TabLayout 화면 꽉 채우도록 설정](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-tablayout-fill/)
* [(android) Layout에 click event 넣기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-layout-click/)
* [(android) Layout에 그림자 효과 넣기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-layout-shadow/)

---

이번에는 모서리가 둥근 layout을 만들어보겠습니다.

LinearLayout에 TextView를 하나 넣고, 그 Layout에 둥근 배경을 적용해보겠습니다.

최종 만들게 될 결과물은 아래와 같습니다.

![screenshot](https://apt-info.github.io/images/2019-10-06-android-layout-corner/1.jpg)


# 1. shape 추가

layout 배경에 사용될 shape를 추가합니다.

사각형을 만들고, corner를 이용해 둥근 모서리를 적용합니다.

- drawable/layout_background.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="#EEEEEE" />
    <stroke android:color="#CCCCCC" android:width="1dp" />
    <corners android:radius="7dp" />
</shape>
```

배경색(solid)와 외곽선(stroke)를 추가하여 둥근모서리가 좀더 잘 보이도록 하였습니다.

# 2. LinearLayout 및 TextView 생성

- activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    android:orientation="vertical">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:layout_margin="6dp"
        android:padding="6dp"
        android:background="@drawable/layout_background" >
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textAppearance="@style/TextAppearance.AppCompat.Large"
            android:text="코너가 둥근 Layout" />
    </LinearLayout>
</LinearLayout>
```

실행해보면 둥근 모서리가 잘 적용되는걸 확인할 수 있습니다.

# 3. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/LayoutCorner) 에서 확인하실 수 있습니다.
