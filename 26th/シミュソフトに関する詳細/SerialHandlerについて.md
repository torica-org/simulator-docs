# SerialHandlerについて

`using System.IO.Ports`が必要．
`Thread`によりバックグラウンドで常時処理をおこなう．

これも`static`なクラスなので，`SerialHandler.status`などと直接アクセスできる．

`SerialPort`クラスのインスタンスを作成し，`Open`メソッドでシリアルポートを開く．

`status`は`string`型で，シリアルポートの状態を表す．  
`FlightSettingScreen`に表示している．

## 接続
`Config.txt`を編集することで接続をおこなう．