# ParallaxEffects 视差特效

- 了解ImageView 的scaleType 属性
- 掌握ListView 的overScrollBy()方法

应用场景：QQ 空间，微信朋友圈，微博，需要快速定位的列表效果图

![](http://img.blog.csdn.net/20170218095423642?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYXhpMjk1MzA5MDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) ![](http://img.blog.csdn.net/20170218095533455?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYXhpMjk1MzA5MDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 界面初始化

## 填充ListView

自定义ParallaxListView 继承ListView


```java
public class ParallaxListView extends ListView {
    public ParallaxListView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }
    public ParallaxListView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    public ParallaxListView(Context context) {

        super(context);
    }
}
```

填充ListView 的数据

```java
public class Cheeses {
    public static final String[] NAMES = new String[]{"宋江", "卢俊义", "吴用",
            "公孙胜", "关胜", "林冲", "秦明", "呼延灼", "花荣", "柴进", "李应", "朱仝", "鲁智 深",
            "武松", "董平", "张清", "杨志", "徐宁", "索超", "戴宗", "刘唐", "李逵", "史进", " 穆弘",
            "雷横", "李俊", "阮小二", "张横", "阮小五", " 张顺", "阮小七", "杨雄", "石秀", " 解珍",
            " 解宝", "燕青", "朱武", "黄信", "孙立", "宣赞", "郝思文", "韩滔", "彭玘", "单廷珪 ",
            "魏定国", "萧让", "裴宣", "欧鹏", "邓飞", " 燕顺", "杨林", "凌振", "蒋敬", "吕方 ",
            "郭盛", "安道全", "皇甫端", "王英", "扈三娘", "鲍旭", "樊瑞", "孔明", "孔亮", " 项充",
            "李衮", "金大坚", "马麟", "童威", "童猛", "孟康", "侯健", "陈达", "杨春", "郑天寿 ",
            "陶宗旺", "宋清", "乐和", "龚旺", "丁得孙", "穆春", "曹正", "宋万", "杜迁", "薛永 ", "施恩",
            "周通", "李忠", "杜兴", "汤隆", "邹渊", "邹润", "朱富", "朱贵", "蔡福", "蔡庆", " 李立",
            "李云", "焦挺", "石勇", "孙新", "顾大嫂", "张青", "孙二娘", " 王定六", "郁保四", " 白胜",
            "时迁", "段景柱"};
}
```

activity_main.xml
```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:tools="http://schemas.android.com/tools"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                tools:context=".MainActivity" >
    <com.example.parallax.widget.ParallaxListView
        android:id="@+id/plv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</RelativeLayout>
```
MainActivity 填充数据
```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    requestWindowFeature(Window.FEATURE_NO_TITLE);
    setContentView(R.layout.activity_main);
    plv = (ParallaxListView) findViewById(R.id.plv);
    plv.setAdapter(new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, Cheeses.NAMES));
}
```

## ListView 添加Header
创建header 布局文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

    <ImageView
        android:id="@+id/iv_header"
        android:layout_width="match_parent"
        android:layout_height="160dp"
        android:contentDescription="@null"
        android:scaleType="centerCrop"
        android:src="@drawable/parallax_img"/>
</RelativeLayout>
```
第10 行scaleType 为图片的填充模式

## 图片填充模式对比

![](http://img.blog.csdn.net/20161130100825999) ![](http://img.blog.csdn.net/20161130100854375)

- matrix:图片宽高不变，以ImageView 左上角为基准向右向下填充ImageView
- fixXY:图片宽高分别拉伸或压缩，以ImageView 左上角为基准向右向下填充满ImageView 的宽和高
- fitStart:图片宽高分别拉伸或压缩，以ImageView 左上角为基准向右向下填充满ImageView 的宽，ImageView的高不用管
- fitCenter:图片宽高分别拉伸或压缩，图片居中显示，填充满ImageView 的宽，ImageView 的高不用管
- fitEnd:图片宽高分别拉伸或压缩，以ImageView 左下角为基准向右向上填充满ImageView 的宽，ImageView的高不用管
- center:图片宽高不变，图片居中显示，填充ImageView
- centerCrop:图片宽高分别拉伸或压缩，图片居中显示，直到填充满ImageView
- centerInside:原图比ImageView 小，图片居中显示，填充ImageView，原图比ImageView 大，图片宽高分别拉伸或压缩，直到填充满ImageView 的宽或高即可

MainActivity.java 中添加头布局
```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    requestWindowFeature(Window.FEATURE_NO_TITLE);
    setContentView(R.layout.activity_main);
    plv = (ParallaxListView) findViewById(R.id.plv);
    View headerView = View.inflate(this,R.layout.layout_header, null);
    //添加头布局，需要在setAdapter 前添加
    plv.addHeaderView(headerView);
    plv.setAdapter(new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1,Cheeses.NAMES));
}
```

# 下拉放大
## overScrollBy()方法参数解析
ParallaxListView 需要重写overScrollBy()方法，api 要求9 以上

```java
/**
 * 滑动到ListView 两端才会调用
 */
@Override
protected boolean overScrollBy(int deltaX, int deltaY, int scrollX,
                               int scrollY, int scrollRangeX, int scrollRangeY,
                               int maxOverScrollX, int maxOverScrollY, boolean isTouchEvent) {
    //deltaY 竖直方向滑动的瞬时变化量，顶部下拉为-，底部上拉为+
    //scrollY 两端滑动超出的距离，顶部为-，底部为+
    //scrollRangeY 竖立方向滑动的范围
    //maxOverScrollY 竖立方向最大的滑动位置
    //isTouchEvent 是否是用户触摸拉动，true 表示用户手指触摸拉动，false 表示惯性
    System.out.println("deltaY：" + deltaY + " scrollY:" + scrollY
            + " scrollRangeY:" + scrollRangeY + " maxOverScrollY:"
            + maxOverScrollY + " isTouchEvent:" + isTouchEvent);
    return super.overScrollBy(deltaX, deltaY, scrollX, scrollY,
            scrollRangeX, scrollRangeY, maxOverScrollX, maxOverScrollY, isTouchEvent);
}
```
 理解deltaY 及isTouchEvent 参数的含义
## 动态改变头布局的高度
ParallaxListView 添加setParallaxImage()方法
```java
private ImageView headerImage;
public void setParallaxImage(ImageView imageView){
    headerImage = imageView;
}
```
MainActivity 调用ParallaxListView 的setParallaxImage()方法
```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    requestWindowFeature(Window.FEATURE_NO_TITLE);
    setContentView(R.layout.activity_main);
    plv = (ParallaxListView) findViewById(R.id.plv);
    View headerView = View.inflate(this,R.layout.layout_header, null);
    final ImageView headerImage = (ImageView) headerView.findViewById(R.id.iv_header);
    //等view 的树状结构渲染完毕时，再将headerImage 设置到plv 中
    headerImage.getViewTreeObserver().addOnGlobalLayoutListener(newOnGlobalLayoutListener() {

        @Override
        public void onGlobalLayout() {
            //宽高已经测量完毕
            plv.setParallaxImage(headerImage);
            //移除监听，避免下次渲染时还调用
            headerImage.getViewTreeObserver().removeGlobalOnLayoutListener(this);
        }
    });
    //添加头布局，需要在setAdapter 前添加
    plv.addHeaderView(headerView);
    plv.setAdapter(new ArrayAdapter<String>(this,android.R.layout.simple_list_item_1, Cheeses.NAMES));
}
```

第7-18 行view 树状结构渲染完毕时，再将头布局中ImageView 设置到ParallaxListView 中，这样在
ParallaxListView 的setParallaxImage()方法中才能获取到ImageView 的宽高
获取头部ImageView 的高度
```java
private int orignalHeight;
private int drawableHeight;

public void setParallaxImage(ImageView imageView){
    headerImage = imageView;
    //ImageView 初始高度
    orignalHeight = imageView.getHeight();
    //图片原始高度
    drawableHeight = imageView.getDrawable().getIntrinsicHeight();
}
```

顶部下拉时动态设置头部ImageView 的高度
```java
/**
 * 滑动到ListView 两端才会调用
 */
@Override
protected boolean overScrollBy(int deltaX, int deltaY, int scrollX,
                               int scrollY, int scrollRangeX, int scrollRangeY,
                               int maxOverScrollX, int maxOverScrollY, boolean isTouchEvent) {
    //deltaY 竖直方向滑动的瞬时变化量，顶部下拉为-，底部上拉为+
    //scrollY 两端滑动超出的距离，顶部为-，底部为+
    //scrollRangeY 竖立方向滑动的范围
    //maxOverScrollY 竖立方向最大的滑动位置
    //isTouchEvent 是否是用户触摸拉动，true 表示用户手指触摸拉动，false 表示惯性
    System.out.println("deltaY：" + deltaY + " scrollY:" + scrollY
            + " scrollRangeY:" + scrollRangeY + " maxOverScrollY:"
            + maxOverScrollY + " isTouchEvent:" + isTouchEvent);
    //顶部下拉，用户触摸时，将deltaY 累加给Header
    if(deltaY < 0 && isTouchEvent){
        int newHeight = headerImage.getHeight()+Math.abs(deltaY);
        //新高度小于图片原始高度才允许累加变化量
        if(newHeight <= drawableHeight){
            //让新的值生效
            headerImage.getLayoutParams().height = newHeight;
            headerImage.requestLayout();
        }
    }
    return super.overScrollBy(deltaX, deltaY, scrollX, scrollY,
            scrollRangeX, scrollRangeY, maxOverScrollX, maxOverScrollY, isTouchEvent);
}
```
第16-24 行动态设置头部ImageView 的高度
## 回弹动画
ParallaxListView 重写onTouchEvent()方法
```java
@Override
public boolean onTouchEvent(MotionEvent ev) {
    switch (ev.getAction()) {
        case MotionEvent.ACTION_UP:
            //松手时，把currentHeight 恢复到orignalHeight
            int currentHeight = headerImage.getHeight();
            //300->160 300,299,280,250,200,...160 随时间生成300 到160 间的值
            ValueAnimator animator = ValueAnimator.ofInt(currentHeight,orignalHeight);
            animator.setDuration(500);
            //动画更新的监听
            animator.addUpdateListener(new AnimatorUpdateListener() {
                @Override
                public void onAnimationUpdate(ValueAnimator animation) {
                    //获取随时间变化得到的currentHeight 到orignalHeight 间的值
                    int value = (Integer) animation.getAnimatedValue();
                    //让新的值生效
                    headerImage.getLayoutParams().height = value;
                    headerImage.requestLayout();
                }
            });
            //设置插值器实现回弹效果
            animator.setInterpolator(new OvershootInterpolator(2));
            animator.start();
            break;
        default:
            break;
    }
    return super.onTouchEvent(ev);
}
```
第8-23 行手指抬起时，用属性动画实现headerImage 恢复到初始高度

# 关于我

- Email：<815712739@qq.com>
- CSDN博客：[Allen Iverson](http://blog.csdn.net/axi295309066)
- 新浪微博：[AndroidDeveloper](http://weibo.com/u/1848214604?topnav=1&amp;wvr=6&amp;topsug=1&amp;is_all=1)

# License

    Copyright 2015 AllenIverson

    Copyright 2012 Jake Wharton
    Copyright 2011 Patrik Åkerfeldt
    Copyright 2011 Francisco Figueiredo Jr.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
