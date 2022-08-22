# Android开发笔记总结



## 一、配置文件参考

### build.gradle

```json
implementation 'com.google.android.material:material:1.3.0' //导入material design
implementation 'com.github.bumptech.glide:glide:4.9.0' //导入图片加载的开源项目
implementation 'com.android.support:percent:28.0.0' //导入百分比布局
implementation 'org.litepal.android:core:2.0.0' //导入开源数据库litepal
implementation 'com.google.code.gson:gson:2.8.6' //导入Gson
debugImplementation 'com.amitshekhar.android:debug-db:1.0.6' //导入数据库查看工具
implementation 'org.jsoup:jsoup:1.14.3' //java爬虫
debugImplementation 'com.squareup.leakcanary:leakcanary-android:2.7' //内存泄漏检测

//lombok需要的文件
compileOnly 'org.projectlombok:lombok:1.18.16'
annotationProcessor 'org.projectlombok:lombok:1.18.16'
implementation "org.projectlombok:lombok:1.18.12"
```



### AndroidManifest.xml

```xml
<activity ...
	android:launchMode="singleTask" //页面启动模式
	android:screenOrientation="portrait" //保持竖屏  横屏：landscape
    android:usesCleartextTraffic="true" //允许使用http
	...>
	<intent-filter>
        <!--去掉页面黄色警告-->
		<action android:name="android.intent.action.VIEW" />
	</intent-filter>
</activity>
```



### colors.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
	<color name="colorPrimary">#7FB5FF</color>
	<color name="colorPrimaryDark">#2B84FF</color>
	<color name="colorAccent">#FFAE37</color>
	
	<!--自定义颜色-->
    <color name="myBackground">#F6F6F6</color>

    <color name="myWhite">#FFFFFFFF</color>
    <color name="myBlack">#3C3C3C</color>
    <color name="myGray">#D8D5D5</color>
    <color name="myGrayDark">#C8C8C8</color>
    <color name="myBlue">#7FB5FF</color>
    <color name="myBlueDark">#2B84FF</color>
    <color name="myGreen">#A8E066</color>
    <color name="myPink">#FF4081</color>
    <color name="myOrange">#FFAE37</color>

    <color name="myTextGray">#969696</color>
    <color name="myTextGreen">#41AB5D</color>
    <color name="myTextBlue">#4292C6</color>
    <color name="myTextOrange">#EF3B2C</color>
    <color name="myTextYellow">#FE9929</color>
</resources>
```





## 二、xml属性参考

### 全屏无标题栏

```xml
<application
    ...
    android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen">
    <activity android:name=".MainActivity">...</activity>
</application>
```



### 自定义背景

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
	<!-- 填充的颜色 -->
	<solid android:color="#fff"/>
    <!-- 渐变填充 -->
    <gradient
        android:angle="0"
        android:endColor="@color/myBlueDark"
        android:startColor="@color/myBlue"
        android:type="linear" />
    <!--边框的宽度及颜色-->
    <stroke
        android:width="8dp"
        android:color="@color/myWhite" />
    <!-- 设置按钮的四个角为弧形 -->
    <corners android:topRightRadius="12dp" />
</shape>
```



### 隐藏list划到头时的阴影

```xml
android:fadingEdge="none"
android:overScrollMode="never"
```



### 水波纹样式

```xml
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
	android:color="@color/mGreenDark"><!--点击前的颜色-->
	<item>
		<shape>
			<!--点击后的颜色-->
			<solid android:color="@color/mGreen" />
			<!--圆角-->
			<corners android:radius="8dp" />
		</shape>
	</item>
</ripple>
```



### 按钮内英文小写显示

```xml
android:textAllCaps="false"
```



### 布局对齐方式

```xml
android:orientation="vertical" //将LinearLayout设为纵向布局
android:layout_gravity="center" //水平居中
android:layout_centerVertical="true" //水平垂直居中，适用于RelativeLayout
```





## 三、java代码参考

### 开机广播

```xml
<!--AndroidManifest.xml权限-->

<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

<receiver android:name=".LaunchReceiver">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>
```

```java
public class LaunchReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Log.d("LaunchReceiver", "LaunchReceiver start log service");

        if (Intent.ACTION_BOOT_COMPLETED.equals(intent.getAction())) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                context.startForegroundService(new Intent(context, LogService.class));
            } else {
                context.startService(new Intent(context, LogService.class));
            }
        }
    }
}
```



### Handler

**匿名类形式创建**

```java
Handler mHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);
        //handler会发一条信息，在该方法中收到然后处理
        switch (msg.what) {
            case 0x1:
                break;
            default:
                break;
        }
    }
};

new Thread(new Runnable() {
    @Override
    public void run() {
        Message msg = Message.obtain(); //创建所需消息对象
        msg.what = 0x1;
        mHandler.sendMessage(msg); //发送消息
    }
}).start();
```

**使用post创建**

> post形式创建实际上是默认创建了一个msg.what为0的消息

```java
Handler mHandler = new Handler();

new Thread(new Runnable() {
    @Override
    public void run() {
        //在子线程中使用Handler.post()
        mHandler.post(new Runnable() {
            @Override
            public void run() {
                //执行的UI操作
            }
        });
    }
}).start();
```

**内部类形式创建**

```java
mHandler mhandler = new mHandler();

class mHandler extends Handler {
    @Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);
        //执行的UI操作
    }
}

new Thread(new Runnable() {
    @Override
    public void run() {
        Message msg = Message.obtain(); //创建所需消息对象
        msg.what = 1;
        mHandler.sendMessage(msg); //发送消息
    }
}).start();
```

**Handler常用api**

```java
//消息
Message message = Message.obtain();
//发送消息
new Handler().sendMessage(message);
//发送带标记的消息(内部创建了message,并设置msg.what = 0x1)
new Handler().sendEmptyMessage(0x1);

//延时1s发送消息
new Handler().sendMessageDelayed(message, 1000);
//延时1s发送带标记的消息
new Handler().sendEmptyMessageDelayed(0x1, 1000);
//延时1s发送消息
//（第二个参数为：相对系统开机时间的绝对时间，而SystemClock.uptimeMillis()是当前开机时间）
new Handler().sendMessageAtTime(message, SystemClock.uptimeMillis() + 1000);

//移除标记为0x1的消息
new Handler().removeMessages(0x1);
//移除回调的消息
new Handler().removeCallbacks(Runnable);
//移除回调和所有message
new Handler().removeCallbacksAndMessages(null);
```



### recyclerView

**通用RecyclerView**

```java
RecyclerView recyclerView = findViewById(R.id.recyclerView);
LinearLayoutManager layoutManager = new LinearLayoutManager(getContext());
recyclerView.setLayoutManager(layoutManager);

CommonBaseAdapter<mType> Adapter = new CommonBaseAdapter<>(list,
    new CommonBaseAdapter.OnBindDataInterface<mType>() {
        @Override
        public int getItemLayoutId(int viewType) { return R.layout.xxx; }

        @Override
        public void onBindData(BidNewsItem item, BaseViewHolder holder, int type) {
            holder.setText(R.id.xxx, item.getTitle());
            holder.setImage(R.id.xxx, item.getImgId());
            holder.setOnClickListener(R.id.xxx, v -> {
                Intent intent = new Intent(getContext(), xxxActivity.class);
                intent.putExtra("title", item.getTitle());
                startActivity(intent);
            });
        }
    });

recyclerView.setAdapter(Adapter);
recyclerView.setNestedScrollingEnabled(false); //隐藏滚动条
```

**RecyclerView常用API**

```java
canScrollHorizontally();//能否横向滚动
canScrollVertically();//能否纵向滚动
scrollToPosition(int position);//滚动到指定位置

setOrientation(int orientation);//设置滚动的方向
getOrientation();//获取滚动方向

findViewByPosition(int position);//获取指定位置的Item View
findFirstCompletelyVisibleItemPosition();//获取第一个完全可见的Item位置
findFirstVisibleItemPosition();//获取第一个可见Item的位置
findLastCompletelyVisibleItemPosition();//获取最后一个完全可见的Item位置
findLastVisibleItemPosition();//获取最后一个可见Item的位置
```

**RecyclerView刷新方法**

```java
notifyDataSetChanged() //刷新全部的item
notifyItemChanged(int) //刷新指定的item
notifyItemRangeChanged(int, int) //从指定的位置开始刷新指定个item
notifyItemInserted(int)、notifyItemMoved(int)、notifyItemRemoved(int) //插入、移动指定位置的item，并刷新(记得要重新设置下标)
notifyItemChanged(int, Object) //局部刷新指定的数据
```



### dialog

```java
AlertDialog.Builder dialog = new AlertDialog.Builder(getContext());
dialog.setMessage("标题");
dialog.setCancelable(false); //设置是否能通过返回键关闭

dialog.setPositiveButton("登录", (dialog1, which) -> {
});

dialog.setNegativeButton("取消", (dialog12, which) -> {
});

dialog.show(); //显示消息提示框
```



### 切换到下一页

```java
@Override
public void onClick(View view) {
	Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
	startActivity(intent);
}
```



### 手机号码正则

`^0?(13[0-9]|14[579]|15[012356789]|16[6]|17[0135678]|18[0-9]|19[89])[0-9]{8}$`（截止于2018.06.11）
https://blog.csdn.net/Hiking_Tsang/article/details/79698911





## 四、参考表汇总

### Intent Flag

| Flag                                | 说明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| FLAG_ACTIVITY_CLEAR_TASK            | 本flag能造成在新活动启动前，与新活动关联的任务被清空。也就是说，新活动成为新任务的根，旧的活动都被结束了。本flag只能与`FLAG_ACTIVITY_NEW_TASK`联合使用。 |
| FLAG_ACTIVITY_CLEAR_TOP             | 1. 新活动已在当前任务中时，在新活动上面的活动会被关闭，新活动不会重新启动，只会接收new intent<br />2. 新活动已在任务最上面时：如果启动模式是"multiple" (默认的)，并且没添加`FLAG_ACTIVITY_SINGLE_TOP`，那么活动会被销毁重新创建；如果启动模式是其他的，或者添加了`FLAG_ACTIVITY_SINGLE_TOP`，那么只会调用活动的`onNewIntent()`<br />3. 跟`FLAG_ACTIVITY_NEW_TASK`联合使用效果很好：如果用于启动一个任务中的根活动，会把该任务移到前面并清空至root状态。这特别有用，比如用于从notification manager中启动活动 |
| FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET | 已废弃。API 21后用`FLAG_ACTIVITY_NEW_DOCUMENT`               |
| FLAG_ACTIVITY_MULTIPLE_TASK         | 1. 用于创建一个新任务，并启动一个活动放进去； 总是跟`FLAG_ACTIVITY_NEW_DOCUMENT`或者`FLAG_ACTIVITY_NEW_TASK`一起使用； 单独用`FLAG_ACTIVITY_NEW_DOCUMENT`或者`FLAG_ACTIVITY_NEW_TASK`时，会在已存在的任务中寻找匹配的Intent，找不到才会创建一个新任务； 使用了本flag不会寻找匹配的Intent，无条件创建一个新任务<br />2. 用了`FLAG_ACTIVITY_NEW_TASK`就不要用本flag，除非你启动的是应用的launcher。 跟`FLAG_ACTIVITY_NEW_TASK`联合使用能防止把已存在的任务移到前面，会为新活动创建一个新任务，无论已存在的任务中有没有新活动<br />3. 因为默认安卓系统中没有提供可视化的任务管理，所以你不应该使用本flag，除非给用户提供可以回到其他任务的方法<br />4. 单独用本flag而不用`FLAG_ACTIVITY_NEW_DOCUMENT`或者`FLAG_ACTIVITY_NEW_TASK`是无效的 |
| FLAG_ACTIVITY_NEW_DOCUMENT          | 1. 本flag会给启动的活动开一个新的任务记录。使用了本flag或`documentLaunchMode`属性时，相同活动的多实例会在最近任务列表中产生不同的记录<br />2. 使用本flag比使用`documentLaunchMode`属性更好，因为`documentLaunchMode`属性会跟活动绑定，而flag只在需要时添加<br />3. 注意本flag的默认词义，活动销毁后最近任务列表中的入口不会移除。这跟使用`FLAG_ACTIVITY_NEW_TASK`不一样，后者活动销毁后入口会马上移除。你可以用`FLAG_ACTIVITY_RETAIN_IN_RECENTS`改变这个行为<br />4. 本flag可以跟`FLAG_ACTIVITY_MULTIPLE_TASK`联合使用。单独使用时跟manifest活动中定义`documentLaunchMode="intoExisting"`效果相同，联合使用时跟manifest活动中定义`documentLaunchMode="always"`效果相同。 |
| **FLAG_ACTIVITY_NEW_TASK**          | 1. 新活动会成为历史栈中的新任务（一组活动）的开始<br />2. 通常用于具有"launcher"行为的活动：让用户完成一系列事情，完全独立于之前的活动<br />3. 如果新活动已存在于一个为它运行的任务中，那么不会启动，只会把该任务移到屏幕最前<br />4. 如果新活动要返回result给启动自己的活动，就不能用这个flag |
| FLAG_ACTIVITY_NO_ANIMATION          | 本flag会阻止系统展示活动的当前状态到另一个状态之间的转移动画。这并不意味着永远没有动画 -- 如果另一项活动的改变在当前展示的活动启动前发生并且没有使用本flag，那么动画还会展示。当你要进行一系列活动操作，但是用户看到的动画不应该由第一项改变来驱动，而是由下一项。 |
| FLAG_ACTIVITY_NO_HISTORY            | 1. 新活动不会保留在历史栈中，一旦用户切换到其他页面，新活动会马上销毁<br />2. 旧活动的`onActivityResult()`方法永远不会被触发 |
| FLAG_ACTIVITY_REORDER_TO_FRONT      | 1. 如果新活动已在任务中，用本flag启动会将它移到任务的历史栈的前面<br />2. 如果用了`FLAG_ACTIVITY_CLEAR_TOP`，本flag就无效 |
| FLAG_ACTIVITY_RETAIN_IN_RECENTS     | 1. 默认情况下由`FLAG_ACTIVITY_NEW_DOCUMENT`创建的新纪录，用户关闭时（按返回键或其他方式结束）它在最近任务中的入口会被移除。如果你想保留入口，就用本flag<br />2. 接收的活动可以用`autoRemoveFromRecents`属性或者调用`Activity.finishAndRemoveTask()`来覆盖本请求。 |
| FLAG_ACTIVITY_SINGLE_TOP            | 新活动已存在历史栈的顶端时就不会重新启动                     |
| FLAG_ACTIVITY_FORWARD_RESULT        | （当前活动由源活动启动）本intent从当前活动启动新活动时，源活动的接收目标会从当前活动转移为新活动。新活动调用setResult的数据会传送给源活动 |
| FLAG_ACTIVITY_PREVIOUS_IS_TOP       | 本intent从当前活动启动新活动时，当前活动不会被视为顶端活动，不管是决定传intent给顶端还是启动新活动。新活动被当做顶端活动使用，假设当前活动立即销毁了 |
| FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS  | 新活动不会保存在最近启动的活动列表中                         |
| FLAG_ACTIVITY_BROUGHT_TO_FRONT      | 本flag一般不由应用代码设置，singleTask模式时系统会给你设置   |
| FLAG_ACTIVITY_RESET_TASK_IF_NEEDED  | 新活动在新任务中启动或者被放到一个已存在任务的顶端时，会被当做任务的前门来启动。这会导致任何相关性的活动在适当状态下需要拥有这个任务（无论移动活动到它里面或者是移走），或者在需要时简单地重置任务到初始状态 |
| FLAG_ACTIVITY_LAUNCHED_FROM_HISTORY | 本flag一般不由应用代码设置，活动从历史栈中启动（长按home键）时系统会给你设置 |
| FLAG_ACTIVITY_NO_USER_ACTION        | 1. 本flag会阻止当前最前面活动的onUserLeaveHint回调，在它被新启动的活动造成paused状态时<br />2. 通常，一个活动在受到用户操作而从前面移走的时候会调用上面的回调。该回调标志着活动生命周期中的一个点，在该点活动会隐藏它想要显示的”直到用户看到“的东西，比如闪烁的LED灯<br />3. 如果一个活动曾经由非用户驱动的事件比如来电或闹钟启动，应该在startActivity中添加本flag，以保证暂停时活动知道用户并没有看到通知 |
| FLAG_ACTIVITY_TASK_ON_HOME          | 本flag会造成新的启动任务放在当前主页活动任务（如果有的话）的顶端。也就是说，在任务中按返回键总是会回到主页，即使上一个用户看到的活动不是主页。本flag只能与`FLAG_ACTIVITY_NEW_TASK`联合使用 |
| FLAG_ACTIVITY_LAUNCH_ADJACENT       | 本flag只在分屏多窗口模式下使用。新活动会显示在旧活动旁边。本flag只能跟`FLAG_ACTIVITY_NEW_TASK`联合使用。并且如果你想创建一个已存在活动的新实例，那么要设置`FLAG_ACTIVITY_MULTIPLE_TASK` |

**详情请参考：**https://www.jianshu.com/p/2bdc16cba04f



### android自带样式

```xml
例：Android:theme="@android:style/Theme.Dialog" 将一个Activity显示为能话框模式
```

| 属性                                       | 作用                           |
| ------------------------------------------ | ------------------------------ |
| @android:style/Theme.Dialog                | 将一个Activity显示为对话框模式 |
| @android:style/Theme.NoTitleBar            | 不显示应用程序标题栏           |
| @android:style/Theme.NoTitleBar.Fullscreen | 不显示应用程序标题栏，并全屏   |
| Theme.Light                                | 背景为白色                     |
| Theme.Light.NoTitleBar                     | 白色背景并无标题栏             |
| Theme.Light.NoTitleBar.Fullscreen          | 白色背景，无标题栏，全屏       |
| Theme.Black                                | 背景黑色                   |@android:style/Theme.Dialog|将一个Activity显示为能话框模式|
|@android:style/Theme.NoTitleBar|不显示应用程序标题栏|
|@android:style/Theme.NoTitleBar.Fullscreen|不显示应用程序标题栏，并全屏|
|Theme.Light|背景为白色|
|Theme.Light.NoTitleBar|白色背景并无标题栏|
|Theme.Light.NoTitleBar.Fullscreen|白色背景，无标题栏，全屏|
|Theme.Black|背景黑色|
|Theme.Black.NoTitleBar|黑色背景并无标题栏|
|Theme.Black.NoTitleBar.Fullscreen|黑色背景，无标题栏，全屏|
|Theme.Wallpaper|用系统桌面为应用程序背景|
|Theme.Wallpaper.NoTitleBar|用系统桌面为应用程序背景，且无标题栏|
|Theme.Wallpaper.NoTitleBar.Fullscreen|用系统桌面为应用程序背景，无标题栏，全屏|
|Translucent|透明背景|
|Theme.Translucent.NoTitleBar|透明背景并无标题|
|Theme.Translucent.NoTitleBar.Fullscreen|透明背景并无标题，全屏|
|Theme.Panel|面板风格显示|
|Theme.Light.Panel|平板风格显示|



### 活动的四种启动模式

| 类型           | 作用                                                         |
| -------------- | ------------------------------------------------------------ |
| standard       | 默认启动模式，每次启动都会创建该活动的一个新的实例，不管这个活动是否已经存在 |
| singleTop      | 在启动活动时如果发现该活动已经在栈顶，就直接使用，不创建新的活动实例 |
| singleTask     | 在启动活动时如果发现该活动在栈中存在，就直接使用，同时让把这个活动之上的活动统统出栈，否则才创建新的活动实例 |
| singleInstance | 由单独的返回栈来管理这个活动，同时也解决了共享活动实例的问题 |



### 图片填充方式

```xml
android:scaleType="fitCenter" //填充方式
android:adjustViewBounds="true" //保证长宽比
```

| scaleType属性 | 作用                                                         |
| ------------- | ------------------------------------------------------------ |
| center        | 保持原图的大小，显示在ImageView的中心。当原图的size大于ImageView的size，超过部分裁剪处理 |
| centerCrop    | 以填满整个ImageView为目的，将原图的中心对准ImageView的中心，等比例放大原图，直到填满ImageView为止（指的是ImageView的宽和高都要填满），原图超过ImageView的部分作裁剪处理 |
| centerInside  | 以原图完全显示为目的，将图片的内容完整居中显示，通过按比例缩小原图的size宽(高)等于或小于ImageView的宽(高)。如果原图的size本身就小于ImageView的size，则原图的size不作任何处理，居中显示在ImageView |
| matrix        | 不改变原图的大小，从ImageView的左上角开始绘制原图，原图超过ImageView的部分作裁剪处理 |
| fitCenter     | 把原图按比例扩大或缩小到ImageView的高度，居中显示            |
| fitEnd        | 把原图按比例扩大(缩小)到ImageView的高度，显示在ImageView的下部分位置 |
| fitStart      | 把原图按比例扩大(缩小)到ImageView的高度，显示在ImageView的上部分位置 |
| fitXY         | 把原图按照指定的大小在View中显示，拉伸显示图片，不保持原比例，填满ImageView |



### drawable state属性参考

| 属性                         | 作用                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| android:state_pressed        | 是否按下，如一个按钮触摸或者点击                             |
| android:state_focused        | 是否取得焦点，比如用户选择了一个文本框                       |
| android:state_hovered        | 光标是否悬停，通常与focused state相同，它是4.0的新特性       |
| android:state_selected       | 被选中，它与focus state并不完全一样，如一个list view被选中的时候，它里面的各个子组件可能通过方向键，被选中了 |
| android:state_checkable      | 组件是否能被check。如：RadioButton是可以被check的            |
| android:state_checked        | 被checked了，如：一个RadioButton可以被check了                |
| android:state_enabled        | 能够接受触摸或者点击事件                                     |
| android:state_activated      | 被激活                                                       |
| android:state_window_focused | 应用程序是否在前台，当有通知栏被拉下来或者一个对话框弹出的时候应用程序就不在前台了 |

**注意：**如果有多个item，那么程序将自动从上到下进行匹配，最先匹配的将得到应用（不是通过最佳匹配），如果一个item没有任何的状态说明，那么它将可以被任何一个状态匹配



### 权限参考

**网络权限**

```xml
允许应用访问网络
<uses-permission android:name="android.permission.INTERNET" />

允许应用获取网络信息状态，如当前的网络连接是否有效
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

允许应用程序更改网络连接的状态，如是否能联网
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />

允许应用获取当前WiFi接入的状态，以及WLAN热点的信息
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

允许应用程序连接或断开WLAN接入点，并对配置的WLAN网络进行更改
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />

允许应用接收WLAN多播
设置该权限后，允许应用接收并非直接向设备发送的数据，比如查找附近提供的服务，WLAN多播所耗电量大于非多播模式。
<uses-permission android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE" />
```

**蓝牙权限**

```xml
允许应用连接配对过的蓝牙设备
<uses-permission android:name="android.permission.BLUETOOTH" />

允许应用进行发现和配对新的蓝牙设备
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
```

**安装权限**

```xml
允许应用安装全新或更新的Android包
<uses-permission android:name="android.permission.INSTALL_PACKAGES" />

允许应用添加桌面快捷图标
<uses-permission android:name="com.android.launcher.permission.INSTALL_SHORTCUT" />
```

**拍照、闪光灯、电池电量、屏幕截图权限**

```xml
允许应用使用相机拍照
<uses-permission android:name="android.permission.CAMERA"/>

允许应用控制闪光灯
<uses-permission android:name="android.permission.FLASHLIGHT" />

允许应用获取电池电量
<uses-permission android:name="android.permission.BATTERY_STATS" />

允许应用屏幕截图
<uses-permission android:name="android.permission.READ_FRAME_BUFFER" />
```

**位置权限**

```xml
允许应用获取大概的位置源
例如蜂窝网络数据库以确定手机的大概位置，通过WiFi或移动基站的方式获取用户错略的经纬度信息，定位精度大概误差在30~500米。
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

允许应用获取精确的位置源
通过GPS芯片接收卫星的定位信息，定位精度达0米以内，使用GPS功能会消耗额外的电池电量。
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

允许应用模拟定位信息
创建模拟地点来源进行测试，一般用于帮助开发者调试应用。
恶意应用程序可能利用此选项覆盖由真实地点来源（如GPS或网络提供商）传回的地点和状态。
<uses-permission android:name="android.permission.ACCESS_MOCK_LOCATION" />
```

**外部存储权限**

```xml
允许应用读取外部存储，如读取SD上的文件
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

允许应用写入外部存储，如向SD卡上写入文件
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

**手机状态权限**

```xml
允许应用读取手机状态
有此权限的应用程序，可确定此手机的号码和序列号、是否正在通话、以及对方的号码等。
<uses-permission android:name="android.permission.READ_PHONE_STATE" />

允许应用修改手机状态
允许应用程序控制设备的电话功能，如修改为飞行模式。
拥有此权限的应用程序可自行切换网络、打开和关闭无线通信等，这些操作并不会通知你。
<uses-permission android:name="android.permission.MODIFY_PHONE_STATE" />
```

**录音权限**

```xml
允许应用访问录音路径
<uses-permission android:name="android.permission.RECORD_AUDIO" />

允许应用修改整个系统的音频设置，如音量和路由
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
```

**日程权限**

```xml
允许应用读取日程提醒
<uses-permission android:name="android.permission.READ_CALENDAR" />

允许应用写入日程提醒
<uses-permission android:name="android.permission.WRITE_CALENDARR" />
```

**短信权限**

```xml
允许应用读取手机或SIM卡中短信的内容
<uses-permission android:name="android.permission.READ_SMS" />

允许应用接收短信
<uses-permission android:name="android.permission.RECEIVE_SMS" />

允许应用接收彩信
<uses-permission android:name="android.permission.RECEIVE_MMS" />

允许应用发送短信
<uses-permission android:name="android.permission.SEND_SMS" />
```

**联系人权限**

```xml
允许应用读取联系人
<uses-permission android:name="android.permission.READ_CONTACTS" />

允许应用写入联系人
<uses-permission android:name="android.permission.WRITE_CONTACTS" />

允许应用读取联系人（超级权限）
<uses-permission android:name="android.permission.READ_CONTACTS_SUPER" />

允许应用写入联系人（超级权限）
<uses-permission android:name="android.permission.WRITE_CONTACTS_SUPER" />
```

**android 11文件完整读写权限**

```xml
写外部存储（分区存储）
<uses-permission
	android:name="android.permission.WRITE_EXTERNAL_STORAGE"
	tools:ignore="ScopedStorage" />
访问外部存储（分区存储）
<uses-permission
	android:name="android.permission.MANAGE_EXTERNAL_STORAGE"
	tools:ignore="ScopedStorage" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

**双屏异显**

```xml
显示系统窗口权限
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
在屏幕最顶部显示权限
<uses-permission android:name="android.permission.SYSTEM_OVERLAY_WINDOW"/>
```

**监听开机广播**

```xml
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
```





### tools属性参考

| UI预览相关属性                       | 作用                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| tools: (instead of android)          | 可替代任何`android:`开头的属性，例如`tool:text`              |
| tools:menu                           | 在app bar中显示的menu布局                                    |
| tools:listitem/listheader/listfooter | 在列表布局中直接预览到item布局，例`tools:listitem="@layout/sample_list_item"` |
| tools:showIn                         | 适用于被 `<include>` 标签引用的布局根 `<view>`，使用此标签可以直接在被引用的地方直接看到显示效果，例`tools:showIn="@layout/activity_main"` |
| tools:layout                         | 在布局预览界面预览fragment的显示视图，例`tools:layout="@layout/list_content"` |
| tools:context                        | 告诉系统哪个Activity和当前元素绑定，在开发工具的快速修复中使用 |
| tools:shrinkMode                     | 该属性允许你指定构建工具是否使用`safe mode`安全模式 （该模式会保留所有明确引用的资源以及可能被动态引用的资源）默认情况下`shrinkMode="safe"`，如果需要使用严格模式，则在` <resources> `中设置 `tools:shrinkMode="strict"` |
| tools:actionBarNavMode               | 这个属性告诉ide app bar（Material中对actionbar的称呼）的显示模式，其值可以是[`standard` |


| **Lint相关的属性** | 作用                                                        |
| ------------------ | ----------------------------------------------------------- |
| tools:ignore       | 告诉Lint忽略xml中的某些警告                                 |
| tools:targetApi    | 忽略版本警告，例如`tools:targetApi="14"`                    |
| tools:locale       | 指定默认的资源文件所使用的语言环境，例如`tools:locale="zh"` |



### themes文件的样式名称

| 样式名称                               | 作用位置                                                     |
| -------------------------------------- | ------------------------------------------------------------ |
| colorPrimary                           | 主要品牌颜色，一般用于ActionBar背景                          |
| colorPrimaryDark                       | 默认用于顶部状态栏和底部导航栏                               |
| colorPrimaryVariant                    | 主要品牌颜色的可选颜色                                       |
| colorSecondary                         | 第二品牌颜色                                                 |
| colorSecondaryVariant                  | 第二品牌颜色的可选颜色                                       |
| colorPrimarySurface                    | 对应Light主题指向colorPrimary，Dark主题指向colorSurface      |
| colorOn[Primary, Secondary, Surface …] | 在Primary等这些背景的上面内容的颜色，例如ActioBar上面的文字颜色 |
| colorAccent                            | 默认设置给colorControlActivated，一般是主要品牌颜色的明亮版本补充 |
| colorControlNormal                     | 图标和控制项的正常状态颜色                                   |
| colorControlActivated                  | 图标和控制项的选中颜色（例如Checked或者Switcher）            |
| colorControlHighlight                  | 点击高亮效果（ripple或者selector）                           |
| colorButtonNormal                      | 按钮默认状态颜色                                             |
| colorSurface                           | cards, sheets, menus等控件的背景颜色                         |
| colorBackground                        | 页面的背景颜色                                               |
| colorError                             | 展示错误的颜色                                               |
| textColorPrimary                       | 主要文字颜色                                                 |
| textColorSecondary                     | 可选文字颜色                                                 |



### Android颜色透明度

| 透明度 | 数值 | 透明度 | 数值 |
| ------ | ---- | ------ | ---- |
| 100%   | FF   | 95%    | F2   |
| 90%    | E6   | 85%    | D9   |
| 80%    | CC   | 75%    | BF   |
| 70%    | B3   | 65%    | A6   |
| 60%    | 99   | 55%    | 8C   |
| 50%    | 80   | 45%    | 73   |
| 40%    | 66   | 35%    | 59   |
| 30%    | 4D   | 25%    | 40   |
| 20%    | 33   | 15%    | 26   |
| 10%    | 1A   | 5%     | 0D   |

https://blog.csdn.net/hewuzhao/article/details/78821954



### Activity的七种生命周期

```java
public static final String TAG = "当前的页面";

@Override
protected void onCreate() {
        super.onCreate();
        Log.d(TAG, "onCreate: 在活动第一次被创建的时候调用");
}

@Override
protected void onStart() {
    super.onStart();
    Log.d(TAG, "onStart: 在活动由不可见变为可见时调用");
}

@Override
protected void onResume() {
    super.onResume();
    Log.d(TAG, "onResume: 在活动准备好和用户进行交互的时候调用");
}

@Override
protected void onPause() {
    super.onPause();
    Log.d(TAG, "onPause: 在系统准备去启动或者恢复另一个活动的时候调用");
}

@Override
protected void onStop() {
    super.onStop();
    Log.d(TAG, "onStop: 在活动完全不可见的时候调用");
}

@Override
protected void onDestroy() {
    super.onDestroy();
    Log.d(TAG, "onDestroy: 在活动被销毁之前调用，之后活动的状态将变为销毁状态");
}

@Override
protected void onRestart() {
    super.onRestart();
    Log.d(TAG, "onRestart: 在活动由停止状态变为运行状态之前调用(活动重启)");
}
```



### 四大组件

**activity**

一个Activity通常就是一个单独的屏幕（窗口），Activity之间通过Intent进行通信

**service**

service用于在后台完成用户指定的操作。service分为两种：

- started（启动）：当应用程序组件（如activity）调用startService()方法启动服务时，服务处于started状态
- bound（绑定）：当应用程序组件调用bindService()方法绑定到服务时，服务处于bound状态

startService()与bindService()区别：

- started service（启动服务）是由其他组件调用startService()方法启动的，这导致服务的onStartCommand()方法被调用。**当服务是started状态时，其生命周期与启动它的组件无关，并且可以在后台无限期运行，即使启动服务的组件已经被销毁。**因此，服务需要在完成任务后调用stopSelf()方法停止，或者由其他组件调用stopService()方法停止

- 使用bindService()方法启用服务，调用者与服务绑定在了一起，调用者一旦退出，服务也就终止

**content provider（内容提供器）**

ContentProvider实现数据共享。ContentProvider用于保存和获取数据，并使其对所有应用程序可见。这是不同应用程序间共享数据的唯一方式，因为android没有提供所有应用共同访问的公共存储区

ContentProvider使用URI来唯一标识其数据集，这里的URI以content://作为前缀，表示该数据由ContentProvider来管理

broadcast receiver（广播）

广播接收者的注册有两种方法，分别是程序动态注册和AndroidManifest文件中进行静态注册

动态注册广播接收器特点是当用来注册的Activity关掉后，广播也就失效了。**静态注册无需担忧广播接收器是否被关闭，只要设备是开启状态，广播接收器也是打开着的。**也就是说哪怕app本身未启动，该app订阅的广播在触发时也会对它起作用

**总结：**

4大基本组件都需要注册才能使用，每个Activity、service、Content Provider都需要在AndroidManifest文件中进行配置。**AndroidManifest文件中未进行声明的activity、服务以及内容提供者将不为系统所见**，从而也就不可用

内容提供者的激活：当接收到ContentResolver发出的请求后，内容提供者被激活。而其它三种组件activity、服务和广播接收器被一种叫做intent的异步消息所激活



### 六大布局

**LinearLayout（线性布局）**

 ```xml
android:orientation="horizontal" //从左到右
android:orientation="vertical"  //从上到下
 ```

**RelativeLayout（相对布局）**

参考其他控件进行布局，默认为父控件

三种类型的属性：

1. 属性值是`true`或`false`

    ```xml
    android:layout_centerHrizontal //水平居中
    android:layout_centerVertical //垂直居中
    android:layout_centerInparent //相对于父元素完全居中。
    android:layout_alignParentBottom //位于父元素的下边缘
    android:layout_alignParentTop //位于父元素的上边缘
    android:layout_alignParentLeft //位于父元素的左边缘
    android:layout_alignParentRight //位于父元素的右边缘
    ```

 2. 属性值是`"@id/*"`

    ```xml
    android:layout_below //在某元素的下方
    android:layout_above //在某元素的上方
    android:layout_toRightOf //在某元素的右方
    android:layout_toLeftOf //在某元素的左方
    android:layout_alignBottom //和某元素下方对齐
    android:layout_alignTop //和某元素上方对齐
    android:layout_alignRight //和某元素右方对齐
    android:layout_alignLeft //和某元素左方对齐
    ```

 3. 属性值是数值

    ```xml
    android:layout_marginLeft //离某元素左边缘的距离
    android:layout_marginRight //离某元素右边缘的距离
    android:layout_marginTop //离某元素上边缘的距离
    android:layout_marginBottom //离某元素下边缘的距离
    ```

**MyLayout（自定义布局）**

自定义ViewGroup，主要是重写两个方法：

 ```xml
onMeasure() //这个是写自定义容器的大小
onLayout() //这个是写子元素的布局
 ```

**FrameLayout（框架布局）**

帧布局（特点是从左上角开始，后面的会覆盖前面的控件）

**TableLayout（表格布局）**

 写在TableLayout中的属性 

 ```xml
android:stretchColumns //设置第几列为伸展(0表示第一列)
android:shrinkColumns //设置第几列为收缩
android:collapseColumns //设置第几列为隐藏
 ```

 写在TableRow里的控件里的属性

 ```xml
android:layout_column //设置控件在第几列
android:layout_span //设置控件能跨多少列
 ```

**AbsoluteLayout（绝对布局）（官方已经舍弃）**



### 文件访问权限字母解析

`drwxrwxrwx`：一共是10个字符，而表示权限就只有后面的9位，第1位不算，因为第1位代表文件的是当前文件的一种属性，如下：

| 字母 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| d    | 表示目录                                                     |
| -    | 表示文件                                                     |
| l    | 表示链接文件（linkfile）                                     |
| b    | 表示设备文件里面的可供存储的接口设备                         |
| c    | 表示设备文件里面的串行端口设备，例如键盘、鼠标（一次性读取设备） |
| r    | 读取权限，权值为4                                            |
| w    | 更改权限，权值为2                                            |
| x    | 执行权限，权值为1                                            |

接下来的字符中3个为一组来划分就是：`rwx`	`rwx`	`rwx`

| 次序      | 含义               |
| --------- | ------------------ |
| 第一组rwx | 用户的文件权限     |
| 第二组rwx | 用户组的文件权限   |
| 第三组rwx | 其他用户的文件权限 |

因为`rwxrwxrex`按数值来分，r=4；w=2；x=1，所以如果全部权限打开就是`777`





## 五、库导入和工具使用参考

### Litepal配置

在`main`文件中创建`assets`文件（和`java`文件夹平级）

在`assets`文件夹中创建`litepal.xml`

```xml
<?xml version="1.0" encoding="utf-8" ?>
<litepal>
    <!-- 数据库名称 -->
    <dbname value="xxx" />
    <!-- 数据库版本，如果新增了表，版本要+1 -->
    <version value="1" />

    <list>
        <!-- 映射的实体类 -->
        <mapping class="com.example.taxation.bean.UserInfo" />
    </list>

</litepal>
```

在`AndroidManifest.xml`中增加`android:name="org.litepal.LitePalApplication"`

```xml
...
<application
	android:name="org.litepal.LitePalApplication"
	android:allowBackup="true"
    android:icon="@drawable/app_icon"
    android:label="@string/app_name"
    android:roundIcon="@drawable/app_icon"
    android:supportsRtl="true"
    android:theme="@style/Theme.XXX">
...
```

实体类要继承`LitePalSupport `

```java
public class UserInfo extends LitePalSupport {
    private String username;
    private String password;
    ...
}
```

简单使用

```java
//保存数据到数据库
List<UserInfo> infoList = ...
LitePal.saveAll(infoList);

//读取第一条数据
UserInfo info = LitePal.findFirst(UserInfo.class);

//读取所有数据
UserInfo info = LitePal.findAll(UserInfo.class);

//条件查询
List<info> queryResult = LitePal.where("username like ?", "aaa").find(username.class);
```



### butterKnife配置

> 新版Android Studio中已被弃用，改用新的绑定方式

```json
//Module: build.gradle
implementation 'com.jakewharton:butterknife:10.2.3'
annotationProcessor 'com.jakewharton:butterknife-compiler:10.2.3'

//Project: build.gradle
classpath 'com.jakewharton:butterknife-gradle-plugin:10.2.3'
```



### 数据库查看工具使用指导

**模拟器：**在Terminal中输入 adb forward tcp:8080 tcp:8080，然后打开http://localhost:8080/

**真机：**确保在同一局域网下，打开log中显示的网址

https://github.com/amitshekhariitbhu/Android-Debug-Database



### 反编译命令

提取apk资源：`java -jar [apktool_2.3.3.jar] d -f [安装包名称.apk] -o [文件夹名称]`
提取apk源代码：`...\dex2jar-2.0\dex2jar-2.0> [d2j-dex2jar] [classes.dex]`





## 六、错误解决相关参考

### Build控制台乱码

工具栏：Help -> Edit Custom VM Options，打开一个`.vmoptions`文件后，添加一行：`-Dfile.encoding=UTF-8`，点击项目的编译按钮（小锤子）和运行按钮，然后重启android studio

一般编译通过就不会有问题，重启后就能看到正常效果。如果启动失败，去android studio的bin目录下，找到：`studio.exe.vmoptions`和`studio64.exe.vmoptions`，用记事本打开，删除掉刚才添加的一行

错误原因：我们装的java，里面的javac输出的都是中文语言，但是android studio不能识别中文，所以全变成了乱码



### 界面无法实时预览

在`...\app\src\main\res\values\styles.xml`中将`<style>`中的`Theme.AppCompat...`之前加上`Base.`，变成`Base.Theme.AppCompat...`



### 后台service创建前台activity无效

现象大致为：前台无应用的activity，同时后台的service想使用`startActivity()`创建一个acitivity出来，如果activity刚被关闭没多久，此方法是有效的，但是过一会此方法将不起作用，且无报错，其实问题并不是activity被移出队列或被杀掉，是因为在android 10+上，限制了后台启动Service和Activity的时间。

一般允许Service在被创建后的十秒内启动Activity，这个时候是可以启动成功的，但是十秒后就禁止启动Activity。对于这种情况需要申请悬浮窗权限：`"android.permission.SYSTEM_ALERT_WINDOW`，同时在代码中发起权限请求，由用户手动授权。

```xml
//AndroidManifest.xml
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
```

```java
//startActivity
Intent intent = new Intent(getApplicationContext(), MainActivity.class);
intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
```

