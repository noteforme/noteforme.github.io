---
title: CustomView
comments: true
date: 2017-11-12 22:18:58
tags:
categories: ANDROID
---
绘图
http://blog.csdn.net/huaiyiheyuan/article/details/52205969

http://blog.csdn.net/yissan/article/details/51136088

画进度条　
https://github.com/zhouzhuo810/ZzHorizontalProgressBar

画圆
http://www.jianshu.com/p/d891fe636898

自定义控件　最先执行构造方法


自定义view:
http://www.gcssloop.com/customview/CustomViewIndex/

自定义TextView

```

/**
 * Created by jon on 3/3/18.
 * 自定义TextView  ,均份LinearLayout
 */

public class LinearTextView extends View {
    private final Paint mPaint;
    private final int xwidth;

    public LinearTextView(Context context) {
        this(context, null);
    }

    public LinearTextView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setTextSize(40f);
        xwidth = DisplayUtils.getScreenW((Activity) context);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        mPaint.setColor(Color.BLACK);
        String txttext = "你好";
        float stringWidth = mPaint.measureText(txttext);
        int tWidth = xwidth / 12;

        int k = 12;
        for (int i = 1; i < k; i++) {
            if (i % 2 == 0) {
                continue;
            }
            canvas.drawText(txttext, xwidth * i / k - stringWidth / 2, 50, mPaint);
        }
    }

}
```

