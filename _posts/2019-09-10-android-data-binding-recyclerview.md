---
title: (android) MVVM 3. RecyclerView binding
categories:
- 개발
tags:
- android
- 안드로이드
- databinding
- 바인딩
- mvvm
- recyclerview
---

Android MVVM 3번째 시간!

이번엔 RecyclerView를 만들고, 리스트 아이템을 바인딩하는 방법을 알아보겠습니다.

RecyclerView 의 Adapter및 ViewHolder는 [쎄미님 블로그](http://susemi99.kr/4674/)를 참고하였습니다.

# 1. Binding 기본 준비

[지난번 포스팅](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/android-data-binding)을 참고하여 MainActivity를 Binding 가능한 상태로 변경합니다.

1. app/build.gradle 에 dataBinding 옵션 활성화
2. Data binding 을 사용할 Activity resource를 layout tag로 감싸기
3. MainActivity class 수정

# 2. ViewModel 추가

RecyclerView에 binding할 ViewModel과  각 Item들을 표현할 ViewModel을 각각 만들어야 합니다.

Item은 ListItemViewModel 로 만들었습니다.

```java
public class ListItemViewModel {
    private String title;
    private String content;

    public ListItemViewModel(String title, String content) {
        this.title = title;
        this.content = content;
    }

    public String getTitle() {
        return title;
    }

    public String getContent() {
        return content;
    }
}
```

RecyclerView에 binding할 ModelView는 ListViewModel로 만들었습니다.

addListItem을 button callback에 달아서 button 클릭 시 ListItemViewModel을 추가할 예정입니다.

```java
public class ListViewModel {
    private int count = 0;
    private ObservableArrayList<ListItemViewModel> listItemViewModels;

    public ListViewModel() {
        listItemViewModels = new ObservableArrayList<>();
    }

    public ObservableArrayList<ListItemViewModel> getListItemViewModels() {
        return this.listItemViewModels;
    }

    public void addListItem() {
        ++count;
        listItemViewModels.add(new ListItemViewModel("title " + count, "content " + count));
    }
}
```

# 3. item에 대한 resource layout 추가

item을 표현할 resource layout을 추가해야합니다.

마찬가지로 layout 태그로 묶어야 binding을 사용할 수 있습니다.

저는 list_item.xml로 만들었습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout>
    <data>
        <variable
            name="viewModel"
            type="com.aptinfo.databindingrecylcerview.viewModel.ListItemViewModel" />
    </data>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{viewModel.title}"/>
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{viewModel.content}"/>
    </LinearLayout>
</layout>
```

# 4. Adapter 및 ViewHolder class 추가

ListView를 binding하기 위해서는 Adapter와 ViewHolder class가 필요합니다.

우선 ViewHodler를 만들어봅니다.

```java
public class BindingViewHolder<T extends ViewDataBinding> extends RecyclerView.ViewHolder {
    private final T binding;

    public BindingViewHolder(View view) {
        super(view);
        this.binding = (T)DataBindingUtil.bind(view);
    }

    public T binding() {
        return binding;
    }
}
```

Adapter를 구현합니다.

```java
public class ListViewAdapter extends RecyclerView.Adapter<BindingViewHolder<ListItemBinding>> {
    private ArrayList<ListItemViewModel> listItemViewModels = new ArrayList<>();

    public void updateItems(ArrayList<ListItemViewModel> listItemViewModels) {
        ListItemDiffCallback callback = new ListItemDiffCallback(this.listItemViewModels, listItemViewModels);
        DiffUtil.DiffResult result = DiffUtil.calculateDiff(callback);

        this.listItemViewModels.clear();
        this.listItemViewModels.addAll(listItemViewModels);
        result.dispatchUpdatesTo(this);
    }

    @Override
    public BindingViewHolder<ListItemBinding> onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        LayoutInflater inflater = LayoutInflater.from(parent.getContext());
        return new BindingViewHolder<>(inflater.inflate(R.layout.list_item, parent, false));
    }

    @Override
    public void onBindViewHolder(@NonNull BindingViewHolder<ListItemBinding> holder, int position) {
        holder.binding().setViewModel(listItemViewModels.get(position));
    }

    @Override
    public int getItemCount() {
        return listItemViewModels.size();
    }
}
```

Adapter코드를 보면 Item업데이트를 위한 ListItemDiffCallback 클래스를 사용한 것을 볼 수 있습니다.

ListItemDiffCallback은 아래처럼 구현하였습니다.

```java
public class ListItemDiffCallback extends DiffUtil.Callback {
    private ArrayList<ListItemViewModel> oldListItemViewModels;
    private ArrayList<ListItemViewModel> newListItemViewModels;

    public ListItemDiffCallback(
            ArrayList<ListItemViewModel> oldListItemViewModels,
            ArrayList<ListItemViewModel> newListItemViewModels) {
        this.oldListItemViewModels = oldListItemViewModels;
        this.newListItemViewModels = newListItemViewModels;
    }

    @Override
    public int getOldListSize() {
        return oldListItemViewModels.size();
    }

    @Override
    public int getNewListSize() {
        return newListItemViewModels.size();
    }

    @Override
    public boolean areItemsTheSame(int oldItemPosition, int newItemPosition) {
        ListItemViewModel oldItem = oldListItemViewModels.get(oldItemPosition);
        ListItemViewModel newItem = newListItemViewModels.get(newItemPosition);

        return oldItem.equals(newItem);
    }

    @Override
    public boolean areContentsTheSame(int oldItemPosition, int newItemPosition) {
        ListItemViewModel oldItem = oldListItemViewModels.get(oldItemPosition);
        ListItemViewModel newItem = newListItemViewModels.get(newItemPosition);

        if (!oldItem.getTitle().equals(newItem.getTitle()))
            return false;
        if (!oldItem.getContent().equals(newItem.getContent()))
            return false;

        return true;
    }
}
```

# 5. RecyclerView 및 Button 추가

이제 main activity 에 RecyclerView와 Button을 추가하여 binding 시켜보겠습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <data>
        <variable
            name="viewModel"
            type="com.aptinfo.databindingrecylcerview.viewModel.ListViewModel" />
    </data>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity">
        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Add list"
            android:onClick="@{()->viewModel.addListItem()}" />
        <androidx.recyclerview.widget.RecyclerView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:scrollbars="vertical"
            app:items="@{viewModel.listItemViewModels}"
            app:layoutManager="LinearLayoutManager"/>
    </LinearLayout>
</layout>
```

button 클릭에 viewModel.addListItem이 호출되도록 하였고,

RecyclerView의 app:items 항목에 viewModel.listItemViewModels를 binding하였습니다.

app:items는 MainActivity class에 annotation을 달아 구현해주어야 합니다.

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
        binding.setViewModel(new ListViewModel());
    }

    @BindingAdapter("items")
    public static void setItems(RecyclerView recyclerView, ObservableArrayList<ListItemViewModel> listItemViewModels) {
        ListViewAdapter adapter;
        if (recyclerView.getAdapter() == null) {
            adapter = new ListViewAdapter();
            recyclerView.setAdapter(adapter);
        } else {
            adapter = (ListViewAdapter)recyclerView.getAdapter();
        }
        adapter.updateItems(listItemViewModels);
    }
}

```

# 6. 실행 결과

button click 시 list 가 추가되는걸 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-10-android-data-binding-recyclerview/1.png)

# 7. code

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/android/DataBindingRecyclerview) 에서 확인하실 수 있습니다.