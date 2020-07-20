package com.dongqiudi.news.game;

import android.app.Activity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Toast;

import com.android.dongqiudi.library.DQDSDKManager;
import com.android.dongqiudi.library.callback.DqdLoginCallBack;
import com.android.dongqiudi.library.callback.DqdLogoutCallBack;
import com.android.dongqiudi.library.callback.DqdPayCallBack;
import com.android.dongqiudi.library.model.LoginResModel;
import com.android.dongqiudi.library.model.PayReqModel;
import com.android.dongqiudi.library.model.PayResModel;
import com.dongqiudi.news.R;

public class MainActivity extends Activity implements DqdLogoutCallBack {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);


        findViewById(R.id.btn_login).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 登录
                DQDSDKManager.getInstance().loginManager(MainActivity.this);
            }
        });
        Log.d("Main", "========================" + this);
        DQDSDKManager.getInstance().initFloatBall(MainActivity.this);
        DQDSDKManager.getInstance().showFloatBall(MainActivity.this);
        findViewById(R.id.btn_pay).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                PayReqModel model = new PayReqModel();
                /**
                 *  这里的配置信息修改为sdk调用者的配置
                 */
                model.appkey = "xxx";//在懂球帝配置的key
                model.order_no = "xxx";//游戏方自己的订单号
                model.price = "0.01";
                model.commodity_name = "商品-貂蝉皮肤";
                model.extra = "";
                DQDSDKManager.getInstance().payManager(MainActivity.this, model, new DqdPayCallBack() {

                    @Override
                    public void onSuccess(PayReqModel model) {
                        Log.i("pay", "支付成功");
                    }

                    @Override
                    public void onFail(PayResModel model) {
                        Log.i("pay", "支付成功");
                    }

                    @Override
                    public void onCancel() {
                        Log.i("pay", "支付成功");
                    }
                });

            }
        });


        DQDSDKManager.getInstance().setLoginCallback(callBack);
        DQDSDKManager.getInstance().setLogoutCallBack(this);
    }

    @Override
    public void onLogout() {
        //退出登录
        Log.i("logout", "退出登录");
    }

    private DqdLoginCallBack callBack = new DqdLoginCallBack() {
        @Override
        public void onSuccess(LoginResModel model) {
            Toast.makeText(MainActivity.this, "登录成功", Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onFail(int code) {
            Toast.makeText(MainActivity.this, "登录失败", Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onCancel() {
            Toast.makeText(MainActivity.this, "登录取消", Toast.LENGTH_SHORT).show();
        }
    };

    @Override
    public void finish() {
        super.finish();
    }
}
