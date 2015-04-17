
.. _libtoconet-user-event:

ユーザ定義イベント
==================

TODO

.. TODO: ユーザ定義イベント処理関数

.. TODO: ユーザ定義イベントは割り込み禁止されたりするの？？

コールバック
------------

ユーザ定義イベント処理関数で使用するコールバックは以下の形式である必要があります。
名前やスコープに制約はありませんが、引数と返り値の型が一致している必要があります。

.. c:function:: void UserEventHandler(tsEvent *pEv, teEvent eEvent, uint32 u32evarg)

   ユーザ定義イベントが発生した際に呼び出されます。
   
   .. TODO: TODO
