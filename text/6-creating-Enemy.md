# 敵の実装
### ここでは，プレイヤーを襲う敵を実装する

"Assets > Models > Characters" 内の "Zombunny" を "Hierarchy" にドロップ
<img src="../img/creating-Enemy/drop-Zombunny.png">

"Zombunny" を選択し，"Inspector" 内の "Layer" を "Shootable" に変更する
<img src="../img/creating-Enemy/Zombunny-layer-change.png">

"Yes, change children" を選択
<img src="../img/creating-Enemy/change-layer.png">

"Zombunny" に "Add Component" で "Rigidbody"を追加<br>
プロパティは以下のように設定する
<img src="../img/creating-Enemy/add-component-rigidbody.png">

同様に "Capsule Collider" を追加し，プロパティを以下のように設定
<img src="../img/creating-Enemy/add-component-capsule-collider.png">

"Sphere Collider" を追加し，以下のように設定
<img src="../img/creating-Enemy/add-component-sphere-collider.png">

"Audio Source" を追加し，以下のように設定
<img src="../img/creating-Enemy/add-component-audio-source.png">

"Nav Mesh Agent" を追加し，以下のように設定<br>
"Nav Mesh Agent" を用いて，プレイヤーを追う敵の動きを実装する<br>
"Nav Mesh Agent" についての詳細は以下のリンクへ
### [Nav Mesh Agent](https://docs.unity3d.com/ja/2017.3/Manual/nav-CreateNavMeshAgent.html)
<img src="../img/creating-Enemy/add-component-nav-mesh-agent.png">

"Window > Navigation" を開く
<img src="../img/creating-Enemy/window-navigation.png">

"Navigation" ウィンドウの "Bake" タブを選択
<img src="../img/creating-Enemy/bake-tab.png">

以下のように設定する
<img src="../img/creating-Enemy/bake-param.png">

"Bake" をクリックし，"Nav Mesh" を生成
<img src="../img/creating-Enemy/click-bake.png">

"Animation" フォルダ内で "Create > Animator Controller" を選択<br>
"EnemyAC" とする
<img src="../img/creating-Enemy/create-AC.png">

"EnemyAC" を "Zombunny" にドロップする
<img src="../img/creating-Enemy/drop-enemy-ac.png">

"EnemyAC" をダブルクリックで開き，"Assets > Models > Characters" 内の<br>
"Zombunny" から "Idle"，"Move"，"Death" を "Animator" ウィンドウにドロップ<br>
このとき，"Move" がオレンジ色でなければ，"Move" を右クリックでデフォルトステートにする
<img src="../img/creating-Enemy/drop-animations.png">

"Make Transition" で以下のように矢印を結ぶ
<img src="../img/creating-Enemy/make-transition.png">

"PlayerDead" と "Dead" という "Trigger Parameter" を作成
<img src="../img/creating-Enemy/make-animation-param.png">

"Move → Idle" の "Contidion" を "PlayerDead" に設定
<img src="../img/creating-Enemy/condition-playerdead.png">

"Any State → Death" の "Contidion" を "Dead" に設定<br>
このアニメーションウィンドウ内のステートの関係が意味するのは，<br>
**"PlayerDead" が "False" ならば，"Move" の状態でプレイヤーを追いかけ，逆に "True" になると，"Idle" 状態になる。"Dead" が "True" になると "Death" 状態になる**
<br>
ということである
<img src="../img/creating-Enemy/condition-dead.png">

"Assets > Script > Enemy" フォルダから "EnemyMovement" を "Zombunny" へドロップ
<img src="../img/creating-Enemy/drop-enemymovement.png">

"EnemyMovement" を確認してみる
```
using UnityEngine;
using System.Collections;

public class EnemyMovement : MonoBehaviour {
  // 変数の宣言
  Transform player;
  // PlayerHealth playerHealth;
  // EnemyHealth enemyHealth;
  UnityEngine.AI.NavMeshAgent nav;

  /*
   * Awake はゲームが始まる前に呼び出される
   * ここでは変数の初期化のために使用
   */
  void Awake () {
    /*
     * player という変数にゲームオブジェクトの "Player" の "transform" を入れる
     * "transform" はそのゲームオブジェクトの位置，回転，スケールを扱うことができる
     */
    player = GameObject.FindGameObjectWithTag ("Player").transform;

    // playerHealth という変数にゲームオブジェクト "Player" の "PlayerHealth" を入れる
    // playerHealth = player.GetComponent <PlayerHealth> ();

    // enmeyHealth という変数にコンポーネント "enemyHealth" を入れる
    // enemyHealth = GetComponent <EnemyHealth> ();

    // nav という変数にコンポーネント "NavMeshAgent" を入れる
    nav = GetComponent <UnityEngine.AI.NavMeshAgent> ();
  }

  /*
   * Update はフレーム毎に呼び出される
   * ここでは変数の更新のために使用
   */
  void Update () {
    /* 
     * 敵とプレイヤーの体力がどちらも0より大きいのであれば，
     * 敵が向かう座標をプレイヤーがいる位置に設定する
     * それ以外であれば，コンポーネントを無効にして更新をやめる
     */
    // if(/*enemyHealth.currentHealth > 0 &&*/ playerHealth.currentHealth > 0) {
      nav.SetDestination (player.position);
    // }
    // else {
    //   nav.enabled = false;
    // }
  }
}
```

テストプレイしてみて，プレイヤーの移動に敵がついてくるようになれば，問題ない<br>
このとき，"Player" の "Inspector" 内の "Tag" が "Player" になっているかどうかに注意
<img src="../img/creating-Enemy/test-play.png">