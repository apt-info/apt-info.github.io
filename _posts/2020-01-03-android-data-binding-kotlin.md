---
title: (Android:Kotlin) MVVM 1. data binding 시작하기
categories:
- 개발
tags:
- android
- 안드로이드
- kotlin
- databinding
- mvvm
---

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- 기본광고 -->
<ins class="adsbygoogle"
    style="display:block"
    data-ad-client="ca-pub-1142216861245946"
    data-ad-slot="4805727019"
    data-ad-format="auto"
    data-full-width-responsive="true"></ins>
<script>
    (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Android(Kotlin) 에서 MVVM(Model-View-ViewModel)을 적용하기 위해 Data binding을 사용하는 방법을 알아보겠습니다.

java로 MVVM을 사용하는 방법은 아래 페이지를 참고해주세요.

---

* [(android) MVVM 1. data binding 시작하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding/)
* [(android) MVVM 2. button binding (text/click event)](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-button/)
* [(android) MVVM 3. RecyclerView binding](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-recyclerview/)
* [(android) MVVM 4. TabLayout (+ViewPager) 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-mvvm-tablayout/)
* [(android) MVVM 5. EditText (2-way binding)](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-mvvm-edittext/)

---

시작하기 위해 언어를 kotlin으로 지정하고 android project를 생성합니다.

![screenshot](https://apt-info.github.io/images/2020-01-03-android-data-binding-kt/1.png)

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

# 2. kotlin-apt plugin 적용

java와 달리 kotlin에서 databinding을 사용하기 위해서는 `app/build.gradle`에 `kotlin-apt` plugin을 적용해야 합니다.

app/build.gradle 상단에 plugin 적용을 위한 코드를 추가합니다.

```gradle

apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
apply plugin: 'kotlin-kapt' // 추가

```

# 3. Data binding 을 사용할 Activity resource를 layout tag로 감싸기

프로젝트 생성 시 자동 생성된 Activity는 최상단이 ContraintLayout tag로 생성됩니다.

ContraintLayout을 layout tag로 한번 더 감싸도록 합니다.

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

그리고 binding할 data 를 layout 태그 밑에 정의합니다.

```xml
<layout>
    <data>
        <variable
            name="text"
            type="String" />
    </data>
    <androidx.constraintlayout.widget.ConstraintLayout>
        <TextView />
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

그리고 TextView의 text 속성을 새로 추가한 data로 표시하도록 설정합니다.

android:text="@{text}"

```xml
<layout>
    <data>
        <variable
            name="text"
            type="String" />
    </data>
    <androidx.constraintlayout.widget.ConstraintLayout>
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

# 4. MainActivity class 수정

최초 자동 생성된 코드는 아래와 같습니다.

```java
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Data binding을 사용하기 위해 setContentView를 아래처럼 바꿔줍니다.

activity_main.xml 에 의해 ActivityMainBinding은 자동생성된 class 입니다.

그리고 layout xml에 정의한 data(text: String)에 값을 채웁니다.

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //setContentView(R.layout.activity_main)
        val binding: ActivityMainBinding = DataBindingUtil.setContentView(this, R.layout.activity_main)

        binding.text = "Hello Data Binding"
    }
}
```

## 5. 실행 결과

아래처럼 Text가 "Hello Data Binding"으로 잘 표시되면 성공입니다.

![screenshot](https://apt-info.github.io/images/2020-01-03-android-data-binding-kt/1.jpg)


# Code

전체 코드는 [https://github.com/apt-info/samples](https://github.com/apt-info/samples/tree/master/android/DataBindingKt) 에서 확인하실 수 있습니다.