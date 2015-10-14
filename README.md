# cordova-plugin-weixin
Wexin and wepay plugin import from http://wex5.com/

A cordova plugin, a JS version of WeChat Pay.

#Feature

Send WeChat payment request.

#Example

See [https://github.com/ZhichengChen/cordova-plugin-wechat-payment-example](cordova-pluign-wechat-payment-example)

#Install

1. `cordova plugin add https://github.com/ZhichengChen/cordova-plugin-weixin --variable activityname=YOUR_ACTIVITY_NAME`

2. `cordova build ios` or `cordova build android`

#Usage

##Send payment request

        var weixin = navigator.weixin;
        weixin.generatePrepayId({
                "body" : "x5外卖",
                "accessToken" : accessToken,
                "feeType" : "1",
                "notifyUrl" : notifyUrl,
                "totalFee" : "1",
                "traceId" : traceID,
                "tradeNo" : traceNo
            }, function(prepayId) {
                weixin.sendPayReq(prepayId, function(message) {
                    var responseCode = parseInt(message);
                    if (responseCode === 0) {
                        payDtd.resolve(1);
                    } else if (!isNaN(responseCode)) {
                        payDtd.reject((-13) + responseCode);
                    } else {
                        payDtd.reject(-10);
                    }
                }, function(message) {
                    justep.Util.hint("微信支付失败！");
                    payDtd.reject(-10);
                });
            }, function(message) {
                justep.Util.hint("微信支付失败！");
                payDtd.reject(-11);
            });

如果使用默认参数（使用WeX5 的账号测试），请确保应用的包名是com.justep.x5.takeout

上线篇

上面说的是用WeX5的默认配置进行开发测试，但是一旦自己的app开发好了要上线，我们肯定要用自己的商户信息来打包应用（当然你要用WeX5的默认商户配置，然后把钱都支付到WeX5的账户上我们也非常欢迎^_^）,这个时候就需要修改插件中的配置来让开发的app支付到自己公司的账上。

首先我们来修改weixin支付的参数 ，打开/Native/plugins/com.justep.cordova.plugin.weixin/plugin.xml

1. 修改ios部分

      <preference name="weixin_appid" value="wx832f85feb2e76b14" />
      <preference name="weixin_partner_id" value="1230177801" />
      <preference name="weixin_api_key" value="43cab6b2766381683f6cb1b4ee6db27a" />

以及

        <config-file target="*-Info.plist" parent="CFBundleURLTypes">
            <array>
                <dict>
                    <key>CFBundleTypeRole</key>
                    <string>Editor</string>
                    <key>CFBundleURLIconFile</key>
                    <string>icon-50@2x</string>
                    <key>CFBundleURLName</key>
                    <string>weixin</string>
                    <key>CFBundleURLSchemes</key>
                    <array>
                        <string>wx832f85feb2e76b14</string>
                    </array>
                </dict>
            </array>
        </config-file>

wx832f85feb2e76b14这个值也要改哦

2. android部分

    <preference name="weixin_appid" value="wx832f85feb2e76b14" />
    <preference name="weixin_partner_id" value="1230177801" />
    <preference name="weixin_api_key" value="43cab6b2766381683f6cb1b4ee6db27a" />

    <config-file target="AndroidManifest.xml" parent="/manifest/application/activity[@android:name='X5']">
        <intent-filter>
            <action android:name="android.intent.action.VIEW"/>
            <category android:name="android.intent.category.DEFAULT"/>
            <data android:scheme="wx832f85feb2e76b14"/>
        </intent-filter>
    </config-file>

wx832f85feb2e76b14这个值也要改哦

这几个参数的值为你自己从微信商户中得到的值。
