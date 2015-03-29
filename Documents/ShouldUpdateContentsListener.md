ShouldUpdateContentsListener インターフェースリファレンス
=========================================================


### パッケージ
    java.lang.Object
     +-- com.beacapp.ShouldUpdateContentsListener


### 概要

コンテンツ更新時、コンテンツデータを更新するかどうかを問い合わせるためのインタフェースである。



### メソッド

    boolean shouldUpdate(Map<String, Object> info)


メソッド
--------

### shouldUpdate

```````````````````````````````````````````
void shouldUpdate(Map<String, Object> info)
```````````````````````````````````````````


コンテンツ更新するかどうかをSDK利用者に問い合わせるためのコールバック。  
JBCPManager.startUpdateContents がコールされると本関数がコールバックされる。  
利用者は戻り値 true or false を返却することでコンテンツを更新するかどうかを選択可能とする。  
また、このインタフェースを定義しない場合はデフォルトの動作を行う。  
引数 info には、コンテンツ更新するかどうかの情報が辞書形式で格納される。  

##### info パラメータ
キーに文字列、値は任意のオブジェクトを格納する。  

|キー               |値の型   |値の説明                                      |
|-------------------|---------|----------------------------------------------|
|lastUpdatedAt      |Date     |最後に更新した日時                            |
|alreadyNewest      |Boolean  |true:既に最新版  false:サーバに新コンテンツあり |


##### デフォルトの動作

info の alreadyNewest が true の場合は更新をおこなわない。 false の場合のみ更新をおこなう。



#### パラメータ
- info  
 コンテンツ更新するかどうかの情報が格納される


#### 戻り値
false の場合は更新しない。  
true の場合はコンテンツの更新をおこなう。
info の alreadyNewest が true の場合でも true を返却することで強制的にコンテンツ更新を行うことができる。
