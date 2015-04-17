
グローバル変数
==============

libToCoNetでは、以下のグローバル変数が定義されています。

.. c:var:: tsToCoNet_AppContext sToCoNet_AppContext
   
   ToCoNetのパラメータを設定するのに使用するグローバル変数です。
   :c:func:`cbAppColdStart()` が引数 ``TRUE`` を指定して呼び出されている間のみ設定できるフィールドが
   あります。

   .. c:member:: uint32 u32AppId

      アプリケーションの識別子を32ビットで指定します。次の値は指定できません。

      * ``0x????0000``, ``0x????ffff``
      * ``0x0000ffff``, ``0xffff????``

      :c:func:`cbAppColdStart()` でのみ指定できます。

      既定値は ``0xffffffff`` です。

   .. c:member:: uint32 u32ChMask

      利用する無線チャネルの一覧をビットマスクで指定します。例えば、チャンネル12, 14のみを使用する場合、
      ``1UL<<12 | 1UL<<14`` を指定します。TWE-Strongではチャンネル26は使用できません。

      .. FIXME: これで合ってますか?

      既定値は ``0x07fff800`` (チャンネル11〜26) です。

      チャンネルの指定には、モジュール ``MOD_CHANNEL_MGR``, ``MOD_NBSCAN``, 
      ``MOD_NBSCAN_SLAVE``, ``MOD_LAYERTREE`` が必要となります(:ref:`libtoconet-modules` を参照)。

      .. TODO: MOD_NBSCAN_SLAVEってなに

   .. c:member:: uint16 u16ShortAddress

      無線モジュールの16ビットのショートアドレスを設定します。 ``0xffff`` を指定することはできません。

      既定値は無線モジュールのシリアル番号から自動生成されます。

   .. c:member:: uint8 u8Channel

      利用する **単一の** 無線チャネルを指定します。 ``u32ChMask`` で指定したチャネルの中から指定してください。

      モジュール ``MOD_CHANNEL_MGR`` を使用している場合、``u8Channel`` の設定は無視されます。

      既定値は ``18`` です。

   .. c:member:: uint8 u8CPUClk

      通常稼働時のCPUクロックを次の中から一つ選択します。

      ====== ===============
       値     CPUクロック
      ====== ===============
       0      4MHz
       1      8MHz
       2      16MHz
       3      32MHz
      ====== ===============

      :c:func:`cbAppColdStart()` 以外での変更は推奨されません。

      既定値は ``2`` (16MHz) です。

   .. c:member:: uint8 u8TxPower

      モジュールの無線出力を次の中から一つ選択します。

      ====== ===============
       値     出力
      ====== ===============
       0      -34.5db
       1      -23db
       2      -11.5db
       3      最大
      ====== ===============

      既定値は ``3`` (最大) です。

   .. c:member:: uint8 u8TxMacRetry

      MAC層でのパケットの最大再送回数を0以上8未満で指定します。

      既定値は ``3`` です。

   .. c:member:: bool_t bRxOnIdle

      ``TRUE`` を指定すると、アイドル時にも受信回路を動作させます。
      これにより、受信が可能な状態となりますが、常に電力を消費するようになります。

      ネットワーキング機能を利用する場合は、必ず ``TRUE`` を指定してください。

      既定値は ``FALSE`` (受信回路を動作させない) です。

   .. c:member:: uint8 u8HigherDataRate

      高速モードの設定を次の中から一つ選択します。

      ====== ===============
       値     速度
      ====== ===============
       0      250kbps
       1      500kbps
       2      667kbps
      ====== ===============

      TWE-Lite では高速モードに対応していないため、無視されます。

      :c:func:`cbAppColdStart()` でのみ指定できます。

      既定値は ``0`` (250kbps) です。

   .. c:member:: uint8 u8CCA_Retry

      CCAのリトライ回数を指定します。通常は変更する必要はありません。

      TWE-Lite では無視されます。

   .. c:member:: uint8 u8CCA_Level

      CCAアルゴリズムの開始レベルを指定します。通常は変更する必要はありません。

      TWE-Lite では無視されます。

   .. c:member:: uint8 u8RandMode

      ToCoNet内部で使用する乱数源を次の中から一つ選択します。

      ====== =========================================
       値     乱数源
      ====== =========================================
       0      ハードウェア乱数生成器
       1      システム経過時間
       2      モジュール ``MOD_RAND_MT``
       3      モジュール ``MOD_RAND_XOR_SHIFT``
      ====== =========================================

      ``2`` または ``3`` を使用する場合、
      モジュールを使用できるようにする宣言が必要です(:ref:`libtoconet-declare-module` を参照)。

   .. c:member:: uint16 u16TickHz

      :ref:`libtoconet-tick-timer` の周期を次の中から一つ選択します。

      ====== ===============
       値     周期
      ====== ===============
       1000   1ms
       500    2ms
       250    4ms
       200    5ms
       100    10ms
      ====== ===============

      :c:func:`cbAppColdStart()` 以外での変更は推奨されません。

   .. c:member:: bool_t bSkipBootCalib

      ``TRUE`` を指定すると、スリープからの復帰時に行われるRC 32kHzクロック (Wake Timerに使用されます) 
      のキャリブレーション処理が省略されます。



