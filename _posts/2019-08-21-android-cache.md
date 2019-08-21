---
title: (Android) gradle 빌드 시 항상 최신버전을 다운받도록 하는 방법
categories:
- 개발
tags:
- Android
- 안드로이드
- Cache
- Gradle
---

Android gradle 빌드 시 서버의 패키지를 다운받도록 되어있는 경우, 아래 조건을 만족하면 새로 다운받지 않고 cache에 있는 패키지를 사용하게 됩니다.

- 이전 빌드 시 다운받았던 패키지가 cache에 남아있음
- cache에 있는 패키지 버전과 서버에 있는 패키지 버전이 동일함
- cache에 저장된지 24시간이 지나지 않음

개발 중에는 서버의 서버의 패키지 내용이 계속 변경될 수 있으므로, cache에 저장된 패키지를 사용하지 않도록 조치가 필요합니다.

# cache를 사용하지 않도록 build.gradle 수정

app/build.gradle 파일에 아래 내용을 추가합니다.

cache의 유효 기간을 0초로 설정하여, 빌드때마다 새로 cache를 업데이트 하도록 합니다.

```gradle
configurations.all {
    resolutionStrategy {
        cacheChangingModulesFor 0, 'seconds'
    }
}
```
