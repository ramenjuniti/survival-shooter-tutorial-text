# 体力ゲージとスコア
### ここでは，プレイヤーの体力を表す体力ゲージとスコアの値を表示させる
## 2D画面の作成
"Scene" を "2D" にする
<img src="../img/Health-HUD/change-2D.png">

"GameObject > UI > Canvas" を選択<br>
"Canvas" についての詳細は以下のリンクへ
### [Canvas](https://docs.unity3d.com/ja/current/Manual/class-Canvas.html)
<img src="../img/Health-HUD/create-canvas.png">

"HUDCanvas" に名前を変更
<img src="../img/Health-HUD/rename-canvas.png">

"Add Component" で　"Canvas Group" を追加<br>
設定は以下のようにする
<img src="../img/Health-HUD/add-component-canvas-group.png">

## 体力のUI作成
"HUDCanvas" で右クリックし，"Create Empty" を選択
<img src="../img/Health-HUD/HUDcanvas-create-children.png">

"HealthUI" に名前を変更
<img src="../img/Health-HUD/rename-children.png">

"Rect Transform" の "Anchor Presets" をAltキー+Shiftキーを押しながら左下合わせをクリック
<img src="../img/Health-HUD/layout-select.png">

"width" と "Height" を以下のように設定
<img src="../img/Health-HUD/change-width-height.png">

"HealthUI" で右クリックし，"UI > Image" を選択<br>
"Image" についての詳細は以下のリンクへ
### [Image](https://docs.unity3d.com/ja/current/Manual/script-Image.html)
<img src="../img/Health-HUD/create-healthUI-img.png">

"Image" を "Heart" にする
<img src="../img/Health-HUD/rename-heart.png">

"Heart" のプロパティの設定は以下のようにする<br>
"Width" と "Height" を共に "30"<br>
"Source IImage" は "Heart"
<img src="../img/Health-HUD/image-param.png">

"HealthUI" で右クリックし，"UI > Slider" を選択<br>
"Slider" についての詳細は以下のリンクへ
### [Slider](https://docs.unity3d.com/ja/current/Manual/script-Slider.html)
<img src="../img/Health-HUD/create-slider.png">

"Slider" を以下のようにする<br>
名前を "HealthSlider" に変更<br>
"Pos X" を "110"
<img src="../img/Health-HUD/Slider-change-param.png">

"HealthSlider" の子である "Handle Slide Area" を削除する
<img src="../img/Health-HUD/HandleSlideArea-delete.png">

"HealthSlider" を選択し，"Inspector" 内の "Transition" を "None" にする
<img src="../img/Health-HUD/transition-mode-none.png">

"Max Value" と "Value" を "100" にする
<img src="../img/Health-HUD/health-value.png">

## ダメージエフェクトの作成
"HUDCanvas" で右クリックし，"UI > Image" を選択
<img src="../img/Health-HUD/create-damage.png">

"DamageImage" に名前を変更<br>
"Rect Transform" の "Anchor Presets"をAltキーを押しながら一番右下のアイコンをクリック<br>
<img src="../img/Health-HUD/create-damage-img.png">

"Color" を以下のように変更する<br>
"R: 255" と "A: 0" に変更
<img src="../img/Health-HUD/change-color.png">
