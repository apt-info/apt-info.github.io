---
title: (android) TabLayout 화면 꽉 채우도록 설정
categories:
- 개발
tags:
- android
- 안드로이드
- TabLayout
---

---

지금까지 공부한 것

* [(android) webview 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-webview/)
* [(android) 로딩 화면(splash background) 적용 하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-splash-background/)
* [(android) TabLayout (+ViewPager) 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-tablayout/)
* [(android) MVVM 1. data binding 시작하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding/)
* [(android) MVVM 2. button binding (text/click event)](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-button/)
* [(android) MVVM 3. RecyclerView binding](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-recyclerview/)

---

# 1. 기본 TabLayout 구현

TabLayout사용 시 별다른 설정이 없으면 Portrait화면에서는 Tab이 화면에 꽉 차지만 Landscape 변환시(가로모드) Tab이 화면에 꽉 차지 않는 모양이 됩니다. (Tablet에서 실행시에는 Portrait에서도 꽉 안참)

간단한 TabLayout을 만들어 확인해봅니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.google.android.material.tabs.TabLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent">
        <com.google.android.material.tabs.TabItem
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Tab1" />
        <com.google.android.material.tabs.TabItem
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Tab2" />
    </com.google.android.material.tabs.TabLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

- portrait 화면

![screenshot](https://apt-info.github.io/images/2019-09-21-android-tablayout-fill/1.png)

- landscape 화면

![screenshot](https://apt-info.github.io/images/2019-09-21-android-tablayout-fill/2.png)


# 2. TabLayout 속성 변경

TabLayout의 tabMode, TabMaxWidth, tabGravity 속성을 수정하여 Landscape이나 Tablet에서도 Tab을 꽉 채우도록 설정할 수 있습니다.

```xml
<com.google.android.material.tabs.TabLayout
    ...
    app:tabMode="fixed"
    app:tabMaxWidth="0dp"
    app:tabGravity="fill"
    ...>
```


# 3. 실행화면

Tab 선택, 화면 슬라이드 시 Page/Tab이 잘 변경되는 것을 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-21-android-tablayout-fill/1.png)

![screenshot](https://apt-info.github.io/images/2019-09-21-android-tablayout-fill/3.png)

# 4. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/TabLayoutFill) 에서 확인하실 수 있습니다.

