# 敵の出現ポイントの作成
### ここでは，敵の種類を増やし，出現ポイントを実装する
## ZomBearのプレハブ作成
"Assets > Models > Characters" フォルダ内の "ZomBear" を "Hierarchy" へドロップ
<img src="../img/Spawning-Enemies/drop-zombear.png">

"Layer" を "Shootable" に変更<br>
"Animator" の "Controller" に "EnemyAC" を入れる<br>
"ZomBear" に "Rigidbody" と "Audio Source" を追加し，以下のようにプロパティを設定する
<img src="../img/Spawning-Enemies/zombear-add-rigidbody-audio.png">

"Sphere Collider" と "Capsule Collider" を追加し，以下のようにプロパティを設定する
<img src="../img/Spawning-Enemies/zombear-add-sphere-capsule.png">

"Nav Mesh Agent" を追加し，以下のようにプロパティを設定する
<img src="../img/Spawning-Enemies/zombear-add-navmesh.png">

"Assets > Scripts > Enemy" フォルダ内の "EnemyMovement"，"EnemyHealth"，"EnemyAttack" をドロップし，以下のようにプロパティを設定する
<img src="../img/Spawning-Enemies/zombear-add-scripts.png">

"Zombear" に "HitParticles" をドロップする
<img src="../img/Spawning-Enemies/zombear-drop-hitparticle.png">

"Zombear" を "Prefabs" にドロップし，プレハブ化する<br>
"Hierarchy" の "Zombear" は削除する
<img src="../img/Spawning-Enemies/zombear-drop-prefabs.png">

## Hellephantのプレハブ作成
"Assets > Models > Characters" フォルダ内の "Hellephant" を "Hierarchy" にドロップする
<img src="../img/Spawning-Enemies/drop-hellephant.png">

"Layer" を "Shootable" に変更<br>
"HellephantHellephant" に "Rigidbody" と "Audio Source" を追加し，以下のようにプロパティを設定する
<img src="../img/Spawning-Enemies/hellephant-add-rigidbody-audio.png">

"Assets > Animation" で右クリックし，"Create > Animator Override Controller" を選択
<img src="../img/Spawning-Enemies/create-AOC.png">

"HellephantAOC" に名前を変更
<img src="../img/Spawning-Enemies/rename-AOC.png">

"Controller" の部分に "EnemyAC" をドロップ
<img src="../img/Spawning-Enemies/drop-enemyAC.png">

"Assets > Models > Characters > Hellephant" 内の "Death"，"Idle"，"Move" を以下のように入れる
<img src="../img/Spawning-Enemies/drop-state.png">

"Hellephant" の "Animator" 部分の "Controller" に先ほど作った "HellephantAOC" をドロップする
<img src="../img/Spawning-Enemies/drop-hellephantAOC.png">

"Add Component" で "Sphere Collider" と "Capsule Collider" を追加し，以下のようにプロパティを設定する
<img src="../img/Spawning-Enemies/hellephant-add-sphere-capsule.png">

"Assets > Scripts > Enemy" の 3つのスクリプトを全て "Hellephant" にドロップし，以下のようにプロパティを設定する
<img src="../img/Spawning-Enemies/hellephant-add-scripts.png">

"Hellephant" に "HitParticles" をドロップする
<img src="../img/Spawning-Enemies/hellephant-drop-particles.png">

"Hellephant" を "Prefabs" にドロップし，プレハブ化する<br>
"Hierarchy" の "Hellephant" は削除する
<img src="../img/Spawning-Enemies/drop-hellephant-prefabs.png">

## 出現ポイントの作成
"GameObject > Create Empty" で "GameObject" を作成し，名前を "EnemyManeger" とする
<img src="../img/Spawning-Enemies/create-enemy-manager.png">

"Assets > Scripts > Manager" フォルダ内の "EnemyManager" スクリプトを先ほど作った "EnemyManager" オブジェクトにドロップする
<img src="../img/Spawning-Enemies/drop-enemy-manager.png">

"EnemyManager" を確認する
```
using UnityEngine;

public class EnemyManager : MonoBehaviour {

  // 変数の宣言
  public PlayerHealth playerHealth;
  public GameObject enemy;
  public float spawnTime = 3f;
  public Transform[] spawnPoints;

  // 最初の Update が呼び出されるフレームの前で呼び出せれる
  void Start () { 
    // Spawn を spawnTime 毎に呼び出す
    InvokeRepeating ("Spawn", spawnTime, spawnTime);
  }

  // 敵を生み出す処理
  void Spawn () {
    // プレイヤーが死亡している場合は，敵を生み出さない
    if(playerHealth.currentHealth <= 0f) {
      return;
    }

    // 0 〜　spawnPoints.Length の範囲で乱数を取得する
    int spawnPointIndex = Random.Range (0, spawnPoints.Length);

    // 変数 enemy に入っているオブジェクトをクローンする
    Instantiate (enemy, spawnPoints[spawnPointIndex].position, spawnPoints[spawnPointIndex].rotation);
  }
}
```
"InvokeRepeating"，"Instantiate" についての詳細は以下のリンクへ
### [InvokeRepeating](https://docs.unity3d.com/ja/current/ScriptReference/MonoBehaviour.InvokeRepeating.html)
### [Instantiate](https://docs.unity3d.com/ja/current/ScriptReference/Object.Instantiate.html)
"GameObject > Create Empty" で "GameObject" を作成し，名前を"ZombunnySpawnPoint" とする
<img src="../img/Spawning-Enemies/create-ZombunnySpawnPoint.png">

"Inspector" 内の箱のアイコンをクリックし，青を選択
<img src="../img/Spawning-Enemies/select-blue.png">

"Transform" を以下のように設定する
<img src="../img/Spawning-Enemies/ZombunnySpawnPoint-transform.png">

"ZombunnySpawnPoint" と同様に "ZombearSpawnPoint" を作成する<br>
プロパティは以下のようにする<br>
色はピンクにする
<img src="../img/Spawning-Enemies/create-ZombearSpawnPoint.png">

"HellephantSpawnPoint" も同様に作る<br>
プロパティを以下のようにする<br>
色は黄色にする
<img src="../img/Spawning-Enemies/create-HellephantSpawnPoint.png">

"Hierarchy" の "EnmeyManager" を選択し，<br>
"Player Health" に "Player" を，"Enemy" に "ZomBunny" をそれぞれ "Assets > Prefabs" フォルダ内からドロップする
<img src="../img/Spawning-Enemies/drop-player-zombunny.png">

"Spawn Points" の "Size" を "1" にし，"Element 0" に "ZombunnySpawnPoint" をドロップする
<img src="../img/Spawning-Enemies/drop-ZombunnySpawnPoint.png">

"EnemyManager" に "EnemyManager" スクリプトを二つドロップする<br>
それぞれを先ほどの"Zombunny"と同様に，以下のように設定する<br>
敵の出現時間の間隔を "Spawn Time" で指定する
<img src="../img/Spawning-Enemies/drop-spawn-points.png">

テストプレイをする<br>
設定した出現ポイントから敵が生成されていれば問題ない
<img src="../img/Spawning-Enemies/test-play.png">
