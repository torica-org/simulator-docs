# MonoBehaviourからの脱却
`AeroDynamicCalculator`などのクラスでは，計算さえできればよいです．  
`AddForce()`などの関数を使っていますが，これらは`using UnityEngine`で使えます．

`Config`などのクラスでは，変数と値を管理できればいいだけです．

これらを意味もなく何かしらのGameObjectにアタッチする必要はないという話です．

## MonoBehaviourを継承しないと？
### アタッチができない
アタッチできなくてもUnityはちゃんと認識しています．  
問題ありません．

### Start()とかUpdate()が動かない
継承元（上書き先）がありませんから当然です．  
でも`Start()`が動かないのは困ります．きっかけ無しでは動かし続けることはできません．  
C#には「属性」というものがあり，とっつきにくいですがこれを使うと勝手に実行される関数が書けます．
```csharp
using System;
private double intervalMs = 1000;
// 定期実行のための`Timer`
private System.Timers.Timer timer;

// ===== ↓↓↓ これが属性 ==========
[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)] // ゲームのシーンがロードされる前にこのメソッドを呼び出すための属性.
private static void Initialize()
{
    Debug.Log("これはGameObjectにアタッチされていなくても動く");
    // ここに定期実行されるような処理を書けば，ずっと実行させることができます．
    // 例えば
    // `Timer`を初期化し，指定した間隔で自動的にイベントを発生させるように設定.
    timer = new System.Timers.Timer(intervalMs) { AutoReset = true, Enabled = true };
    timer.Elapsed += (s, e) => 
    {
        Debug.Log("1秒おきに定期実行されます");
    };
    timer.Start();
}
```
### ゲーム内のオブジェクトを操作できない
`MonoBehavior`を継承しないと，Unityのゲーム自体とは全く別の動作をすることになります．  
それでもゲーム内のオブジェクトに干渉したいときは，メインスレッド（ゲームの処理をおこなっている場所）に処理を投げる必要があります．
1. まず，フィールド（変数）に`SynchronizationContext`を宣言すると同時に，現在の処理の文脈（`context`）を取得します．
  ```csharp
  private static SynchronizationContext context = SynchronizationContext.Current;
  ```
2. 次に，`context`を使って，メインスレッドに処理を投げます．
  ```csharp
  context.Post(state => 
    {
      Debug.Log("メインスレッドに処理を投げました");
      Debug.Log("この中なら，GameObjectに干渉することができます");
      Debug.Log("UnityEngine名前空間がusingされていることが前提です");
    }, null);
  ```
## ゲームが異常終了する
起こりがちな問題のひとつに`NullReferenceException`があります．  
日本語では「Null参照例外」です．  
変数に格納されている中身（オブジェクト）が`null`（からっぽみたいなもの）になっているのに，その変数にアクセスしようとしたときに発生します．  

発生する理由は，Unityのシーンロードが起こっても，独立したスクリプトからはわからないからです．  
初期化時に正しく取得していても，シーンが再ロードされると元のオブジェクトは`null`になり，新しいオブジェクトは全く別のものとして再生成されます．  

この例外を回避するには，
1. `SceneManager.sceneLoaded`などのイベントを使ってシーンのロードを検知する．
> GameManager.cs
```csharp
private void OnEnable()
{
    SceneManager.sceneLoaded += OnSceneLoaded;
}

private void OnSceneLoaded(Scene scene, LoadSceneMode mode)
{
    game.status = GameParameters.Status.Preparation;
    calc?.Initialize(); // シーンロードのたびにRigidbodyが破壊されるので再取得
}
```
2. `if`文で`null`の場合の処理をちゃんと書く．
> HoldingDetector.cs
```csharp
if (cm == null)
{
    cm = GameObject.Find("CameraManager").GetComponent<CameraManager>();
}
if (cm != null)
{
    cm.CalibrateVR();
}
```