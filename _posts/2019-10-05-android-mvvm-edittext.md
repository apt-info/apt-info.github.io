---
title: (android) MVVM 5. EditText (2-way binding)
categories:
- 개발
tags:
- android
- 안드로이드
- MVVM
- Binding
- Edittext
---

---

지금까지 공부한 것

* [(android) MVVM 1. data binding 시작하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding/)
* [(android) MVVM 2. button binding (text/click event)](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-button/)
* [(android) MVVM 3. RecyclerView binding](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-recyclerview/)
* [(android) MVVM 4. TabLayout (+ViewPager) 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-mvvm-tablayout/)

---

이번에는 Edittext에 viewmodel의 String에 바인딩 하는 예제를 알아보겠습니다.

지금까지는 단방향 바인딩이었지만, 이번에는 EditText내용이 변경되면 변수에 값이 저장되도록 양방향 바인딩을 구현할 것입니다.

# 1. Binding 기본 준비

[지난번 포스팅](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding)을 참고하여 MainActivity를 Binding 가능한 상태로 변경합니다.

1. app/build.gradle 에 dataBinding 옵션 활성화
2. Data binding 을 사용할 Activity resource를 layout tag로 감싸기
3. MainActivity class 수정

# 2. ViewModel 생성

MainViewModel class를 생성하고, binding 시켜줄 text 멤버를 만듭니다.

String은 변경을 자동으로 알리기 위해 ObservableField를 사용합니다.

```java
public class MainViewModel {
    private ObservableField<String> text;

    public MainViewModel() {
        this.text = new ObservableField<>("text binding");
    }

    public ObservableField<String> getText() {
        return text;
    }
}
```

# 3. ActivityMain.xml 에 EditText 생성

MainActivity에 EditText를 만들어주고, MainViewModel의 text와 binding 합니다.

EditText가 변경될 때 MainViewModel의 text가 잘 변경되는지 확인을 위해 TextView를 하나 더 추가하였습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context=".MainActivity">
    <data>
        <variable
            name="viewModel"
            type="com.aptinfo.databindingedittext.MainViewModel" />
    </data>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{viewModel.text}"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{viewModel.text}" />
    </LinearLayout>
</layout>
```

# 4. 실행

이 상태로 실행해보면 EditText와 TextView에 각각 `text binding` 이라고 잘 표시됩니다.

![screenshot](https://apt-info.github.io/images/2019-10-05-android-mvvm-edittext/1.jpg)


하지만 EditText내용을 변경했을 때 TextView의 변경내용이 바뀌지 않습니다.

![screenshot](https://apt-info.github.io/images/2019-10-05-android-mvvm-edittext/2.jpg)

이것은 단방향 바인딩으로, 생각했던 동작과 다릅니다.

# 5. Binding 방법 변경

EditText의 바인딩 기호 `@` 뒤에 `=`를 붙여줍니다.

```xml
<EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="@={viewModel.text}"/>
```

이렇게 변경하면 EditText의 변경사항을 viewModel.text에 반영하겠다는 의미가 됩니다.

# 6. 최종 실행

이번엔 EditText내용을 변경할 때 마다 TextView의 내용도 같이 변경되는걸 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-10-05-android-mvvm-edittext/3.jpg)

# 7. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/DataBindingEditText) 에서 확인하실 수 있습니다.
