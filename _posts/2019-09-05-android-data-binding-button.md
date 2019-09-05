---
title: (android) MVVM 2. button binding (text/click event)
categories:
- 개발
tags:
- android
- 안드로이드
- databinding
- 바인딩
- mvvm
- button
---

Android MVVM 2번째 시간!

이번엔 Button 을 하나 만들어, button 의 text와 click event를 binding 하는 방법을 알아보겠습니다.

지난번과 마찬가지로 [Android Developers: Data binding library](https://developer.android.com/topic/libraries/data-binding)를 참고하였습니다.

# 1. Binding 기본 준비

[지난번 포스팅](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding)을 참고하여 MainActivity를 Binding 가능한 상태로 변경합니다.

1. app/build.gradle 에 dataBinding 옵션 활성화
2. Data binding 을 사용할 Activity resource를 layout tag로 감싸기
3. MainActivity class 수정

# 2. Button 추가

activity에 button 하나를 추가합니다.

밑에서 text부분을 binding으로 변경하고, onClick event handler를 연결할 겁니다.

```xml
<Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="button"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
```

# 3. Binding 할 Data class 생성

Binding할 class를 생성합니다.

전 ModelView 라고 이름 지었습니다.

```java
package com.aptinfo.databindingbutton;

import androidx.databinding.ObservableField;

public class ViewModel {
    private int count = 0;
    private ObservableField<String> buttonText = new ObservableField<>();

    public ViewModel() {
        updateButtonText();
    }

    public ObservableField<String> getButtonText() {
        return buttonText;
    }

    public void updateButtonText() {
        buttonText.set(count + " clicked");
    }

    public void onButtonClick() {
        ++count;
        updateButtonText();
    }
}
```

## 3.1. ObservableField

buttonText 필드를 ObservableField type으로 선언하였습니다.

Observable field로 binding 해두면, 값이 변경되었을 때 변경 내용이 통지되어 자동으로 UI에 반영됩니다.

## 3.2. getButtonText

buttonText를 private field로 선언하고, getButtonText 라는 getter를 만들었습니다.

buttonText를 public final 로 선언하면, getter 없이 binding 가능합니다.

## 3.3. onButtonClick

button click시 불릴 function입니다.

내부 count를 증가하고, buttonText항목을 업데이트 하도록 했습니다.

# 4. data binding

Button과 ViewModel을 binding 해보겠습니다.

## 4.1. data tag 추가

activity_main.xml 에 data tag를 추가합니다.

```xml
<data>
    <variable
        name="viewModel"
        type="com.aptinfo.databindingbutton.ViewModel" />
</data>
```

## 4.2. MainActivity 에서 viewModel 객체를 생성하여 바인딩 설정을 합니다.

```java
private ViewModel viewModel = new ViewModel();

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
    binding.setViewModel(viewModel);
}
```

## 4.3. button 속성 변경

button의 text를 viewModel의 buttonText로 변경하고

button 이 click되었을 때 viewModel의 onButtonClick이 호출되도록 Button 속성을 수정합니다.

```xml
<Button
    ...
    android:text="@{viewModel.buttonText}"
    android:onClick="@{()->viewModel.onButtonClick()}"
    ... />
```

# 5. 실행 결과

button click 시 text가 변경되는걸 확인할 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-05-android-data-binding-button/1.png)

# 6. code

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/DataBindingButton) 에서 확인하실 수 있습니다.