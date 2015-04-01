# Beacapp SDK for Android Version 1.0.0
## はじめに
[Beacapp](http://www.beacapp.com)で登録したコンテンツをAndroidで利用するためのSDKです。

## 概要
BeacappSDKforAndroidは、AndroidアプリケーションにBeacappの機能を組み込むためのSDK（ソフトウェア開発キット）です。
既存のAndroidアプリケーション、これから製造するAndroidアプリケーションにBeacappSDKforAndroidを組み込み、必要な処理を実行するだけで ビーコンを検知して様々なアクションを実行することができます。

BeacappSDKforAndroidの主な機能は以下の通りです。

- BeacappSDKのアクティベーション
	BeacappSDKforAndroidを実行するためにはアプリ起動毎にアクティベーションが必要となります。アクティベーションとは、Beacapp Web管理コンソール（以下、CMS)にアクセスし、BeacappSDKforAndroidが組み込まれているアプリケーションがCMSで登録されているアプリケーションかを判定します。CMSで登録されているアプリケーションではない、もしくはCMSで発行されている認証情報がBeacappSDKに正しく設定されていない場合はエラーと判断されます。また、アクティベーション時に端末の電波状態が著しく悪い場合、機内モード・圏外などの場合もエラーとなる場合があります。

- CMSで設定された各種情報のダウンロード
	CMSで設定された各種情報（ビーコンの情報、イベントの発火条件の情報など）をCMSからダウンロードします。この情報のダウンロードは後のビーコンの検知、イベントの発火に必要となります。

- ビーコンの検知・検知停止
	ビーコンの検知・検知停止を実行します。BeacappSDKforAndroidでは、ビーコンの検知を行うために位置情報サービスの利用とBluetoothの状態確認を行います。

- イベントのハンドリングと発火
	BeacappSDKforAndroidではビーコンを検知した際に、CMSで設定されたイベントの条件と合致するかを自動で判定します。イベントの条件と合致する場合は、BeacappSDKforAndroidを組み込んでいるアプリケーションに通知を行います。


## 実行環境
* Android Studio 1.0 and later
* Android4.3 and later
* Bluetooth Low Energy 対応機種

## SDKの導入
1.「libBeacappSDKforAndroid.jar」をダウンロードし、プロジェクトにインポートします。

2.[AWS SDK for Android](http://aws.amazon.com/jp/mobile/sdk/)をダウンロードし、プロジェクトにインポートします。（※1）  
  - 必要なjarファイルは以下の4つです。    
      - aws-android-sdk-ddb-mapper    
      - aws-android-sdk-ddb    
      - aws-android-sdk-core    
      - aws-android-sdk-kinesis    
	 [Github](https://github.com/aws/aws-sdk-android)  

（※1）
Android Studioをご利用の方はGradleに以下の記載をするだけでもインポート可能です。

	buildscript {
		repositories {
         		jcenter()
     		}
     		dependencies {
         		classpath 'com.android.tools.build:gradle:1.1.0'
         		classpath 'com.amazonaws:aws-android-sdk-core:2.1.+'
         		classpath 'com.amazonaws:aws-android-sdk-ddb:2.1.+'
         		classpath 'com.amazonaws:aws-android-sdk-ddb-mapper:2.1.+'
         		classpath 'com.amazonaws:aws-android-sdk-kinesis:2.1.+'
         	}
         }



## SDKの使い方
・AndroidManifest  
  - 必要な権限を追加します  
      - インターネットアクセス  
      - Bluetoothアクセス  
      - GPSアクセス  
  - サービスの登録をします  
```
	<service
		android:name="com.beacapp.service.JBCPScanService" android:process=":bleservice" android:exported="false" >
		<intent-filter>
			<action android:name="com.beacapp.BLEServiceAIDL" > </action>
			<action android:name="com.beacapp.BLEServiceCalbackAIDL" > </action>
		</intent-filter>
	</service>
```

・Activity  
  - 必要なライブラリをインポートします
　
```
	import com.beacapp.FireEventListener;
	import com.beacapp.JBCPException;
	import com.beacapp.JBCPManager;
	import com.beacapp.ShouldUpdateEventsListener;
	import com.beacapp.UpdateEventsListener;
```

  - マネージャーを初期化してリスナーの登録をします
　
```	
	try {
		jbcpManager = JBCPManager.getManager(this, requestToken, secretKey, null);
	} catch (JBCPException e) {
		Log.d("", e.getMessage());
	}

	jbcpManager.setUpdateEventsListener(updateEventsListener);
	jbcpManager.setShouldUpdateEventsListener(shouldUpdateEventsListener);
	jbcpManager.setFireEventListener(fireEventListener);
```	
  - スキャン開始
　
```　
	jbcpManager.startScan();
```	
  - スキャン終了 
　
```
	jbcpManager.stopScan();
```	

## 補足

1. サンプルコード
	BeacappSDKforAndroidの一般的な処理の流れとしてのサンプルコードです。  
AndroidManifest.xml

```
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="jmas.co.jp.beacappandroidtest" >

    <!-- GPSを使用するために必要なパーミッション -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.BLUETOOTH"/>
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />

    <uses-feature android:name="android.hardware.bruetooth_le" android:required="true"/>

    <uses-sdk
        android:minSdkVersion="18"
        android:targetSdkVersion="21" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="jmas.co.jp.beacappandroidtest.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <service
            android:name="com.beacapp.service.JBCPScanService" android:process=":bleservice" android:exported="false" >
            <intent-filter>
                <action android:name="com.beacapp.BLEServiceAIDL" > </action>
                <action android:name="com.beacapp.BLEServiceCalbackAIDL" > </action>
            </intent-filter>
        </service>
    </application>

</manifest>
```

xxxActivity.java

イベント取得処理

	public FireEventListener fireEventListener = new FireEventListener() {
		@Override
		public void fireEvent(JSONObject event) {
			setEventText(event);

			JSONObject action_data = event.optJSONObject("action_data");
			String action = action_data.optString("action");


			// URLの場合
			if(action.equals("jbcp_open_url"))
			{
			    eventUrl = action_data.optString("url");

			}
			//画像の場合
			else if(action.equals("jbcp_open_image"))
			{
			    eventUrl = action_data.optString("url");

			}
			//カスタムの場合
			else if(action.equals("jbcp_custom_key_value"))
			{

			    eventText = action_data.optString("key_values");

			}
			//テキストの場合
			else if(action.equals("jbcp_open_text"))
			{
			    eventText = action_data.optString("text");

            }
        }
    };


## ドキュメント
各種APIのドキュメントなどは[こちら](Documents/android-api-reference.md)です。

## 製作者

- jena co. ltd.
- JMAS Systems Corp,.

## ライセンス

BeacappSDKforAndroidを利用するためには、Beacappの利用規約に同意する必要があります。
BeacappSDKforAndroidをいかなる方法を以ってダウンロードした場合、Beacappの利用規約に同意しているものとみなします。
Beacappのご利用申し込みおよび利用規約の同意は[こちら](https://cms.beacapp.com/signup/index/)から可能です。

## その他

- BeacappSDKforAndroidでは、[Version2 of AWS SDK for Android](https://github.com/aws/aws-sdk-android)が必要となります。

    Version2 of AWS SDK for Androidは　Apache 2.0ライセンスとなります。
	Version2 of AWS SDK for Android
    Copyright 2014 Amazon.com, Inc. or its affiliates. All Rights Reserved.
    [https://github.com/aws/aws-sdk-android/blob/master/LICENSE.txt](https://github.com/aws/aws-sdk-android/blob/master/LICENSE.txt)


