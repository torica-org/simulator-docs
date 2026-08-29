# Logについて

`DataIO/`内の`Log.cs`に`static`なクラスとして実装されています．

`static`なのでプロジェクト内のどこでも，`Log.Append()`を適当に実行するだけで，実行時の時間とともにログが`Log.txt`に記録されます．