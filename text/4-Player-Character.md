# プレイヤーキャラクター
### ここでは，プレイヤーを実装する
## プレイヤーの実装
用意されているプレイヤーキャラクターのモデルを使用します<br>
"Assets > Models> Characters > Player"
<img src="../img/Player-Character/find-player-model.png">

"Hierarchy" にドロップする<br>
"Position" は (x: 0, y: 0, z: 0) に設定
<img src="../img/Player-Character/drop-player.png">

"Inspector" で "Tag" を "Player" に設定する
<img src="../img/Player-Character/change-tag-player.png">

## プレイヤーにアニメーションをつける

"Assets" の下に "Animation" というフォルダを作る
<img src="../img/Player-Character/create-animation-folder.png">

"Animation" の中で右クリックで　"Create > Animator Controller" を選択
<img src="../img/Player-Character/create-animator-controller.png">

名前を "PlayerAC" とする
<img src="../img/Player-Character/rename-animator-controller.png">

"Player" にドロップする
<img src="../img/Player-Character/drop-playerAC.png">

"PlayerAC" をダブルクリックし，"Animator" というウィンドウを表示させる
<img src="../img/Player-Character/display-animator-window.png">

"Assets > Models > Characters > Player > Death" をクリックし，<br>
Animatorウィンドウにドロップします<br>
以上の作業を "Idle" と "Move" にも行う
<img src="../img/Player-Character/find-death-animation.png">

問題無ければ、以下のように "Death", "Idle", "Move" が"Animator"ウィンドウに表示される
<img src="../img/Player-Character/drop-death-Idle-Move.png">

"Idle"を初期状態として設定するため，"Idle" を右クリックし，<br>
"Set as Layer Default State" を選択
<img src="../img/Player-Character/Idle-set-default-state.png">

選択すると "Idle" がオレンジ色になる
<img src="../img/Player-Character/Idle-change-color-orange.png">

"Parameters" の "+" をクリックし，"Bool" を選択する<br>
"Bool" は "True" か "False" の真偽値を持つデータ型である<br>
今回は，キャラクターが移動しているか，していないかの二つの状態を判定するために使用する
<img src="../img/Player-Character/add-pram-bool.png">

"IsWalking" とする
<img src="../img/Player-Character/create-IsWalking.png">

同様に，"Trigger"パラメータとして "Die" を作成する<br>
"Trigger" は "Bool" と同様に真偽値を持つが，<br>
"Trigger" は "True" になると自動的に "False" に戻る性質がある
<img src="../img/Player-Character/create-Die.png">

"Idle" を右クリックし，"Make Transition" を選択
<img src="../img/Player-Character/make-transition.png">

出てきた矢印を "Move" に結合する
<img src="../img/Player-Character/connect-Move.png">

矢印をクリック
<img src="../img/Player-Character/click-allow.png">

"Conditions" で "IsWalking" を選択し，"true" とする
<img src="../img/Player-Character/Idle-animation.png">

同様に "Move" で "Make Transition" を選択し，"Idle" へと矢印を伸ばす<br>
"Conditions" は "IsWalking" を選択し、"false" にする<br>
この二つの矢印が意味することは，<br>
**"IsWalking" が "true" であるならば，初期状態の "Idle" から　"Move" に状態が遷移し，<br>
逆に "IsWalking" が　"false" であるならば，"Move" から "Idle" の状態に戻る<br>**
ということである
<img src="../img/Player-Character/Move-animation.png">

"Any State" で "Make Transition" を選択，"Death" へ矢印を伸ばす<br>
"Conditions" は "Die" を選択する
<img src="../img/Player-Character/Any-State-allow-Death.png">

## プレイヤーに様々なコンポーネントを追加する
"Player"オブジェクトを選択，"Add Component > Rigidbody" を追加
<img src="../img/Player-Character/add-component-RigidBody.png">

"Rigidbody" は以下のスクリーンショットのように設定する<br>
"Rigidbody" を追加することによってオブジェクトを物理特性を与えて制御することができる<br>
"Rigidbody" 詳細は以下のリンクへ
### [Rigidbody](https://docs.unity3d.com/ja/2017.3/Manual/class-Rigidbody.html)
<img src="../img/Player-Character/RigidBody-prameter.png">

同様に，"Add Component > Capsule Collider" を追加<br>
以下のスクリーンショットのように設定する<br>
"Capsule Collider" の詳細は以下のリンクへ
### [Capsule Collider](https://docs.unity3d.com/ja/2017.3/Manual/class-CapsuleCollider.html)
<img src="../img/Player-Character/add-component-capsule-collider.png">

"Add Component > Audio Source" を追加
<img src="../img/Player-Character/add-audio-source.png">

"AudioClip" の右にある○をクリックし，出てきたウィンドウの中から "Player Hurt" を選択
<img src="../img/Player-Character/audio-clip-player-hurt.png">

他のプロパティーは以下のようになる
<img src="../img/Player-Character/player-audio-source.png">

"Assets > Script > Player > PlayerMovement" を "Player" にドロップ
<img src="../img/Player-Character/click-playerMovement.png">

以下のように追加される
<img src="../img/Player-Character/drop-playermovement.png">

追加した "PlayerMovement" をダブルクリックし，エディタを起動
<img src="../img/Player-Character/editer.png">

変数の追加
```
using UnityEngine;

public class PlayerMovement : MonoBehaviour {
  public float speed = 6f;
  Vector3 movement;
  Animator anim;
  Rigidbody playerRigidbody;
  int floorMask;
  float camRayLength = 100f;   
}
```

"Awake" メソッドを追加<br>
"Awake" はゲームが始まる前に変数やゲーム状態を初期化するために使用
### [Awake](https://docs.unity3d.com/jp/current/ScriptReference/MonoBehaviour.Awake.html)
ここでは "floor" オブジェクト，"Animator" コンポネート，"player" の "Rigidbody" コンポーネントを取得し，それぞれの変数に代入する
```
void Awake () {
  floorMask = LayerMask.GetMask ("Floor");

  anim = GetComponent <Animator> ();
  playerRigidbody = GetComponent <Rigidbody> ();
}
```

"FixedUpdate" メソッドを追加<br>
このメソッドは固定フレームレートで呼び出される
### [FixedUpdate](https://docs.unity3d.com/jp/current/ScriptReference/MonoBehaviour.FixedUpdate.html)
矢印キーの入力を受け付けるようにする<br>
Input.GetAxisRawについては以下のリンクに詳細が載っています
### [Input.GetAxisRaw](https://docs.unity3d.com/jp/current/ScriptReference/Input.GetAxisRaw.html)
```
void FixedUpdate () {
  float h = Input.GetAxisRaw ("Horizontal");
  float v = Input.GetAxisRaw ("Vertical");
}
```

"Move" メソッドを追加<br>
引数に渡された値をもとにプレイヤーを移動させる
```
void Move (float h, float v) {
  movement.Set (h, 0f, v);

  movement = movement.normalized * speed * Time.deltaTime;

  playerRigidbody.MovePosition (transform.position + movement);
}
```

"Turning" メソッドを追加<br>
ゲーム画面内にあるマウスのポインタの方を，常にプレイヤーが向くようにする
```
void Turning () {
  Ray camRay = Camera.main.ScreenPointToRay (Input.mousePosition);

  RaycastHit floorHit;

  if(Physics.Raycast (camRay, out floorHit, camRayLength, floorMask)) {
    Vector3 playerToMouse = floorHit.point - transform.position;

    playerToMouse.y = 0f;

    Quaternion newRotation = Quaternion.LookRotation (playerToMouse);

    playerRigidbody.MoveRotation (newRotation);
  }
}
```

"Animating" メソッドを追加<br>
プレイヤーが移動すると "IsWalking" を "true" にし，アニメーションにセットする<br>
この"IsWalking" はアニメーションのときに設定したものです
```
void Animating (float h, float v) {
  bool walking = h != 0f || v != 0f;

  anim.SetBool ("IsWalking", walking);
}
```

"FixedUpdate"から各メソッドを呼び出すため，コードを追加
```
void FixedUpdate ()
{
  float h = Input.GetAxisRaw ("Horizontal");
  float v = Input.GetAxisRaw ("Vertical");

  Move (h, v);
  Turning ();
  Animating (h, v);
}
```

"PlayerMovement" のコード全体は以下のようになる
```
using UnityEngine;

public class PlayerMovement : MonoBehaviour {
  public float speed = 6f;
  Vector3 movement;
  Animator anim;
  Rigidbody playerRigidbody;
  int floorMask;
  float camRayLength = 100f;   

  void Awake () {
    floorMask = LayerMask.GetMask ("Floor");

    anim = GetComponent <Animator> ();
    playerRigidbody = GetComponent <Rigidbody> ();
  }

  void FixedUpdate () {
    float h = Input.GetAxisRaw ("Horizontal");
    float v = Input.GetAxisRaw ("Vertical");

    Move (h, v);
    Turning ();
    Animating (h, v);
  }

  void Move (float h, float v) {
    movement.Set (h, 0f, v);

    movement = movement.normalized * speed * Time.deltaTime;

    playerRigidbody.MovePosition (transform.position + movement);
  }

  void Turning () {
    Ray camRay = Camera.main.ScreenPointToRay (Input.mousePosition);

    RaycastHit floorHit;

    if(Physics.Raycast (camRay, out floorHit, camRayLength, floorMask)) {
      Vector3 playerToMouse = floorHit.point - transform.position;

      playerToMouse.y = 0f;

      Quaternion newRotation = Quaternion.LookRotation (playerToMouse);

      playerRigidbody.MoveRotation (newRotation);
    }
  }

  void Animating (float h, float v) {
    bool walking = h != 0f || v != 0f;

    anim.SetBool ("IsWalking", walking);
  }

}
```

スクリプトを保存し，上部の "Play" をクリック<br>
ここでエラーが出ていなければ問題ない
<img src="../img/Player-Character/click-play.png">

実際に矢印キーを入力し，移動しているときと止まっているときでアニメーションが異なることが確認できれば問題ない
<img src="../img/Player-Character/play-mode.png">

