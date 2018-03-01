# ゲームオーバの実装
### ここでは，プレイヤーの体力が無くなって死んでしまった時に，ゲームオーバ画面が表示されるようにする
"HUDCanvas" で右クリックし，"UI > Image" を選択
<img src="../img/Game-Over/create-screen-fader.png">

名前を "ScreenFader" に変更し，プロパティを以下のように設定する<br>
"Rect Transform" の "Anchor Presets" はAltキーを押しながら，右下のものを選択する<br>
"Color" は水色に変更
<img src="../img/Game-Over/screen-fader-param.png">

"HUDCanvas" で右クリックし，"UI > Text" を選択<br>
名前を "GameOverText" に変更<br>
"Rect Transform" の "Anchor Presets" をAltキーを押しながら，真ん中のものを選択<br>
"Width" を "300"，"Height" を "50"<br>
"Text" を "GameOver!"に変更し，"Font" を "LuckiestGuy" に変更<br>
"Alignment" で左右上下を中央にし，"Color" を白にする<br>
"Add Component" で "Shadow" を追加し，以下のようにプロパティを設定する
<img src="../img/Game-Over/game-over-text-param.png">

"HUDCanvas" 内の子要素の順番を以下のようにする
* HealthUI
* ScreenFader
* GameOverText
* ScoreText

<img src="../img/Game-Over/change-order.png">

"ScreenFader" の "Color" の "A: 0" とし，透明にする
<img src="../img/Game-Over/screen-fader-alpha-0.png">

同様に"GameOverText" も透明にする
<img src="../img/Game-Over/game-over-alpha-0.png">

"Hierarchy" の "HUDCanvas" をクリックする<br>
"Window > Animation" を選択
<img src="../img/Game-Over/select-window-animation.png">

"Create" をクリック
<img src="../img/Game-Over/click-create-animation.png">

"GameOverClip.anim" とし，"Animation" フォルダに "Save" する
<img src="../img/Game-Over/create-GameOverClip.png">

"Add Property > GameOverText > Text > Color" の右にある "+" をクリック
<img src="../img/Game-Over/add-text.png">

"Add Property > GameOverText > RectTransform > Scale" の右にある "+" をクリック
<img src="../img/Game-Over/add-text-rect.png">

"Add Property > ScreenFader > Image > Color" の右にある "+" をクリック
<img src="../img/Game-Over/add-image.png">

"Add Property > ScoreText > RectTransform > Scale" の右にある "+" をクリック
<img src="../img/Game-Over/add-score-rect.png">

全てのキーフレームを "0:30" に移動させる
<img src="../img/Game-Over/move-animation-end.png">

白いラインを "0:20" に合わせる
<img src="../img/Game-Over/20sec-line.png">

"GameOverText : Scale" で右クリックし，"Add Key" を選択
<img src="../img/Game-Over/add-key.png">

以下のように "0:20" の部分に "Key" が二つ追加される
<img src="../img/Game-Over/plus-key.png">

白いラインを "0:00" に合わせ，"GameOverText : Scale" を以下のように全て "0" にする
<img src="../img/Game-Over/scale-0.png">

"0:20" のときは全て "1.2" にする
<img src="../img/Game-Over/1.2-text-scale.png">

白いラインを "0:30" に合わせ，"GameOverText : TextColor" の "Color.a" を "1" とする
<img src="../img/Game-Over/30sec-color-1.png">

同様に "ScreenFader : Image.Color" の "Color.a" を "1" とする
<img src="../img/Game-Over/30sec-image-1.png">

"ScoreText : Scale" を全て "0.8" とする
<img src="../img/Game-Over/30sec-score-scale.png">

全てのキーフレームを　"1:30" に移動させる
<img src="../img/Game-Over/move-key-frames.png">

赤い録画ボタンを押す
<img src="../img/Game-Over/record-key-frames.png">

もう一度録画ボタンを押す
<img src="../img/Game-Over/recorded.png">

"Assets > Animation" フォルダ内の "GameOverClip" を選択
<img src="../img/Game-Over/select-game-over-animation.png">

"Loop Time" のチェックを外す
<img src="../img/Game-Over/checkout-loop.png">

"Assets > Animation" フォルダ内の "HUDCanvas" を選択
<img src="../img/Game-Over/select-HUDCanvasAC.png">

"GameOverClip" をクリック
<img src="../img/Game-Over/click-game-over-clip.png">

"Animator" ウィンドウで "Create State > Empty" を選択
<img src="../img/Game-Over/create-state.png">

"Empty" に名前を変更
<img src="../img/Game-Over/rename-empty.png">

"Empty" から "Make Transition" で "GameOverClip" に矢印を結ぶ
<img src="../img/Game-Over/make-transition.png">

"Trigger parameter" を作成し，"GameOver" とする
<img src="../img/Game-Over/trigger-game-over.png">

"Set as Layer Default State" で "Empty" を初期状態する
<img src="../img/Game-Over/empty-state-default.png">

"Empty　→ GameOverClip" の矢印をクリックし，"Condition" に "GameOver" を入れる
<img src="../img/Game-Over/condition-game-over.png">

"Hierarchy" の "HUDCanvas" に "Assets > Scripts > Managers" フォルダ内にある "GameOverManager" をドロップする
<img src="../img/Game-Over/drop-GameOver-HUDCanvas.png">

"GameOverManager" を確認する
```
using UnityEngine;

public class GameOverManager : MonoBehaviour {

  // 変数の宣言
  public PlayerHealth playerHealth;


  Animator anim;

  // ゲームが始まる前に変数を初期化
  void Awake() {
    anim = GetComponent<Animator>();
  }

  /* 
   * フレーム毎にプレイヤーの体力を評価し，
   * プレイヤーの体力が0以下であるなら，ゲームオーバーのアニメーションを起動させる 
   */
  void Update() {
    if (playerHealth.currentHealth <= 0){
      anim.SetTrigger("GameOver");
    }
  }
}
```

"GameOverManager" の "Player Health" に "Hierarchy" の "Player" を入れる
<img src="../img/Game-Over/drop-player-game-over.png">

テストプレイをする<br>
プレイヤーの体力が無くなった時に， ゲームオーバー画面が表示されれば問題ない
<img src="../img/Game-Over/test-play.png">

以上でゲームは完成です<br>
お疲れ様でした