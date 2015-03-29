UpdateContentsListener インターフェースリファレンス
=================================================


### パッケージ
    java.lang.Object
     +-- com.beacapp.UpdateContentsListener


### 概要

コンテンツ更新処理の進捗、完了を通知するためのインタフェースである。



### メソッド

    void onProgress(int current, int total)
    void onFinished(JBCPException error)


メソッド
--------

### onProgress

```````````````````````````````````````
void onProgress(int current, int total)
```````````````````````````````````````


コンテンツ更新の進捗コールバック。
このコールバックは JBCPManager の startUpdateContents が呼ばれるとコールされる。
プログレスバーの表示などをおこないたい場合に本関数を使用する。


#### パラメータ
- current  
 現在の進捗値。%で表す場合は ```100 * current / total``` で計算できる。
- total  
 進捗値の最大値





### onFinished

````````````````````````````````````
void onFinished(JBCPException error)
````````````````````````````````````


コンテンツ更新が完了すると、このコールバック関数がコールされる。
エラー発生時にもコールされ、その場合は error に詳細情報が格納される。


#### パラメータ
- error  
 エラーが発生した場合、詳細情報の JBCPException オブジェクトを格納する。成功した場合は null が格納される。
