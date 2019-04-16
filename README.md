# Intent

![Image text](https://github.com/chenzifeng123/image/blob/master/5_001.PNG)
![Image text](https://github.com/chenzifeng123/image/blob/master/5_002.PNG)





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
