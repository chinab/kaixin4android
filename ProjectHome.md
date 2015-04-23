# Kaixin4Android #
Kaixin\_android\_sdk是一个开源的JAVA库，通过这个库，你可以登录到开心网，并调用开心网开放出来的rest apis。

## 最新版本下载地址 ##

http://code.google.com/p/kaixin4android/downloads/list

## API文档 ##

开发者可以参考开心网开放平台提供的api文档

http://wiki.open.kaixin001.com/index.php?id=API%E6%96%87%E6%A1%A3

## 意见与反馈 ##

如果对sdk有更好的意见或是想法以及bug提交都可以反馈到这里

http://www.kaixin001.com/home/?uid=100038315

http://www.kaixin001.com/group/group.php?gid=1060588

## 简单的示例 ##
在使用Kaixin\_android\_sdk之前，先要在开心网上创建一个新组件，并把/kaixin\_android\_sdk/src/com/kaixin/connect/Kaixin.java中的CONSUMER\_KEY和CONSUMER\_SECRET替换为你申请组件的API Key和Secret Key。
### oauth认证： ###
```
Kaixin kaixin = Kaixin.getInstance();
try {
	// 获取未授权的Request Token
	if (kaixin.getRequestToken(AndroidExample.this,
			"kxapidemo://apidemos")) {
		Uri uri = Uri.parse(Kaixin.KX_AUTHORIZE_URL
				+ "?oauth_token=" + kaixin.getRequestToken()
				+ "&oauth_client=1");
		startActivity(new Intent(Intent.ACTION_VIEW, uri));
	}
} catch (Exception e) {
	Log.e("AndroidExample", e.toString());
}
```
授权完成后跳转到kxapidemo工程的apidemos界面：
```
Kaixin kaixin = Kaixin.getInstance();
Uri uri = getIntent().getData();
if (uri != null) {
	Bundle bund = Util.decodeUrl(uri.toString());
	String verifier = (String) bund.get("oauth_verifier");
	try {
		if (verifier != null) {
			// 获取Access Token
			if (kaixin.getAccessToken(this, kaixin.getRequestToken(),
					kaixin.getRequestTokenSecret(), verifier)) {
				
				// 存储Access Token
				kaixin.updateStorage(this);
			}
		}

	} catch (Exception e) {
		Log.e("apidemos", e.toString());
	}
}
```

### 取出我的用户信息： ###
```
Kaixin kaixin = Kaixin.getInstance();
String jsonResult = kaixin.request(context, "/users/me.json", null,	"GET");
Log.i("GetUserInfoTask", jsonResult );
```

### 取出当前用户的好友信息： ###
```
Kaixin kaixin = Kaixin.getInstance();
Bundle bundle = new Bundle();
bundle.putString("start", "0");
bundle.putString("num", "20");
String jsonResult = kaixin.request(context, "/friends/me.json", bundle, "GET");
Log.i("GetFriendListTask", jsonResult);
```

### 写记录： ###
```
Bundle bundle = new Bundle();
bundle.putString("content", content);
Map<String, Object> photoes = new HashMap<String, Object>();
photoes.put("filename", in);
String jsonResult = kaixin.uploadContent(context,"/records/add.json", bundle, photoes);
Log.i("PostRecordTask", jsonResult);
```
