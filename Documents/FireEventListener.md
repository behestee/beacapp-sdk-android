FireEventListener インターフェースリファレンス
==============================================


### パッケージ
    java.lang.Object
     +-- com.beacapp.FireEventListener


### 概要

ビーコンを受信し、条件に合致したイベントを受信するためのインタフェースである。  
イベントが発生すると fireEvent() で定義された関数へコールバックする。
ビーコンのスキャンは JBCPManager.startScan でバックグラウンドにて行われる。  



### メソッド

    void fireEvent(Map event)


メソッド
--------

### fireEvent

`````````````````````````
void fireEvent(Map event)
`````````````````````````

ビーコンを受信し、条件に合致すると、このコールバック関数がコールされる。  
event に受信したビーコン情報、トリガーの条件、実行すべきアクションを辞書形式で格納する。  
event に格納されるデータをJSON形式で表すと以下のイメージである。



    {
      "triggers": [{
        "type" : {"signal":true, "region":true, "time":false, "timestamp":false},
        "beacon_id" : [xxx, yyy, zzz],
        "signal_condition" : "far",
        "region_condition" : "area_in",
      },
      {
        "type" : {"signal":false, "region":false, "time":true, "timestamp":true},
        "time" : {"start": "12:00", "end": "13:00"},
        "datetime" : {"start": 1407596400, "end": 1408028400},
        "historical_condition" : "時間:06:30(comming soon)"
      }],
      "trigger_condition": {"$seq": [0, 1]},
      "action_data" : {
        "action" : "notification",
        "text" : "いつもご来店ありがとうございます。ただいまタイムサービス中です！"
      }
    }

- triggers  
 イベントの条件を格納する。複数のトリガーを配列形式で格納する。  
 ひとつのトリガーには以下のデータが格納される。

  - type
   トリガー条件の種別を格納。複数のトリガー条件に対し真偽値で格納する。
   値はオブジェクト形式で格納し、各トリガー条件を使用するしないを格納。

   |    キー    |                   意味                     |
   |------------|--------------------------------------------|
   |"signal"    |ビーコンからの距離条件                      |
   |"region"    |ビーコンエリアへの入出条件                  |
   |"time"      |時刻条件                                    |
   |"datetime"  |期間条件                                    |

  - beacon_id  
   トリガーにビーコンを使う場合の beacon_id が複数格納される。  
   値はサーバで生成したビーコンIDになる。  
   type.signal, type.region のいずれかが true であれば必須である。  

  - signal_condition  
   ビーコンからの距離を次の値で指定。  
   
   |     値     |                   意味                     |
   |------------|--------------------------------------------|
   |"immediate" |ビーコンからの距離がほぼ 0                  |
   |"near"      |ビーコンからの距離が1〜3メートルぐらいの距離|
   |"far"       |ビーコンを検出かつ near 以上の距離          |

  - region_condition  
   ビーコンを検出した場合をエリアイン、その場所から離れそのビーコンを検出しなくなった場合をエリアアウトと呼び、次の値を格納する。  
   "area_in", "area_out"

  - time_condition  
   時刻条件の場合に開始時間と終了時間をJSONオブジェクト形式で格納する。  
   キーは "start", "end" が入り、それぞれ開始時間、終了時間を格納する。  
   値の形式は MM:DD である。

  - datetime_condition  
   期間条件の場合に開始日時と終了日時をJSONオブジェクト形式で格納する。  
   キーは "start", "end" が入り、それぞれ開始時間、終了時間を格納する。  
   値の形式はUnixタイムスタンプである。

  - histroical_condition  
   指定回数だけ検知などの条件の場合の回数や、指定時間以降などの時間を格納する。(comming soon)  

- trigger_condition  
 triggers が複数の場合に定義する。
 各トリガーの組み合わせ条件をJSON形式で定義する。
 
 **JSON形式 : {key : value}**  
 key には次の演算子を定義する。  
 "$and" : value に配列を格納し、各配列値が and 条件であることを表す。  
 "$or" : value に配列を格納し、各配列値が or 条件であることを表す。  
 "$seq" : value に配列を格納し、各配列値の条件を順序どおりに満たすことで成立する。  
 
 value には triggers 配列に格納されたトリガーへの位置（添字）が格納される。  
 
 例. triggers に 0,1,2 の３つが格納されている場合  
 例1. 0 -> 1 -> 2 の順で発火する条件  
 {"$seq": [0, 1, 2]}
 
 例2. 0 -> 2 または 1 -> 2 で発火する条件  
 {"$seq": [{"$or": [0, 1]}, 2]}  
 下記と同値  
 {"$or": [{"$seq": [0, 2]}, {"$seq": [1, 2]}]}


- action_data  
 実行すべきアクションを格納する。
 key, value 形式の辞書型とする。
 key には必ず "action" キーが存在し、その値にアクションの種別を格納する。
 
  - action の値

  |値           |説明                  |
  |-------------|----------------------|
  |notification |ローカル通知          |
  |open-url     |Webブラウザ通知       |
  |url-scheme   |URLスキーム通知       |
  |custom       |カスタムデータを表す  |

  - notification の場合  
    キー text の値に表示する文言が格納される
    
    `````````````````````````````````````````````````````
    "action_data" : {
      "action" : "notification",
      "text" : "いつもご来店ありがとうございます。ただいまタイムサービス中です！"
    }
    `````````````````````````````````````````````````````

  - open-url の場合  
    キー url の値に Web ページに表示するURLを格納
    
    ````````````````````````````````````````````````````````
    "action_data" : {
      "action" : "open-url",
      "url" : "http://some-server/some-path/?some-query=foo"
    }
    ````````````````````````````````````````````````````````

  - url-scheme の場合  
    キー url の値に URLスキームを実行するための URL を文字列で定義
    
    ````````````````````````````````````````````````````````
    "action_data" : {
      "action" : "url-scheme",
      "url" : "mailto:someone@some-company.com?subject=test mail&body=hello world"
    }
    ````````````````````````````````````````````````````````
    
  - custom の場合
    CMSで登録した任意のキー、値で構成される

    ````````````````````````````````````````````````````````
    "action_data" : {
      "action" : "custom",
      "custom-key1" : "custom-value1",
      "custom-key2" : "custom-value2"
    }
    ````````````````````````````````````````````````````````


#### パラメータ
- event  
 イベントデータを辞書形式で格納する。上記のとおり。
