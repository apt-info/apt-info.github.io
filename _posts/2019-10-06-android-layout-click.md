---
title: (android) Layout에 click event 넣기
categories:
- 개발
tags:
- android
- 안드로이드
- Layout
- Click
---

---

지금까지 공부한 것

* [(android) webview 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-webview/)
* [(android) 로딩 화면(splash background) 적용 하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-splash-background/)
* [(android) TabLayout (+ViewPager) 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-tablayout/)
* [(android) TabLayout 화면 꽉 채우도록 설정](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-tablayout-fill/)

---

이번에는 Layout에 Click event를 넣는 방법을 알아보겠습니다.

LinearLayout안에 TextView하나를 넣고, touch(click)했을 때 Toast를 띄우는 예제를 만들어보겠습니다.

# 1. LinearLayout 및 TextView 생성

- activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout>
    <data>
        <variable
            name="viewModel"
            type="com.aptinfo.layoutselect.MainViewModel" />
    </data>
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:layout_margin="8dp"
            android:padding="6dp"
            android:onClick="@{()->viewModel.onClick()}"
            android:background="#EEEEEE">
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="@style/TextAppearance.AppCompat.Large"
                android:text="@{viewModel.text}"/>
        </LinearLayout>
    </LinearLayout>
</layout>

```

`LinearLayout`에 `onClick` 을 추가하여 viewModel의 onClick메서드가 호출되도록 하였습니다.

`LinearLayout`구분을 위해 background 색상을 넣어보았습니다.

- MainViewModel

```java
public class MainViewModel {
    private Context context;
    public final String text = "TextView";

    public MainViewModel(Context context) {
        this.context = context;
    }

    public void onClick() {
        Log.i("MainViewModel", "onClick");
        Toast.makeText(context, "TextView 클릭", Toast.LENGTH_SHORT).show();
    }
}
```

Binding을 사용하고 있는데, 이 주제에서는 Binding에 관련된 내용은 따로 설명하지 않습니다.

Binding과 관련된 내용은 아래를 참고하세요

---

* [(android) MVVM 1. data binding 시작하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding/)
* [(android) MVVM 2. button binding (text/click event)](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-button/)
* [(android) MVVM 3. RecyclerView binding](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-recyclerview/)
* [(android) MVVM 4. TabLayout (+ViewPager) 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-mvvm-tablayout/)
* [(android) MVVM 5. EditText (2-way binding)](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-mvvm-edittext/)

---

# 2. 실행

`LinearLayout`위에 `TextView`가 잘 표시되고, 터치 시 Toast 팝업도 잘 나오는 걸 볼 수 있습니다.

- 실행 화면

![screenshot](https://apt-info.github.io/images/2019-10-06-android-layout-click/1.jpg)

- layout 선택 시

![screenshot](https://apt-info.github.io/images/2019-10-06-android-layout-click/2.jpg)

하지만 선택했을 때 색상의 변화가 없어 이대로는 좀 밋밋합니다.

색상 변화를 주기 위해 selector를 추가합니다.

# selector 추가

- drawable/layout_selector.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="false">
        <color android:color="#EEEEEE" />
    </item>
    <item android:state_pressed="true">
        <color android:color="#3E84FF" />
    </item>
</selector>
```

pressed가 `false`일때는 옅은 회색, pressed가 `true`일때 파란색을 표시하도록 합니다.

그리고 layout에 해당 색상을 적용하기 위해 background 항목을 수정합니다.

- activity_main.xml

```xml

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:layout_margin="8dp"
    android:padding="6dp"
    android:onClick="@{()->viewModel.onClick()}"
    android:background="@drawable/layout_selector">
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textAppearance="@style/TextAppearance.AppCompat.Large"
        android:text="@{viewModel.text}"/>
</LinearLayout>

```

# 4. 최종 실행

이번에는 Layout 선택 시 background가 잘 변경되는걸 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-10-06-android-layout-click/3.jpg)

# 5. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/LayoutSelect) 에서 확인하실 수 있습니다.
