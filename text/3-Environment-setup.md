# 環境構築
### ここでは，ゲームのフィールドとBGMを実装する
## ゲームのフィールドを実装
"Scenes" というフォルダをAssets内に作成する
<img src="../img/Environment-setup/create-new-folder.png">

シーンを作成します <br>
"File" > "New Scene" 
<img src="../img/Environment-setup/new-scene-select.png">

"Level 01" という名前でsceneで保存します<br>
"File" > "Save Scene as"
<img src="../img/Environment-setup/scene-save-as.png">

"Assets > Scenes" に保存する
<img src="../img/Environment-setup/save-scene-level01.png">

"Assets > Prefabs" から "Environment" を "Hierarchy" にドロップする
<img src="../img/Environment-setup/select-Environment.png">

"Environment" をクリックし，<br>
"Inspector" 内の "Transform" のPositionを <br>
"x : 0, y : 0, z : 0" にする
<img src="../img/Environment-setup/drop-Environment.png">

同じ手順をPrefabs内の "Lights" にも行う
<img src="../img/Environment-setup/drop-Lights.png">

"GameObject > 3D Object > Quad" で "Quad" を作成
<img src="../img/Environment-setup/create-Floor.png">

名前を "Floor" に変更する<br>
Transformの値を以下のように設定する
<img src="../img/Environment-setup/Floor-inspector.png">

"Mesh Renderer" を "Remove Component" で削除する
<img src="../img/Environment-setup/Floor-remove-MeshRenderer.png">

"layer" を "Floor" に変更する
<img src="../img/Environment-setup/change-layer-floor.png">

## BGMを実装
"GameObject > Create Empty" で GameObjectを生成
<img src="../img/Environment-setup/create-backgroundMusic.png">

"backgroundMusic" に名前を変更
<img src="../img/Environment-setup/rename-backgroundMusic.png">

"Inspector" から "Add Component" で "Audio Source" を追加
<img src="../img/Environment-setup/add-component-Audio.png">

"Audio Source" 内の "AudioClip" の入力欄の右にあるボタンを押し，<br>
表示されたAudioの中から "Background Music" を選択
<img src="../img/Environment-setup/select-backgroundMusic.png">

"Audio Source" 内の "Loop" にチェックを入れる
<img src="../img/Environment-setup/loop-backgroundMusic.png">