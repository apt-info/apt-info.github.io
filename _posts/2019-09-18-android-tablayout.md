---
title: (android) TabLayout (+ViewPager) 사용하기
categories:
- 개발
tags:
- android
- 안드로이드
- TabLayout
- ViewPager
---

---

지금까지 공부한 것

* [(android) webview 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-webview/)
* [(android) 로딩 화면(splash background) 적용 하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-splash-background/)
* [(android) MVVM 1. data binding 시작하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding/)
* [(android) MVVM 2. button binding (text/click event)](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-button/)
* [(android) MVVM 3. RecyclerView binding](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding-recyclerview/)

---

이번에는 안드로이드에서 TabLayout을 사용하는 방법을 알아보겠습니다.

최종적으로 만들 뷰는 아래와 같습니다.

![screenshot](https://apt-info.github.io/images/2019-09-18-android-tablayout/1.png)

# 1. 의존성 추가

TabLayout은 안드로이드 기본 component에 들어있지 않습니다.

TabLayout을 사용하기 위해 build.gradle에 의존성을 추가합니다.

* `app/build.gradle`

```gradle
dependencies {
    ...
    implementation 'com.google.android.material:material:1.1.0-alpha10'
    ...
}
```

# 2. Framgent 생성

TabLayout에서 Tab을 선택하면 ViewPager에 content(fragment)가 보이는 구조입니다.

실제 보여질 content(Fragment)를 구현해야 합니다.

resource (xml)를 만들고, `Fragment` class를 상속받은 class를 구현합니다.

* res/layout/page1.xml

```xml

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent" android:layout_height="match_parent">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Page1" />
</RelativeLayout>

```

* Page1Fragment.java

```java

public class Page1Fragment extends Fragment {
    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.page1, container, false);
    }
}

```

Page2도 위와 마찬가지로 xml과 class를 정의하여 구현합니다.


# 3. ViewPager에 사용할 Adapter 생성

ViewPager에 content를 표시하기 위해 Adapter를 구현해야 합니다.

`FragmentStatePagerAdapter` 를 상속받아 구현해야 하며, 구현이 필요한 메소드는 `getItem`과 `getCount` 입니다.

`getCount`는 ViewPager가 가지는 총 content개수를 의미하고 `getItem`은 특정 index의 content를 리턴하는 메소드 입니다.

여기서는 Page1, Page2를 각각 보이도록 구현하였습니다.

* `ViewPagerAdapter.java`

```java
public class ViewPagerAdapter extends FragmentStatePagerAdapter {
    private int pageCount;

    public ViewPagerAdapter(FragmentManager mgr, int pageCount) {
        super(mgr, BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT);
        this.pageCount = pageCount;
    }

    @NonNull
    @Override
    public Fragment getItem(int position) {
        switch (position) {
            case 0:
                return new Page1Fragment();
            case 1:
                return new Page2Fragment();
            default:
                return null;
        }
    }

    @Override
    public int getCount() {
        return pageCount;
    }
}
```

# 4. MainActivity 구현

activity_main 에 ViewPager와 TabLayout을 각각 그리고, MainActivity.java 에서 TabLayout과 ViewPager를 연동하는 로직을 구현해야 합니다.

* res/layout/activity_main.xml

```xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.viewpager.widget.ViewPager
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/viewPager"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent">
    </androidx.viewpager.widget.ViewPager>
    <com.google.android.material.tabs.TabLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/tabLayout"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintBottom_toBottomOf="parent">
        <com.google.android.material.tabs.TabItem
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Page1" />
        <com.google.android.material.tabs.TabItem
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Page2" />
    </com.google.android.material.tabs.TabLayout>

</androidx.constraintlayout.widget.ConstraintLayout>

```

* MainActivity.java

```java
public class MainActivity extends AppCompatActivity {
    private TabLayout tabLayout;
    private ViewPager viewPager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tabLayout = findViewById(R.id.tabLayout);
        viewPager = findViewById(R.id.viewPager);

        ViewPagerAdapter viewPagerAdapter = new ViewPagerAdapter(getSupportFragmentManager(), tabLayout.getTabCount());
        viewPager.setAdapter(viewPagerAdapter);

        // TabLayout page change listener를 ViewPager의 page change listener에 연결
        viewPager.addOnPageChangeListener(new TabLayout.TabLayoutOnPageChangeListener(tabLayout));

        tabLayout.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                // 선택된 Tab index로 ViewPager 아이템을 선택
                viewPager.setCurrentItem(tab.getPosition());
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {
            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {
            }
        });
    }
}
```

# 5. 실행화면

Tab 선택, 화면 슬라이드 시 Page/Tab이 잘 변경되는 것을 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-18-android-tablayout/1.png)

![screenshot](https://apt-info.github.io/images/2019-09-18-android-tablayout/2.png)

# 6. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/TabLayout) 에서 확인하실 수 있습니다.

