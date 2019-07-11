---
title: CustomView02
comments: true
date: 2017-11-12 12:34:50
tags: VIEW
categories: VIEW
---

#### Path

* lineTo简单实现

```
 		canvas.translate(DeviceUtil.mWidth / 2, DeviceUtil.mHeight / 2); //原点移动到屏幕中心
        Path path = new Path();
        path.lineTo(200, 200);
        path.lineTo(200, 0);
        canvas.drawPath(path, mPaint);
```

![customview](CustomView02/custoview_02.jpg)

从字面意思 lineto,顾名思义肯定就有from的坐标点，再看这个借的图，第一条线是从原点开始的，第二条线是从A开始的，

```
path.close();  //形成封闭的图形
```



绘制心电图

<https://github.com/SeekerFighter/LuckyEcgDemo>

<https://www.jianshu.com/p/16301de41a18>



##### 表单输入

![CustomView2018-05-12 09-42-16](E:/noteforme.github.io/source/_posts/CustomView/CustomView2018-05-12%2009-42-16.png)



```
/**
 * EditText文字固定在右边
 */
public class EditTextRight extends AppCompatEditText {
    private String txtRight;
    private Paint mPaint;

    public EditTextRight(Context context) {
        super(context);
    }

    public EditTextRight(Context context, AttributeSet attrs) {
        super(context, attrs);

        initAttrs(context, attrs);

    }

    private void initAttrs(Context context, AttributeSet attrs) {
        TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.EditTextRight);
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        txtRight = ta.getString(R.styleable.EditTextRight_textright);
//        Timber.d("text" + txtRight);
        ta.recycle();
    }


    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        float txtSize = getTextSize();
        if (!TextUtils.isEmpty(txtRight)) {
            float yrig = getWidth() - txtSize * txtRight.length() - 10; //getWidth() 控件宽度
            Timber.d("txt " + txtRight + "   getRight " + yrig);
            canvas.drawText(txtRight, yrig, getBaseline(), getPaint());
        }
    }
}
```

```
  <com.jonzhou.mineutils.ui.customview.EditTextRight
        android:layout_width="100dp"
        android:layout_height="wrap_content"
        mineutils:textright="幢" />
```

