# GameObject.Find()

`GameObject.Find("Cube")`などとすることで，ゲームオブジェクトを名前で検索することができます．

返り値は`GameObject`型です．検索出来なかった場合には`null`が返ります．

処理の負荷が高いため，`Start()`などの繰り返し呼び出されることのない関数内で使用し、フィールド（変数）に代入しておくべきです．

## 例題
以下の操作をおこない，ログに何が出力されるかを確認しなさい．
1. Scene（シーン）に`Cube`と`Sphere`を追加し，名前をそれぞれ変更する．
2. C#スクリプトを`CheckExistsSphere`という名前で作成し，もともと`Cube`という名前だった`GameObject`にアタッチする．
3. `CheckExistsSphere`を編集し，存在すれば「(`GameObject`名) found!」，存在しなければ「(`GameObject`名) not found...」がログに出力されるようなコードを作成する．
4. `Playmode`で動作を確認する．
5. もともと`Sphere`という名前だった`GameObject`の有効/無効を切り替えて，改めて動作を確認する．


![](https://panmemory.com/wp-content/uploads/2024/09/24600.jpg)
