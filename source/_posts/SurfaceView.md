---
title: SurfaceView
comments: true
date: 2019-11-05 12:42:57
tags:
categories: VIEW
---





>  我们知道View是通过刷新来重绘视图，系统通过发出`VSSYNC`信号来进行屏幕的重绘，刷新的时间间隔是`16ms`,如果我们可以在16ms以内将绘制工作完成，则没有任何问题，如果我们绘制过程逻辑很复杂，并且我们的界面更新还非常频繁，这时候就会造成界面的卡顿，影响用户体验，为此Android提供了`SurfaceView`来解决这一问题。



如果View需要频繁的刷新，或者刷新的数据量比较大，就需要使用SurfaceView

#### 优势

1. View中刷新频繁，Ondraw()会频繁调用,onDraw方法执行的时间过程会掉帧，出现页面卡顿。SurfaceView使用**双缓冲技术**，提高了绘制速度，可以缓解这一现象。

2. SurfaceView可以在子线程更新UI,不会阻塞主线程，提高了响应速度.

   https://www.jianshu.com/p/b037249e6d31

#### 啥是双缓冲技术?

https://blog.csdn.net/guanguanboy/article/details/99715643

Android群英传 6.8



#### SurfaceView使用模板

大部分绘图都按照模板来

SurfaceViewTemplate

```java
public class SurfaceViewTemplate extends SurfaceView implements SurfaceHolder.Callback, Runnable {
    private SurfaceHolder mSurfaceHolder;
    //绘图的Canvas
    private Canvas mCanvas;
    //子线程标志位
    private boolean mIsDrawing;
    public SurfaceViewTemplate(Context context) {
        this(context, null);
    }

    public SurfaceViewTemplate(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public SurfaceViewTemplate(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initView();
    }

    @Override
    public void surfaceCreated(SurfaceHolder holder) {
        mIsDrawing = true;
        //开启子线程
        new Thread(this).start();
    }

    @Override
    public void surfaceChanged(SurfaceHolder holder, int format, int width, int height) {

    }

    @Override
    public void surfaceDestroyed(SurfaceHolder holder) {
        mIsDrawing = false;
    }

    @Override
    public void run() {
        while (mIsDrawing){
            draw();
        }
    }
    //绘图逻辑
    private void draw() {
        try {
            //获得当前Canva绘图对象，获取的还是继续上次的Canvas对象，而不是一个新的对象,因此之前的绘图都将被保留，如果需要擦除，则绘制前，通过drawColor()方法进行清屏操作.
            mCanvas = mSurfaceHolder.lockCanvas();
            //绘制背景
            mCanvas.drawColor(Color.WHITE);
            //绘图
        }catch (Exception e){

        }finally {
            if (mCanvas != null){
                //释放canvas对象并提交画布
                mSurfaceHolder.unlockCanvasAndPost(mCanvas);
            }
        }
    }

    /**
     * 初始化View
     */
    private void initView(){
        mSurfaceHolder = getHolder();
        mSurfaceHolder.addCallback(this);
        setFocusable(true);
        setKeepScreenOn(true);
        setFocusableInTouchMode(true);
    }
}
```

正旋曲线

```java
/**
 *  https://www.jianshu.com/p/b037249e6d31
 */
public class SurfaceViewSinFun extends SurfaceView implements SurfaceHolder.Callback, Runnable {
    private SurfaceHolder mSurfaceHolder;
    //绘图的Canvas
    private Canvas mCanvas;
    //子线程标志位
    private boolean mIsDrawing;
    private int x = 0, y = 0;
    private Paint mPaint;
    private Path mPath;
    public SurfaceViewSinFun(Context context) {
        this(context, null);
    }

    public SurfaceViewSinFun(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public SurfaceViewSinFun(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        mPaint = new Paint();
        mPaint.setColor(Color.BLACK);
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setAntiAlias(true);
        mPaint.setStrokeWidth(5);
        mPath = new Path();
        //路径起始点(0, 100)
        mPath.moveTo(0, 100);
        initView();
    }

    @Override
    public void surfaceCreated(SurfaceHolder holder) {
        mIsDrawing = true;
        new Thread(this).start();
    }

    @Override
    public void surfaceChanged(SurfaceHolder holder, int format, int width, int height) {

    }

    @Override
    public void surfaceDestroyed(SurfaceHolder holder) {
        mIsDrawing = false;
    }

    @Override
    public void run() {
        long start = System.currentTimeMillis();
        while (mIsDrawing){
            drawSomething();
            x += 1;
            y = (int)(100 * Math.sin(2 * x * Math.PI / 180) + 400);
            //加入新的坐标点
            mPath.lineTo(x, y);
        }

        //控制绘制频率
        long end  = System.currentTimeMillis();
        //50 - 100
        if (end - start<100){
            try {
                Thread.sleep(100-(end-start));  //相当于绘制一次总的时间 100ms
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }


    private void drawSomething() {
        try {
            //获得canvas对象
            mCanvas = mSurfaceHolder.lockCanvas();
            //绘制背景
            mCanvas.drawColor(Color.WHITE);
            //绘制路径
            mCanvas.drawPath(mPath, mPaint);
        }catch (Exception e){

        }finally {
            if (mCanvas != null){
                //释放canvas对象并提交画布
                mSurfaceHolder.unlockCanvasAndPost(mCanvas);
            }
        }
    }

    /**
     * 初始化View
     */
    private void initView(){
        mSurfaceHolder = getHolder();
        mSurfaceHolder.addCallback(this);
        setFocusable(true);
        setKeepScreenOn(true);
        setFocusableInTouchMode(true);
    }
}
```

通过draw()方法所使用的逻辑时长来确定sleep的时长，这是一个非常通用的解决方案，每次绘制时间控制在100ms

画板

```java
/**
 * 画板
 */
class DrawingBoardView : SurfaceView, SurfaceHolder.Callback, Runnable {
    private var mCanvas: Canvas? = null
    private var mSurfaceHolder: SurfaceHolder? = holder

    //子线程标志位
    private var mIsDrawing = false
    private val x = 0
    private var y: Int = 0
    private var mPaint: Paint? = Paint().apply {
        color = Color.BLACK
        style = Paint.Style.STROKE
        isAntiAlias = true
        strokeWidth = 5f
    }
    private var mPath: Path? = null

    constructor(context: Context) : this(context, null)
    constructor(context: Context, attrs: AttributeSet?) : this(context, attrs, 0)
    constructor(context: Context, attrs: AttributeSet?, defStyleAttr: Int) : super(context, attrs, defStyleAttr) {

    }

    init {
        mSurfaceHolder?.addCallback(this)
        isFocusable = true
        keepScreenOn = true
        isFocusableInTouchMode = true

        mPath = Path()
    }

    override fun surfaceChanged(holder: SurfaceHolder?, format: Int, width: Int, height: Int) {

    }

    override fun surfaceDestroyed(holder: SurfaceHolder?) {
    }

    override fun surfaceCreated(holder: SurfaceHolder?) {
        mIsDrawing = true
        //开启子线程
        Thread(this).start()
    }

    override fun run() {
        while (mIsDrawing) {
            draw()
        }
    }

    private fun draw() {
        try {
            mCanvas = mSurfaceHolder?.lockCanvas()
            //SurfaceView背景
            mCanvas?.drawColor(Color.WHITE)
            mPaint?.let { mPath?.let { it1 -> mCanvas?.drawPath(it1, it) } }
        } catch (e: Exception) {

        } finally {
            mCanvas?.let {
                mSurfaceHolder?.unlockCanvasAndPost(it)
            }
        }

    }

    override fun onTouchEvent(event: MotionEvent?): Boolean {
        val x = event?.x
        val y = event?.y
        when (event?.action) {
            MotionEvent.ACTION_DOWN -> x?.let {
                if (y != null) {
                    mPath?.moveTo(it, y)
                }
            }

            MotionEvent.ACTION_MOVE -> x?.let {
                if (y != null) {
                    mPath?.lineTo(it, y)
                }
            }
        }
        return true
    }
}
```

