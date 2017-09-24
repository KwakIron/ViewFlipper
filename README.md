# ViewFlipper
```
public class MainActivity extends AppCompatActivity {
    private ViewFlipper viewFlipper;

    private Context mContext;
    private ViewFlipper vflp_help;
    private int[] resId = {R.mipmap.ic_help_view_1,R.mipmap.ic_help_view_2,
            R.mipmap.ic_help_view_3,R.mipmap.ic_help_view_4};

    private final static int MIN_MOVE = 200;   //最小距离
    private MyGestureListener mgListener;
    private GestureDetector mDetector;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        viewFlipper = (ViewFlipper)findViewById(R.id.viewFlipper);
        viewFlipper.startFlipping();
////////////////////////////////////////////////////////

        mContext = MainActivity.this;
        //实例化SimpleOnGestureListener与GestureDetector对象
        mgListener = new MyGestureListener();
        mDetector = new GestureDetector(mContext, mgListener);
        vflp_help = (ViewFlipper) findViewById(R.id.vflp_help);
        //动态导入添加子View
        for(int i = 0;i < resId.length;i++){
            vflp_help.addView(getImageView(resId[i]));
        }

    }

    //重写onTouchEvent触发MyGestureListener里的方法
    @Override
    public boolean onTouchEvent(MotionEvent event) {

        return mDetector.onTouchEvent(event);
    }


    //自定义一个GestureListener,这个是View类下的，别写错哦！！！
    public class MyGestureListener extends GestureDetector.SimpleOnGestureListener {
        @Override
        public boolean onFling(MotionEvent e1, MotionEvent e2, float v, float v1) {
            if(e1.getX() - e2.getX() > MIN_MOVE){
                vflp_help.setInAnimation(mContext,R.anim.right_in_one);
                vflp_help.setOutAnimation(mContext, R.anim.right_out_one);
                vflp_help.showNext();
            }else if(e2.getX() - e1.getX() > MIN_MOVE){
                vflp_help.setInAnimation(mContext,R.anim.left_in_one);
                vflp_help.setOutAnimation(mContext, R.anim.left_out_one);
                vflp_help.showPrevious();
            }
            return true;
        }
    }

    private ImageView getImageView(int resId){
        ImageView img = new ImageView(this);
        img.setBackgroundResource(resId);
        return img;
    }
}
