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

## SDKの導入
整備中です。

## SDKの使い方
整備中です。

## 補足

1. サンプルコード
	BeacappSDKforAndroidの一般的な処理の流れとしてのサンプルコードです。
	整備中です。



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

    Version2 of AWS SDK for iOSは　Apache 2.0ライセンスとなります。
	Version2 of AWS SDK for iOS
    Copyright 2014 Amazon.com, Inc. or its affiliates. All Rights Reserved.
    [https://github.com/aws/aws-sdk-android/blob/master/LICENSE.txt](https://github.com/aws/aws-sdk-android/blob/master/LICENSE.txt)


