# GameManeger.cs

`GameManeger`クラスが定義されている．

読み込まれると，以下の行で`GameManeger`型の変数が宣言・初期化される．  
`static`キーワードにより，クラス内に1つだけ存在することが保証される．
```cs
public static GameManeger instance = null;
```

`instance == null`の場合，クラス自身が代入される．
```cs
instance = this;
```

ここに，ほとんどのグローバル変数やフラグが宣言されている．  
以下のようにして参照する．
B
> AerodynamicCalculator.cs
```cs
else if (GameManeger.instance.PlaneName == "Ray") {...}
```
便利だが，名前が長いので，
```cs
private GameManager gm = GameManager.instance;
```
とするのが良い．