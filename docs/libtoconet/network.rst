ネットワーキング機能
====================

ToCoNetでは、以下に挙げる2種類の方法で通信を行うことができます。

* *ネットワークレス*

  * ToCoNetの中継機能を利用せず、電波の到達可能な範囲内の端末と直接パケット交換を行います。
  * 事前設定が不要です。
  * この通信方式の場合、ToCoNetでは中継機能は提供しませんが、アプリケーション側で自力で実装
    することも可能です。

* *LayerTree ネットワーク*

  * LayerTreeは、自身の中継深さ(親機からのホップ数)を指定する事で、木構造のネットワークを構成し、
    中継処理を行います。


ネットワークレス
----------------

ネットワーク機能を使用せずに通信を行う場合、以下のようなToCoNetの機能を利用することができます。

* 近隣探索 --- 電波範囲内に存在するノードのアドレスや通信電波強度を列挙する
* エナジースキャン --- 指定したチャネルのノイズレベルを返す
* 同報通信
* アドレスを指定したACK確認付きの通信
* MAC層の再送に加えアプリケーションによる再送(回数・ランダム設定可能な遅延時間・再送間隔の指定可能)

LayerTreeネットワーク機能
-------------------------

.. TODO: LayerTreeネットワーク機能
