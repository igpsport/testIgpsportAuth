# iGPSPORT Connect

> ���������app����iGPSPORT�˻����ϴ�fit�ļ���iGPSPORT��վ


������� [demo](http://my.igpsport.com/staticfile/testIgpsportAuth.apk)

### �����û���Ȩ

����ȨActivity�з�ֹWebView�ؼ���������url: http://my.igpsport.com/webapi/WebLoginAuth ����Я��appid����ΪiGPSPORT�����������app��appid

### ��ȡ�û���Ϣ�Լ�Token

ΪWebView���JavaScript�ӿ����ڽ��ܷ��ص���Ȩ��Ϣ��

```java
class JsInvoker {

    @JavascriptInterface
	public void getLoginResult(String result) {

		//Toast.makeText(getApplicationContext(), result, Toast.LENGTH_SHORT).show();
		Intent intent = new Intent();
		intent.putExtra("data", result);
		setResult(1000, intent);
		finish();
	}

}
```
����nameΪapp
```java
 wvAuth.addJavascriptInterface(new JsInvoker(), "app");

```
��getLoginResult�����У�result����Ϊjson��ʽ���ݣ������û�ID���ǳƣ��Ա�������غ�Token��Token����ʱ�����Ϣ��

### �ϴ�Fit�ļ�
�ϴ��ӿ�Ϊ��http://my.igpsport.com/Partner/UplodFit ���˽ӿ�Ҫ�������ĸ�������

```

* file ���ļ�

* memberid�� �û�ID

* appid�� APPID

* token���û���Ȩtoken
```

�ϴ�����᷵��int��ֵ�������ʾ����·��id����ʱ��Ϊ�ϴ��ɹ�����ֵΪ������롣

```
 -10001 ��Ա����Ϊ��
 -10002 AppID����Ϊ��
 -10003 Token����Ϊ��
 -10004 AppId��Ч
 -10005 �û���Ȩ��Ϣ���󣨲����ڻ��߹��ڣ�
 -20001 file����Ϊ��
 -20002 �ļ�̫��
 -20003 �ļ����Ͳ�֧��
```
�ο��������£�
```java
File myFile = new File(filename);

RequestParams params = new RequestParams();
try {
    params.put("file", myFile);
} catch (Exception ex) {
	Toast.makeText(getApplicationContext(), "�޷��ϴ����ļ�", Toast.LENGTH_SHORT).show();
	return;
}
params.add("memberid", mUser.MemberID + "");
params.add("appid", Constants.APPID + "");
params.add("token", mUser.Token + "");

final ProgressDialog progressDialog = ProgressDialog.show(getApplicationContext(), "", "�ϴ���...");
AsyncHttpClient myClient = new AsyncHttpClient();
myClient.post(getApplicationContext(), Constants.API_UPLOAD_FIT, params, new AsyncHttpResponseHandler() {
	@Override
	public void onSuccess(int statusCode, Header[] headers, byte[] responseBody) {
		progressDialog.dismiss();
		String result = new String(responseBody);
		Toast.makeText(getApplicationContext(), "�ϴ��ɹ��������" + result, Toast.LENGTH_SHORT).show();
	}

	@Override
	public void onFailure(int statusCode, Header[] headers, byte[] responseBody, Throwable error) {
		progressDialog.dismiss();
		Toast.makeText(getApplicationContext(), "�ϴ�ʧ��", Toast.LENGTH_SHORT).show();
	}
});
```


