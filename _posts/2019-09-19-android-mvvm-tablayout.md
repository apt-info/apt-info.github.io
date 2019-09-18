---
title: (android) MVVM 4. TabLayout (+ViewPager) 사용하기
categories:
- 개발
tags:
- android
- 안드로이드
- TabLayout
- ViewPager
- MVVM
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

이번에는 MVVM 으로 TabLayout및 ViewPager를 만드는 방법을 알아보겠습니다.

최종적으로 만들 뷰는 아래와 같습니다.

![screenshot](https://apt-info.github.io/images/2019-09-19-android-mvvm-tablayout/1.png)

앞 내용의 상당수는 [(android) TabLayout (+ViewPager) 사용하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-tablayout/) 페이지와 비슷하거나 동일합니다.

DataBinding을 하는 부분은 3번 부터 보시면 됩니다.

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

생성자에서 Fragment list 를 전달받아 사용하도록 하겠습니다.

* `ViewPagerAdapter.java`

```java
public class ViewPagerAdapter extends FragmentStatePagerAdapter {

    private List<Fragment> fragments;

    public ViewPagerAdapter(FragmentManager fm, List<Fragment> fragments) {
        super(fm, BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT);
        this.fragments = fragments;
    }

    @NonNull
    @Override
    public Fragment getItem(int position) {
        if (position < fragments.size())
            return fragments.get(position);
        return null;
    }

    @Override
    public int getCount() {
        return fragments.size();
    }
}

```

# 4. MainActivity 구현

activity_main 에 ViewPager와 TabLayout을 각각 그리고, MainActivity.java 에서 TabLayout과 ViewPager를 연동하는 로직을 구현해야 합니다.

* res/layout/activity_main.xml

```xml

<?xml version="1.0" encoding="utf-8"?>
<layout>
    <androidx.constraintlayout.widget.ConstraintLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
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
</layout>

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
    }
}
```

# 5. MainViewModel 구현

ActivityMain에 대응하는 MainViewModel을 구현해야 합니다.

ViewPager의 adapter, addOnPageChangeListener와 TabLayout의 addOnTabSelectedListener를 바인딩 해야하기 때문에 MainViewModel에 각각 이것들을 가지고 있어야 합니다.


```java

public class MainViewModel {

    private FragmentStatePagerAdapter viewPagerAdapter;
    private TabLayout.ViewPagerOnTabSelectedListener viewPagerOnTabSelectedListener;
    private TabLayout.TabLayoutOnPageChangeListener tabLayoutOnPageChangeListener;

    public MainViewModel(FragmentManager fm,
                         TabLayout.ViewPagerOnTabSelectedListener viewPagerOnTabSelectedListener,
                         TabLayout.TabLayoutOnPageChangeListener tabLayoutOnPageChangeListener) {
        this.viewPagerOnTabSelectedListener = viewPagerOnTabSelectedListener;
        this.tabLayoutOnPageChangeListener = tabLayoutOnPageChangeListener;

        // ViewPagerAdapter 생성
        ArrayList<Fragment> fragments = new ArrayList<>();
        fragments.add(new Page1Fragment());
        fragments.add(new Page2Fragment());
        viewPagerAdapter = new ViewPagerAdapter(fm, fragments);
    }

    public FragmentStatePagerAdapter getViewPagerAdapter() {
        return viewPagerAdapter;
    }

    public TabLayout.ViewPagerOnTabSelectedListener getViewPagerOnTabSelectedListener() {
        return viewPagerOnTabSelectedListener;
    }

    public TabLayout.TabLayoutOnPageChangeListener getTabLayoutOnPageChangeListener() {
        return tabLayoutOnPageChangeListener;
    }
}

```

# 6. TabLayout 및 ViewPager binding 작업

[(android) MVVM 1. data binding 시작하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding/) 페이지를 참고하여 activity_main을 binding 가능한 형태로 변경합니다.

그리고 ViewPager의 adapter, addOnPageChangeListener 와 TabLayout의 addOnTabSelectedListener를 ViewModel 속성과 바인딩 합니다.


* res/layout/activity_main.xml

```xml

<?xml version="1.0" encoding="utf-8"?>
<layout>
    <data>
        <variable
            name="viewModel"
            type="com.aptinfo.databindingtablayout.MainViewModel" />
    </data>
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
            app:setAdapter="@{viewModel.viewPagerAdapter}"
            app:addOnPageChangeListener="@{viewModel.tabLayoutOnPageChangeListener}"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toBottomOf="parent">
        </androidx.viewpager.widget.ViewPager>
        <com.google.android.material.tabs.TabLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/tabLayout"
            app:addOnTabSelectedListener="@{viewModel.viewPagerOnTabSelectedListener}"
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
</layout>

```

* MainActivity.java

```java

public class MainActivity extends AppCompatActivity {

    private MainViewModel mainViewModel;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);

        mainViewModel = new MainViewModel(
                getSupportFragmentManager(),
                new TabLayout.ViewPagerOnTabSelectedListener((ViewPager)findViewById(R.id.viewPager)),
                new TabLayout.TabLayoutOnPageChangeListener((TabLayout)findViewById(R.id.tabLayout)));
        binding.setViewModel(mainViewModel);
    }
}


```

# 7. 실행 화면

Tab 선택, 화면 슬라이드 시 Page/Tab이 잘 변경되는 것을 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-19-android-mvvm-tablayout/1.png)

![screenshot](https://apt-info.github.io/images/2019-09-19-android-mvvm-tablayout/2.png)

# 8. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/DataBindingTabLayout) 에서 확인하실 수 있습니다.

