---
layout: post
title: 2018-06-02-ListView1
date: 2018-06-02
tag: Android
description: 安卓基础知识1
---



## ListView

### 静态的词条匹配

通过android:entries属性来创建

1. 首先在res/values下创建arrays.xml文件

``` 
<ListView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/listView"
        android:dividerHeight="30dp"          设置间隔宽度
        android:divider="@color/colorAccent"     设置间隔颜色
        android:fadingEdge="vertical"         fadingEdge:拉滚动条时 ，边框渐变的放向,变淡
        android:fastScrollEnabled="true"       设置快速滚动
        android:entries="@array/name"         匹配静态的数组
        android:listSelector="#00ff00"        设置被选中列表的颜色
        android:layout_alignParentStart="true"   
        android:layout_marginStart="24dp" />
		
```


2. 加载到Activity

``` 
public class MainActivity extends AppCompatActivity {
    private ListView listView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        listView = (ListView) findViewById(R.id.listView);
		//添加时间监听
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                TextView tv = (TextView) view;
                Toast.makeText(MainActivity.this,tv.getText(),Toast.LENGTH_SHORT).show();
            }
        });
    }
    
}

```

### ListView专用的Activity：ListActivity

注意：对于 ListView专用的Activity：ListActivity来说，他不需要配置布局文件，只需要有数据文件

 1. 首先在res/values下创建arrays.xml文件

``` 
<ListView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/listView"
        android:dividerHeight="30dp"          设置间隔宽度
        android:divider="@color/colorAccent"     设置间隔颜色
        android:fadingEdge="vertical"         fadingEdge:拉滚动条时 ，边框渐变的放向,变淡
        android:fastScrollEnabled="true"       设置快速滚动
        android:entries="@array/name"         匹配静态的数组
        android:listSelector="#00ff00"        设置被选中列表的颜色
        android:layout_alignParentStart="true"   
        android:layout_marginStart="24dp" />

```

2. Activity

``` 
public class Main3Activity extends ListActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
		
		//使用android提供的样式
        ArrayAdapter arrayAdapter = ArrayAdapter.createFromResource(this, R.array.name, android.R.layout.simple_list_item_1);
        setListAdapter(arrayAdapter);
    }

	//设置事件监听
    @Override
    protected void onListItemClick(ListView l, View v, int position, long id) {
        Toast.makeText(this,((TextView)v).getText(),Toast.LENGTH_SHORT).show();
    }
}

```


### ListView和ListActivity单选框和多选框

1. xml文件布局

``` 
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:showIn="@layout/activity_main4ctivity"
    tools:context="com.example.tianmengmeng.coding8.Main4ctivity">

    <ListView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/listView2"
        android:layout_alignParentTop="true"
        android:layout_alignParentEnd="true" />
</RelativeLayout>
```

2. Activity

``` 
public class Main4ctivity extends AppCompatActivity {

    private ListView listView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main4ctivity);

        listView = (ListView) findViewById(R.id.listView2);

        String[] arr = getResources().getStringArray(R.array.name);
		
		//单选框
//        ArrayAdapter<String> arrayAdapter = new ArrayAdapter<String>(this,android.R.layout.simple_list_item_single_choice,arr);
//        listView.setChoiceMode(ListView.CHOICE_MODE_SINGLE);

	   //多选框
        ArrayAdapter<String> arrayAdapter1 = new ArrayAdapter<String>(this,android.R.layout.simple_list_item_multiple_choice,arr);
        listView.setChoiceMode(ListView.CHOICE_MODE_MULTIPLE);

        listView.setAdapter(arrayAdapter1);
    }

}

```

### ListView实现图文列表

1. xml布局文件

``` 
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:showIn="@layout/activity_main5"
    tools:context="com.example.tianmengmeng.coding8.Main5Activity">

    <ListView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/listView3"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true" />
</RelativeLayout>
```

2. ListView每单元的布局文件

``` 
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="center_vertical"
    android:padding="16dp">

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/imageView"
        android:src="@drawable/ic_launcher"
        android:layout_marginLeft="10dp"
        />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="New Text"
        android:id="@+id/textView2" />
</LinearLayout>
```

3. Activity

``` 
public class Main5Activity extends AppCompatActivity {

    private ListView listView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main5);

        listView = (ListView) findViewById(R.id.listView3);

        //准备数据，每一个HashMap是一条记录
		
		//这俩个内容对应着Map的两个key，"title"、"icon"
        HashMap<String,Object> title1 = new HashMap<>();
        title1.put("title","title-1");
        title1.put("icon",android.R.drawable.alert_dark_frame);
        HashMap<String,Object> title2 = new HashMap<>();
        title2.put("title", "title-2");
        title2.put("icon",android.R.drawable.arrow_up_float);
        HashMap<String,Object> title3 = new HashMap<>();
        title3.put("title", "title-3");
        title3.put("icon",android.R.drawable.bottom_bar);

		//适配器中是ArrayList类型的数据，ArrayList中每条是ListView的一行数据
        ArrayList<Map<String,Object>> list = new ArrayList<>();
        list.add(title1);
        list.add(title2);
        list.add(title3);

        //把数据填充到Adapter中
        SimpleAdapter sa = new SimpleAdapter(this,list,R.layout.list_item_5,new String[]{"title","icon"},new int[]{R.id.textView2,R.id.imageView});
        listView.setAdapter(sa);
    }

}


```

|   L  |   文字Map  |   图片Map  |   
| --- | --- | --- | 
|   I  |   文字Map  |    图片Map |   
|   S  |   文字Map  |   图片Map |     
|    T   |  文字Map |   图片Map  |     


### 自定义的ListView

1. xml文件，添加ListView


``` 
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:showIn="@layout/activity_main6"
    tools:context="com.example.tianmengmeng.coding8.Main6Activity">

    <ListView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/listView4"
        android:layout_alignParentTop="true"
        android:layout_alignParentEnd="true" />
</RelativeLayout>

```

2. 自定义的ListView每行的布局文件

``` 
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="center_vertical"
    android:padding="16dp">

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/imageView"
        android:src="@drawable/ic_launcher"
        android:layout_marginLeft="10dp"
        />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="New Text"
        android:id="@+id/textView2" />
</LinearLayout>
```

3. Activity，绑定自定义的适配器

``` 
public class Main6Activity extends AppCompatActivity {

    private ListView listView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main6);

        listView = (ListView) findViewById(R.id.listView4);
        listView.setAdapter(new MyAdapter(this));


    }


    static class MyAdapter extends BaseAdapter{
        private String[] titles = {"title-1","title-2","title-3","title-4","title-5"};
        private int[] icons = {android.R.drawable.ic_btn_speak_now,
                android.R.drawable.alert_dark_frame,
                android.R.drawable.alert_light_frame,
                android.R.drawable.arrow_down_float,
                android.R.drawable.arrow_up_float,
        };
        private Context context;

        public MyAdapter(Context context){
            this.context = context;
        }

        @Override
        public int getCount() {
            return titles.length;
        }

        @Override
        public Object getItem(int position) {
            return titles[position];
        }

        @Override
        public long getItemId(int position) {
            return position;
        }

        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
		
			//获取资源文件
            LayoutInflater inflater = LayoutInflater.from(context);
            View view = inflater.inflate(R.layout.list_item_6, null);

			//获取布局文件
            TextView textView = (TextView) view.findViewById(R.id.textView2);
            ImageView imageView = (ImageView) view.findViewById(R.id.imageView);
			
			//添加文字图片到布局中
            textView.setText(titles[position]);
            imageView.setImageResource(icons[position]);


			//返回视图
            return view;
        }
    }

}

```
