# Intent

![Image text](https://github.com/chenzifeng123/image/blob/master/5_001.PNG)
![Image text](https://github.com/chenzifeng123/image/blob/master/5_002.PNG)



主要代码：

    public class MainActivity extends AppCompatActivity implements View.OnClickListener{
      private static final String TAG = "MainActivity";
      private Button btn_go;
      private EditText et_url;
      private String urlHead="https://";
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          initView();
      }
      private void initView() {
          //绑定
          btn_go= findViewById(R.id.btn_go);
          et_url= findViewById(R.id.et_url);
          btn_go.setOnClickListener(this);
      }
      @Override
      public void onClick(View view) {
          switch (view.getId()){
              case R.id.btn_go:
                  Intent intent=new Intent(Intent.ACTION_VIEW);
                  intent.setData(Uri.parse(urlHead+et_url.getText().toString()));
                  Intent choose=Intent.createChooser(intent,"选择一个浏览器");
                  startActivity(choose);
                  break;
          }
      } 
     }


    public class MyWebView extends AppCompatActivity {

      private WebView webView;
      private String url;
      @Override
      protected void onCreate(@Nullable Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_my_web_view);
          initView();
          setWebView();
      }
      private void initView() {
          webView=(WebView)findViewById(R.id.web_View);
          url=getIntent().getData().toString();
      }
      private void setWebView() {
          WebSettings webSettings=webView.getSettings();
          webSettings.setJavaScriptEnabled(true);
          webSettings.setSupportZoom(true);
          webView.loadUrl(url);
          webView.setWebViewClient(new WebViewClient(){
              @Override
              public boolean shouldOverrideUrlLoading(WebView view,String url) {
                  webView.loadUrl(url);
                  return true;
              }
          });
       }
     }
     
  AndroidManifest.xm文件：
  
      <activity android:name=".MyWebView"
                android:usesCleartextTraffic="true"
                >
                <intent-filter>
                    <!-- 和系统打开网页的Action一致 -->
                   <action android:name="android.intent.action.VIEW"/>
                    <category android:name="android.intent.category.DEFAULT"/>
                    <data android:scheme="https"/>
                </intent-filter>
            </activity>
    
  在运行时曾出现 net::ERR_CLEARTEXT_NOT_PERMITTED无法加载url的情况  
  原因：为保证用户数据和设备的安全，Google针对 Android  P版本以后的应用程序，将要求默认使用加密连接，这意味着 Android P 将禁止 App 使用所有未加密的连接，因此运行 Android P 系统的安卓设备无论是接收或者发送流量，都不能进行明码传输，需要使用新一代传输层安全协议(Transport Layer Security)，而 Android P以下版本不受影响。  
  解决方法：  
   1）使用Https请求替换Http请求。既然官方都说了用Https更安全，那就替换成Https。  
   2）将targetSdkVersion降到27以下。曲线救国，既然从28开始不支持Http，那么我就用28以下的版本。  
   3）更改网络安全配置  
 参考博客链接：https://blog.csdn.net/Ever69/article/details/88085874

