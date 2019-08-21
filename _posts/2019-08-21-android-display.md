---
title: (Android) 화면 회전 시 onDestroy/onCreate 불리지 않도록 하는 방법
categories:
- 개발
tags:
- Android
- 안드로이드
- 화면
- 회전
- rotate
- onCreate
- onDestroy
---

Android에서 화면 회전 시 Activity의 onCreate / onDestroy 가 불리면서 화면을 새로 그려야 합니다.

화면 변경 시 UI 내용이 변경되거나 작업을 해줘야 하는 경우 의미가 있겠지만, 간단한 앱인 경우 onCreate / onDestroy를 처리하는 것이 더 불리한 경우가 있습니다.

별도의 처리를 하지 않기 위해서 작업이 필요합니다.

# 방법 1. Activity 화면 고정

화면 회전을 하지 못하도록 Activity orientation을 고정하는 방법이 있습니다.

application manifest의 activity항목에 screenOrintation항목을 설정합니다.

```xml
<activity
    ...
    android:screenOrientation="portrait" />
```

혹은

```xml
<activity
    ...
    android:screenOrientation="landscape" />
```

# 방법 2. 화면 회전 / 크기 변경을 직접 핸들링 하도록 설정

manifest의 confiChanges 항목을 설정하여 화면 변경을 직접 핸들링 하도록 설정할 수도 있습니다.

```xml
<activity
    ...
    android:configChanges="keyboardHidden|orientation|screenSize" />
```