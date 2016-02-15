JBCPManager クラスリファレンス
==============================

### パッケージ
    java.lang.Object
     +-- com.beacapp.JBCPManager


### 概要
このクラスはビーコンデバイス、アプリ情報などを管理するクラスである。  
主に以下の機能を提供する。

- SDKの初期化処理
- イベントデータ更新処理  
  ビーコンリスト、 イベント、トリガー、アクションの更新
- ビーコン受信処理開始／終了
- コールバックの登録
- デバイス固有の識別子取得


### メソッド

    static JBCPManager getManager(String requestToken, String secretKey, Map<String, Object> options)
    void startUpdateEvents()
    void startScan()
    void stopScan()
    String getDeviceIdentifier()
    void setFireEventListener(FireEventListener listener)
    void setUpdateEventsListener(UpdateEventsListener listener)
    void setShouldUpdateEventsListener(ShouldUpdateEventsListener listener)
    void setAdditonalLog(String logValue)
    void customLog(String logValue)



メソッド
--------

### getManager

````````````````````````````````````````````````````````````````````````````````````````````````````
static JBCPManager getManager(String requestToken, String secretKey, Map<String, Object> options)
````````````````````````````````````````````````````````````````````````````````````````````````````

JBCPManager のインスタンスを返す。  
SDKの未初期化の場合はSDK初期化もおこなう。  
未アクティベート時にはバックグラウンドでアクティベート処理を開始する。  
本関数は同期型であり、アクティベート処理が完了するまでブロックする。  
SDKを利用するアプリケーションは、本関数を使用しインスタンスを生成する必要がある。  
引数 options にはオプションパラメータを指定する。  

SDK初期化処理では、サーバから成りすまし防止の位置情報認証を利用するかどうかを取得する。  
位置情報を利用する場合はOSの位置情報取得処理を開始する。  

##### options パラメータ

キーに文字列、値は任意のオブジェクトを格納する。  
現状未使用。


##### 成りすまし防止の位置情報認証について

ビーコンデバイスの成りすまし防止に、OSの位置情報を利用した認証機能を利用できます。
ビーコンを検出した際に、現在地とBeacapp コンソール(仮)で登録した位置情報が大きくかけ離れている場合は不正と見なす機能である。
Beacapp コンソール(仮)で位置登録することで利用可能である。



##### 本関数で内部的に行う処理

- アクティベーション処理
- DB (sqlite3) 初期化
- iBeacon 初期化
- 位置情報初期化
- アクションデータ更新チェック



#### パラメータ
- requestToken
 Beacapp Web管理コンソールで登録したアプリケーションのリクエストトークンを指定する
- secretKey  
 Beacapp Web管理コンソールで登録したアプリケーションのシークレットキーを指定する
- options  
 オプションパラメータ。上記説明のとおり。

#### 戻り値
成功した場合は JBCPManager のインスタンスを返す。


#### 例外
- IllegalArgumentException  
 不正な引数の場合
- BeacappException  
 その他エラーが発生した場合





### startUpdateEvents

````````````````````````
void startUpdateEvents()
````````````````````````

イベントデータの更新を開始する。
イベントデータには、ビーコンリスト、 イベント、トリガー、アクションなど Beacapp を動作するさせるために必要な情報である。
更新処理の進捗と完了通知は setUpdateEventsListener でセットされた UpdateEventsListener へコールバックされる。  
また、SDK利用者は setShouldUpdateEventsListener を呼ぶことで、強制的に更新するかどうかを選択する事も可能である。

#### 例外
- BeacappException  
 エラーが発生した場合



### startScan

````````````````
void startScan()
````````````````

iBeacon デバイスのスキャンを開始する。スキャンはバックグラウンドで行われ、イベント発生などの通知は setFireEventListener で登録されたコールバッククラスへコールバックされる。

#### 例外
- BeacappException  
 エラーが発生した場合





### stopScan

```````````````
void stopScan()
```````````````

iBeacon デバイスのスキャンを停止する。

#### 例外
- BeacappException  
 エラーが発生した場合





### getDeviceIdentifier

````````````````````````````
String getDeviceIdentifier()
````````````````````````````

デバイス固有の識別子を取得する。  
識別子は初回SDK利用時に生成するユニークなIDとなる。  


#### 戻り値  
デバイス固有の識別子を返す。

#### 例外
- BeacappException  
 エラーが発生した場合





### setFireEventListener

`````````````````````````````````````````````````````
void setFireEventListener(FireEventListener listener)
`````````````````````````````````````````````````````

イベント発生時のコールバッククラスをセットする。


#### パラメータ
- listener  
 FireEventListener を実装したクラス

#### 例外
- BeacappException  
 エラーが発生した場合





### setUpdateEventsListener

````````````````````````````````````````````````````````
void setUpdateEventsListener(UpdateEventsListener listener)
````````````````````````````````````````````````````````

イベント情報更新のコールバッククラスをセットする。


#### パラメータ
- listener  
 UpdateEventsListener を実装したクラス

#### 例外
- BeacappException  
 エラーが発生した場合



### setShouldUpdateEventsListener

```````````````````````````````````````````````````````````````````````
void setShouldUpdateEventsListener(ShouldUpdateEventsListener listener)
```````````````````````````````````````````````````````````````````````

イベント情報の更新確認用のコールバック関数を定義する。  
startUpdateEvents() がコールされると、このコールバック関数にコールバックし、イベント情報を更新するかどうかをSDK利用者に問い合わせる。  
ShouldUpdateEventsListener クラスの shouldUpdateEvent() が true を返すとイベント情報を更新し、 false を返すと更新しない。


#### パラメータ
- listener  
 ShouldUpdateEventsListener を実装したクラス

#### 例外
- BeacappException  
 エラーが発生した場合



### setAdditonalLog

```````````````````````````````````````````````````````````````````````
void setAdditonalLog(String logValue)
```````````````````````````````````````````````````````````````````````

#### パラメータ
 - log  
  valueログのカスタム領域に追加する文字列を設定する。  
  エラーの場合はerror変数にNSErrorオブジェクトを格納する。
  valueには下記のチェックを行う。  
  - 文字列がSJISの範囲内であること。
  - 制御文字(<>!"#$%&'()-^~|[]{}`@*:;\_?)が使用されていないこと。  
  - utf-8で1000byte以内であること

#### 例外
  - BeacappException  
     エラーが発生した場合


### customLog


```````````````````````````````````````````````````````````````````````
void customLog(String logValue)
```````````````````````````````````````````````````````````````````````

 #### パラメータ
 - log  
  valueログのカスタム領域に追加する文字列を設定しログ出力を行う。  
  ログのtypeは256で出力する。  
  valueには下記のチェックを行う。
  - 文字列がSJISの範囲内であること。  
  - 制御文字(<>!"#$%&'()-^~|[]{}`@*:;\_?)が使用されていないこと。  
  - 前回customLogが呼び出されてから1秒以上経過していること。
  - utf-8で1000byte以内であること

#### 例外
  - BeacappException  
    エラーが発生した場合
