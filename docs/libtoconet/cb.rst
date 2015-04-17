
.. _libtoconet-callbacks:

コールバック関数
================

アプリケーションは、以下に挙げるコールバック関数を定義する必要があります。

libToCoNetは、イベント発生時に以下のコールバック関数を呼び出します。

.. c:function:: void cbAppColdStart(bool_t bStart)

   電源投入・リセット後に最初にアプリケーションで呼ばれる関数です。
   
   本関数は2回呼び出されます。
   
   *  1回目は引数 ``bStart`` に ``FALSE`` が渡されます。

      このとき、アプリケーションが行える処理はモジュールの登録処理のみです(:ref:`libtoconet-declare-module` を参照)。
   
   *  2回目は引数 ``bStart`` に ``TRUE`` が渡されます。

      ここでは、アプリケーションで使用する大域変数やペリフェラルの初期化などが行えますが、
      次の処理を行う必要があります。

      * :c:data:`sToCoNet_AppContext` の初期化を行う必要があります。
        少なくともフィールド ``u32AppId`` の設定が必要ですが、それ以外のフィールドに関しては
        設定は必須ではありません。
   
.. c:function:: void cbToCoNet_vMain()

   ToCoNetのメインループで毎回呼び出される、すなわち割り込みが発生するなどして
   CPUがウェイクアップしたときに呼び出される関数です。

   割り込みの契機の一つとして :ref:`libtoconet-tick-timer` がありますので、
   本関数は少なくともTickタイマー以上の頻度で呼び出されます。

.. c:function:: void cbToCoNet_vRxEvent(tsRxDataApp *psRx)

   パケットを受信した際に呼び出されます。

.. c:function:: void cbToCoNet_vTxEvent(uint8 u8CbId, uint8 bStatus)







