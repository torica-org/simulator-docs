# C#スクリプトの扱い

Unityの`Assets/`に含まれるC#スクリプトは**全て常にUnity側で監視されています**．  
※ちなみに`Script/`に含まれていなくても読み込まれます．  
読み込ませたくないなら`Assets/`ディレクトリから出してください．

ここでは，あなたが例えば自動操縦のためのC#スクリプト`AutoPilot.cs`を作成したとします．  
すると，自動的に以下のようなスクリプトが作成されるでしょう．
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class AutoPilot : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```
ここで最低限知っていてほしいのは，`class AutoPilot`となっている部分です．  
これはクラスの宣言と定義を行っている部分であり，`int`や`float`などと同様の**型**のようなものです．  
`AutoPilot.cs`が`Assets/`配下に存在するなら，Unityによってこれは読み込まれます．  
すると，別のスクリプトで変数として宣言や定義ができるようになります．
```csharp
public class AnotherScript : MonoBehaviour
{
    private AutoPilot autoPilot;

    private void Start()
    {
        autoPilot = GameObject.FindFirstObjectByType<AutoPilot>(); // Unity2023以降推奨されている方法らしい
    }
}
```
特別な操作なしに他のファイルにまで影響が及んでいるような気がするのは，Unityが自動的に全てを検索しているからです．

## おまじないの理解
### using UnityEngine;
これを宣言すると，`GameObject`や`Transform`，`Rigidbody`などの基本的なクラス，Unity特有の`Time`や`Debug.Log()`などが使えるようになります．
### [クラス名] : MonoBehavior
`MonoBehavior`という文字がコロン`:`の後についています．  
これは継承と呼ばれる仕組みです．継承元の関数を使うことができるようになったりします．  
できるようになることを以下に示します．
- ゲームオブジェクトにアタッチできるようになる．
- `Start()`，`Update()`，`FixedUpdate()`などが（オーバーライド，つまり上書きすることで）使えるようになる．
- `GetComponent<>()`などの関数が使えるようになる．

つまり，これらが必要なければいらないということ...  
簡単な制御をするにはとても便利，必須なくらいですが，たまに邪魔になります．

## クラス名とファイル名の一致
`class AutoPilot`が定義されているとき，これをGameObjectにアタッチするなら，C#スクリプトのファイル名は`AutoPilot.cs`となっている必要があります．  
もし後から変えたくなった場合は，どちらも変更する必要があります．

もしファイル名が`AutoPilot.cs`で，クラス名がそれに一致していない場合，アタッチしようとすると以下のようなエラーが出ます．
```
Can't add script component 'AutoPilot' because the script class cannot be found. Make sure that there are no compile errors and that the file name and class name match.
```
