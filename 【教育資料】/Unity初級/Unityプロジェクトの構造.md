# Unityプロジェクトの構造

きっと`src/`直下にはこんな構造があるはず

## src/
```
.gitattributes
.gitignore
.vscode/
.vsconfig
AircraftData/
Assets/
Config.txt
CustomPlaneData.csv
Log.txt
Packages/
ProjectSettings/
SavedGraphs/
UIElementsSchema/
unityProject.vrmanifest
```
んで，ゲームに必要なもの（これらはアセットと呼ぶ）は`Assets/`に配置されている．

## Assets/
```
3D Model/
3D Model.meta
AircraftData/
AircraftData.meta
Fonts/
Fonts.meta
Materials/
Materials.meta
Other/
Other.meta
PlayerSettings.preset
PlayerSettings.preset.meta
Prefab/
Prefab.meta
Resources/
Resources.meta
Samples/
Samples.meta
Scenes/
Scenes.meta
Script/
Script.meta
Standard Assets/
Standard Assets.meta
TextMesh Pro/
TextMesh Pro.meta
UI Toolkit/
UI Toolkit.meta
UniversalRenderPipelineGlobalSettings.asset
UniversalRenderPipelineGlobalSettings.asset.meta
XCharts/
XCharts.meta
XR/
XR.meta
XRI/
XRI.meta
~Ray.csv
~Ray.csv.meta
~Tatsumi.csv
~Tatsumi.csv.meta
```

## 特によくお世話になるであろうディレクトリ
- `Script/`：C#スクリプトを配置する．
- `Scenes/`：シーンを配置する．
  - `FlightScene`をダブルクリックすれば見慣れた開発画面が表示される．
  - タイトル画面などを作りたいならここに追加する．
- `Resources/`：リソース（ソフト動作中に動的に読み込むプレハブなど）を配置する．
  - これのいいところは1行の処理で読み込めること
    ```csharp
    GameObject PlaneObj = (GameObject)Resources.Load(planeName);
    GameObject PlaneObj = Resources.Load<GameObject>(planeName);
    ```
  - [非推奨？だったりするようだが...](https://zenn.dev/gameshitai/articles/7d4bc03477d9c6)問題が発生したら考えてくれ
- `Prefab/`：プレハブを配置する．
  - こちらはソフト動作中には必要としないものを配置している．

  