libToCoNet
==========

libToCoNetは、アプリケーションのイベント処理、基本的な無線通信システムの管理を行うライブラリです。


.. toctree::
   :maxdepth: 3
   :hidden:

   network
   cb
   var
   evt


ライフサイクル
--------------

1. 電源が投入、あるいはリセットされると、最初にアプリケーションの :c:func:`cbAppColdStart()` が
   ``cbAppColdStart(FALSE)`` のようにして呼び出されます。
   アプリケーションはここではモジュールの宣言のみを行う必要があります 
   (:ref:`libtoconet-declare-module` を参照)。
2. 続いて、 ``cbAppColdStart(TRUE)`` が呼び出されるので、アプリケーションは必要に応じて初期化処理を行います。
3. :ref:`ユーザ定義イベント処理関数 <libtoconet-user-event>` が登録されている場合、:c:macro::`E_EVENT_START_UP` イベントが発生し、
   ユーザ定義イベント処理関数が呼び出されます。
4. スリープ・電源断まで継続的にイベント処理が行われます。イベントが発生した場合、アプリケーションで
   定義された以下の関数のうちいずれかを呼び出します(リファレンスマニュアルの :ref:`libtoconet-callbacks` を参照)。

   * :c:func:`cbToCoNet_vMain()` --- 割り込みに起因するイベントの後 (前? 最中?) に呼び出されます。
   * :c:func:`cbToCoNet_vRxEvent()` --- 無線通信によりパケットを受信した時に呼び出されます。
   * :c:func:`cbToCoNet_vTxEvent()` --- 無線通信によりパケットを送信した後、送信が完了した時に呼び出されます。
   * :c:func:`cbToCoNet_vNwkEvent()` --- MAC層やネットワーク層でイベントが発生した際に呼び出されます。
   * :c:func:`cbToCoNet_vHwEvent()` --- ペリフェラルの割り込みが発生した **後に** 呼び出されます。
   * :c:func:`cbToCoNet_u8HwInt()` --- ペリフェラルの割り込みが発生した時に呼び出されます。
   * この他にも、:ref:`ユーザ定義イベント処理関数 <libtoconet-user-event>`
     が登録されている場合は呼び出されることがあります。

4. :c:func:`ToCoNet_vSleep()` が呼び出されると、TWEモジュールはスリープ状態に入ります。

スリープ状態から復帰した場合、以下のようにして通常処理に復帰します。

1. アプリケーションの :c:func:`cbAppWarmStart()` が ``cbAppWarmStart(FALSE)`` のように呼び出されます。
   ここでは、スリープ復帰要因を取得することができますが、ペリフェラル等の使用はできません。(?)
2. アプリケーションの ``cbAppWarmStart(TRUE)`` が呼び出されます。
   ここで必要に応じて初期化処理を行います。
3. :ref:`ユーザ定義イベント処理関数 <libtoconet-user-event>` が登録されている場合、:c:macro:`E_EVENT_START_UP` イベントが発生し、
   ユーザ定義イベント処理関数が呼び出されます。
4. この後、通常のイベント処理に復帰します。

.. warning::
  イベント処理が滞ってしまうため、コールバック関数で時間の掛かる処理をしてはいけません。

.. _libtoconet-modules:

モジュールの一覧
----------------

.. warning::
  ここで使用する「モジュール」はToCoNetライブラリを機能別に分割したものを指すもので、
  TWEのハードウェアを指すものではありません。

* ``MOD_ENERGYSCAN`` --- チャネルの入力レベルを測定するのに使用するモジュールです。
* ``MOD_NBSCAN`` --- 電波到達範囲内に存在するTWEを検索するのに使用します。
* ``MOD_RAND_MT`` --- MT法を使用した高品位な疑似乱数生成を行うモジュールです。 ( **注意** このモジュールを使用する場合、ライセンス表記が必要です。 )
* ``MOD_RAND_XOR_SHIFT`` --- Xorshiftによる高速な疑似乱数生成を行うモジュールです。
* ``MOD_NWK_LAYERTREE`` --- LayerTreeネットワークを実現するモジュールです。( **注意** ``MOD_DUPCHK`` が必要です。 )
* ``MOD_NWK_LAYERTREE_MININODES`` --- ?
* ``MOD_DUPCHK`` --- パケットの重複チェックを行うモジュールです。
* ``MOD_NWK_MESSAGE_POOL`` --- メッセージプール機能を実装するモジュールです。
* ``MOD_CHANNEL_MGR`` --- チャネルアジリティを実装するモジュールです。
* 以下のモジュールはメッセージの送信キューを実装するモジュールですが、送信キューのサイズ
  によって別のモジュールに分かれていますので、一度に送信できるメッセージの個数とメモリ消費量を
  考慮して、いずれか1つを選択して下さい。
  (いずれも選択しない場合は、``MOD_TXRXQUEUE_MID`` が使用されます。)

  * ``MOD_TXRXQUEUE_SMALL`` --- 最大3個のメッセージを送信キューに含むことができます。
    384バイトのRAMを占領します。
  * ``MOD_TXRXQUEUE_MID`` --- 最大6個のメッセージを送信キューに含むことができます。
    768バイトのRAMを占領します。
  * ``MOD_TXRXQUEUE_BIG`` --- 最大20個のメッセージを送信キューに含むことができます。
    2560バイトのRAMを占領します。

.. _libtoconet-declare-module:

モジュールの宣言
----------------

ToCoNetのモジュールを使用する際は、ソースコードに以下の修正を行い、使用するモジュールを宣言する必要があります。

1. ``#include "ToCoNet.h"`` の前で、適切なプリプロセッサ定義を行う。::
    
    // MOD_RXQUEUE_BIG を使用する場合、以下のような定義を行います。
    #define ToCoNet_USE_MOD_RXQUEUE_BIG

    // MOD_CHANNEL_MGR を使用する場合、以下のような定義を行います。
    #define ToCoNet_USE_MOD_CHANNEL_MGR 

    #include "ToCoNet.h"
    #include "ToCoNet_mod_prototype.h"

2. ``cbAppColdStart(FALSE)`` が呼び出された際に、``ToCoNet_REG_MOD_ALL()`` を呼び出すようにする。::

    void cbAppColdStart(bool_t bStart) {
      if (!bStart) {
        ToCoNet_REG_MOD_ALL();
      } else {
        /* (以下略) */
      }
    }

:c:data:`sToCoNet_AppContext` の初期化
--------------------------------------

``cbAppColdStart(TRUE)`` が呼び出された際、アプリケーションはグローバル変数 :c:data:`sToCoNet_AppContext`
のフィールドの値を適切な値に初期化する必要があります。

.. _libtoconet-tick-timer:

Tickタイマー
------------

ToCoNetの初期化が完了した後、 *Tickタイマー* 
と呼ばれるタイマーがスリープ時を除いて常に作動し、周期的に割り込みが発生します。

Tickタイマーの周期は :c:data:`sToCoNet_AppContext`. ``u16TickHz`` で指定することができます。
