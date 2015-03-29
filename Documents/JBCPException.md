JBCPException クラスリファレンス
================================

### パッケージ
    java.lang.Object
     +-- java.lang.Throwable
          +-- java.lang.Exception
               +-- java.lang.RuntimeException
                    +-- com.beacapp.JBCPException


### 概要

エラー発生時の詳細情報を格納する例外クラスである。  
この例外クラスは RuntimeException を継承しているため、メソッドの throws 節でこの例外クラスを宣言する必要はない。  
getCode() では、本SDKで定義されたエラーコードを取得可能である。


### メソッド

    int getCode()
    String getMessage()


メソッド
--------

### getCode

`````````````
int getCode()
`````````````

例外発生時のエラーコードを返す。
Beacapp では以下のコードを使用する。
コード一覧表

|値    |define                            |意味                                  |
|------|----------------------------------|--------------------------------------|
|0     |SUCCESS                           |成功                                  |
|1000  |INVALID_ACTIVATION_KEY            |アクティベーションキーが不正          |
|1001  |NOT_INITIALIZE_YET                |SDK未初期化エラー                     |
|1002  |DB_ERROR                          |DB関連のエラー                        |
|1003  |NETWORK_ERROR                     |ネットワークラー                      |
|1004  |ACCESS_TOKEN_EXPIRED              |アクセストークンの有効期限切れ        |
|1005  |NOT_SUPPORTED_BLUTOOTH            |Bluetoothデバイスをサポートしていない |
|...   |...                               |...                                   |

この値は、このクラスのコンスタントとして定義されている。



#### getMessage

```````````````````
String getMessage()
```````````````````

エラー文言を返す。この文言は、ローカライズされた文字列である。
