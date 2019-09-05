---
title: (android) MVVM 1. data binding 시작하기
categories:
- 개발
tags:
- android
- 안드로이드
- databinding
- mvvm
---

Android 에서 MVVM(Model-View-ViewModel)을 적용하기 위해 Data binding을 사용하는 방법을 알아보겠습니다.

C#(WPF)과 달리 Android에서 Data binding을 사용하기 위해서는 별도 작업이 필요합니다.

아래 코드들은 [Android Developers: Data binding library 페이지](https://developer.android.com/topic/libraries/data-binding)를 참고하였습니다.

# 1. app/build.gradle 에 dataBinding 옵션 활성화

Data binding 관련 라이브러리를 사용하기 위해서는 app/build.gradle 의 android에 dataBinding 속성을 enable시켜야 합니다.

```gradle
android {
    ...
    dataBinding {
        enabled = true
    }
}
```

# 2. Data binding 을 사용할 Activity resource를 layout tag로 감싸기

프로젝트 생성 시 자동 생성된 Activity는 최상단이 ContraintLayout tag로 생성됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    ...
    tools:context=".MainActivity">
    <TextView
        ...
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

이걸 아래처럼 layout tag로 감싸야 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hello World!"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

# 3. MainActivity class 수정

자동으로 생성된 MainActivity class에는 아래와 같이 onCreate method가 정의 되어있습니다.

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

Data binding을 사용하기 위해 setContentView를 아래처럼 바꿔줍니다.

activity_main.xml 에 의해 ActivityMainBinding은 자동생성된 class 입니다.

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
    }
}
```

# 4. Binding 테스트

3번 까지 진행하여 빌드가 되면 사실 Data Binding을 사용하기 위한 준비는 끝났습니다.

MVVM 적용은 이후 해보는 것으로 하고, 일단 Data binding 이 잘 되는지 확인해보기 위해 기본 생성된 TextView의 text에 string data 를 binding 해보겠습니다.

## 4.1. String member 추가

MainActivity class에 TextView에 Binding시킬 string member를 추가합니다.

```java
public class MainActivity extends AppCompatActivity {
    private String text = "Hello Data Binding"; // <- 추가

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
    }
}
```

## 4.2. layout에 binding할 data 추가

activity_main.xml 의 layout tag에 binding할 data 를 추가합니다.

name은 MainActivity에 선언한 member 이름입니다.

```xml
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="text"
            type="String" />
    </data>
...
</layout>
```

## 4.3. onCreate method에서 data binding 연결

MainActivity class의 onCreate 메소드에서 ActivityMainBinding 객체에 text member를 연결해주어야 합니다.

```java
public class MainActivity extends AppCompatActivity {
    private String text = "Hello Data Binding";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
        binding.setText(text); // <- 추가
    }
}
```

## 4.4. TextView의 text 속성 변경

TextView의 text를 "Hello World!" 에서 "@{text}"로 변경합니다.

그럼 TextView의 text를 표시할 때 data tag의 text 값을 읽어와 표시합니다.

```xml
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="text"
            type="String" />
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{text}"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

## 4.5. 실행 결과

아래처럼 Text가 "Hello Data Binding"으로 잘 표시되면 성공입니다.

![screenshot](https://apt-info.github.io/images/2019-09-03-android-data-binding/1.png)


# Code

전체 코드는 [https://github.com/apt-info/samples/android/DataBinding](https://github.com/apt-info/samples/tree/master/android/DataBinding) 에서 확인하실 수 있습니다.