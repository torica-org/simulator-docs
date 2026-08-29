# CsvIOについて

## 概要
これはインスタンス化して使用する，文字区切りテキストファイルの入出力クラスです．

## サンプル
具体的な使い方を示します．
```cs
using (CsvIO csv = new CsvIO(50, 20))
{
  csv.Load(Path.Combine(Directory.GetParent(Application.dataPath).FullName, "Test.csv")); // ファイルの読み込み
  print(csv.Read(1, 1)); // 1行1列の位置にあるデータの読み出し
  print(csv.Read(2, 1)); // 2行1列の位置にあるデータの読み出し

  csv.Write(1, 1, "Hello"); // 1行1列の位置に書き込むデータの一時保存
  csv.Write(2, 1, "World"); // 2行1列の位置に書き込むデータの一時保存
  csv.Flush(Path.Combine(Directory.GetParent(Application.dataPath).FullName, "Test.csv")); // ファイルへの書き込み
}
```

## 関数
### コンストラクタ(`new CsvIO(int records, int fields)`)
- 読み込み範囲を`records`（行）と`fields`（列）に整数値で指定してインスタンス化します．
### `Load(string filepath)`
- 引数はファイルのフルパスです．
