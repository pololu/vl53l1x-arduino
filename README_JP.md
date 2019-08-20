# VL53L1X library for Arduino

バージョン: 1.0.1<br>
最終更新日: 2018-09-19<br>
[![Build Status](https://travis-ci.org/pololu/vl53l1x-arduino.svg?branch=master)](https://travis-ci.org/pololu/vl53l1x-arduino)<br>
[www.pololu.com](https://www.pololu.com/)

## 概要

このライブラリは ST's [VL53L1X time-of-flight distance sensor](https://www.pololu.com/product/3415)の開発を助ける Arduino IDE 用ライブラリです｡このライブラリは I&sup2;C を経由してセンサーから距離を読み取る事をより簡単にします｡

## サポートする環境

このライブラリは Arduino IDE のバージョンが 1.6.x もしくはそれ以降で動作するように設計されています｡ これらのバージョンよりも古いバージョンでは動作テストを行っていません｡ このライブラリは[Pololu A-Star controllers](https://www.pololu.com/category/149/a-star-programmable-controllers) を含む､すべての Arduino 互換ボードをサポートします｡

## はじめに

### ハードウェア

[VL53L1X carrier](https://www.pololu.com/product/3415)は Pololu 社のウェブサイトから購入することが可能です｡ 作業を始める前に[この商品のページ](https://www.pololu.com/product/3415) をよく読んでおいてください｡また VL53L1X のデータシートもよく読んでおいてください｡

Arduino と VL53L1X は以下のように接続します:

#### 動作電圧が 5V の Arduino ボードの場合

(Arduino Uno, Leonardo, Mega; Pololu A-Star 32U4 など)

    Arduino   VL53L1X 基板
    -------   -------------
         5V - VIN
        GND - GND
        SDA - SDA
        SCL - SCL

#### 動作電圧が 3.3V の Arduino ボードの場合

(Arduino Due など)

    Arduino   VL53L1X board
    -------   -------------
        3V3 - VIN
        GND - GND
        SDA - SDA
        SCL - SCL

### ソフトウェア

もし [Arduino IDE](http://www.arduino.cc/en/Main/Software)のバージョンが 1.6.2 以降であれば､ライブラリマネージャーを使ってこのライブラリをインストールすることができます｡

1. Arduino IDE で"スケッチ"のメニューを開き､"ライブラリをインクルード" を選択､その中にある"ライブラリマネージャー"をクリックします｡
2. 表示されたライブラリマネージャーで"VL53L1X"を検索します｡
3. 表示されたリストの中から VL53L1X をクリックします｡
4. "インストール"をクリックします｡

もしライブラリマネージャーが使えない場合は手動でライブラリをインストールすることもできます｡

1. [GitHub 上の最新版ライブラリ](https://github.com/pololu/vl53l1x-arduino/releases) をダウンロードし､解凍します｡
2. 解凍したフォルダの名前を "vl53l1x-arduino-master" から "VL53L1X" に変更します｡
3. "VL53L1X" フォルダを開き､ 中にある "libraries" フォルダを Arduino のスケッチブック保存フォルダへ移動します｡ スケッチブック保存フォルダの場所は Arduino IDE の "ファイル" メニューから "環境設定" を開くことで確認できます｡ もし そこに libraries フォルダが無い場合､自分自身で作成する必要があります｡
4. インストールが完了した後は Arduino IDE を再起動してください｡

## スケッチ例

スケッチ例はこのライブラリの使い方を説明しています｡ これらのスケッチには Arduino IDE の"ファイル" メニューから "スケッチ例" を選択し､その中にある"VL53L1X" を選択することで表示することができます｡ もしスケッチ例を見つけることができない場合は､ライブラリのインストールを間違えている可能性があるため､上記のインストール手順を再試行してください｡

## ST's VL53L1X API と このライブラリ

このライブラリに含まれる関数の大部分は [VL53L1X API](http://www.st.com/content/st_com/en/products/embedded-software/proximity-sensors-software/stsw-img007.html)がベースになっています｡そして､コード内に書かれたいくつかのコメントはこの API のソースコードや API のユーザーマニュアル(UM2356)､VL53L1X のデータシートから引用､意訳される形での引用がされています｡ このライブラリやコードについてのより詳しい説明やこの API からどのようにして引用されたかは VL53L1X.cpp ファイル内のコメントを参照してください｡

このライブラリは VL53L1X を Arduino 互換機で ST の API を使用する場合とは対称的に､より早く､簡単に､使い始めることができることを目的として提供されました｡ このライブラリはより合理化されたインターフェース､より少ないストレージとメモリの使用量を兼ね備えています｡ しかし､いまのところ､API で利用できる高度な機能の一部､(例:カバーガラスの下で機能するようにセンサーを調整､より小さな注目画像領域(ROI)を選択したりする 等)は実装されていません｡また､このライブラリは 堅牢なエラーチェックを行っていません｡ 高度なアプリケーションや､メモリやストレージにおける問題を気にする必要が少ない場合は､VL53L1X API を使用することを検討してみてください｡Arduino 用 ST VL53L1X API は [このリンク](https://github.com/pololu/vl53l1x-st-api-arduino)より入手可能です｡

## Library reference

- `RangingData ranging_data`<br>
- この構造体は最後に行った距離測定に関する情報が入っています｡

  内容:

  - `uint16_t range_mm`<br>
  - 最後の測定からの距離の測定値(単位:mm) (この読み取った値は`read()`の戻り値としても得られます｡)
  - `RangeStatus range_status`<br>
    最後の測定値の状態; ステータスの説明については､ VL53L1X.h の 列挙型で定義されている `RangeStatus` を参照してください｡ `VL53L1X::RangeValid` の状態は測定にあたっての問題の有無を示します｡
  - `float peak_signal_count_rate_MCPS`<br>
    Mhz 単位でのピークシグナルのカウント｡
  - `float ambient_count_rate_MCPS`<br>
    Mhz 単位での､最後に測定したカウントレート｡

- `uint8_t last_status`<br>
  このステータスは最後に I&sup2;C で通信した状態を示します｡
  この値は､[`Wire.endTransmission()` documentation](http://arduino.cc/en/Reference/WireEndTransmission) の戻り値です｡

- `VL53L1X()`<br>
  コンストラクタ関数｡

- `void setAddress(uint8_t new_addr)`<br>
  VL53L1X の I&sup2;C スレーブデバイスアドレスを引数に変更します(値は 7bit)｡

- `uint8_t getAddress()`<br>
  現在の I&sup2;C アドレスを返します｡

- `bool init(bool io_2v8 = true)`<br>
  センサの初期化と設定を行います｡
  もし､任意の引数 `io_2v8` が true であれば(指定しなかった場合はこのデフォルト値になります｡) このセンサは 2V8 モード (2.8 V I/O) で動作します｡ false であれば､ このセンサは 1V8 モードで動作します｡
  戻り値は正常に初期化が終了したかを示すブール値です｡

- `void writeReg(uint16_t reg, uint8_t value)`<br>
  指定した値をセンサの 8bit レジスタに書き込みます｡
  レジスタアドレスは定数によって定義され､定数はヘッダファイル､ VL53L1X.h 内に define によって定義された列挙型 `regAddr` です｡<br>
  使用例: `sensor.writeReg(VL53L1X::SOFT_RESET, 0x00);`

* `void writeReg16Bit(uint16_t reg, uint16_t value)`<br>
  指定した値をセンサの 16bit レジスタに書き込みます｡

* `void writeReg32Bit(uint16_t reg, uint32_t value)`<br>
  指定した値をセンサの 32bit レジスタに書き込みます｡

* `uint8_t readReg(uint16_t reg)`<br>
  センサの 8bit レジスタから読み取った値を返します｡

* `uint16_t readReg16Bit(uint16_t reg)`<br>
  センサの 16bit レジスタから読み取った値を返します｡

* `uint32_t readReg32Bit(uint16_t reg)`<br>
  センサの 32bit レジスタから読み取った値を返します｡

* `bool setDistanceMode(DistanceMode mode)`<br>
  センサの距離を測定する際のモード(`VL53L1X::Short`, `VL53L1X::Medium`, or `VL53L1X::Long`)を設定します｡
  モードにおける測定距離が短いほど外乱光の影響を受けにくいですが測定距離は短くなります｡
  詳しい情報はデータシートを参照してください｡
  戻り値は要求されたモードの設定が正常に行われたかを示すブール値です｡

- `DistanceMode getDistanceMode()`<br>
  設定されているモードを返します｡
  Returns the previously set distance mode.

- `bool setMeasurementTimingBudget(uint32_t budget_us)`<br>
  測定するタイミングを指定した値(単位:ms)に変更します｡
  ここで指定した値は､測定範囲を計測するのに許可された時間です｡
  より長いタイミング･バジェットを指定することによってより正確な測定が可能になります｡
  指定できる最小のバジェットは Short モードでは 20ms (20000 us)､Midium 及び Long モードでは 33ms となります｡
  測定距離と測定時間に関する制限の詳細はデータシートを参照してください｡
  戻り値は指定した値が有効であるかを示すブール値です｡

- `uint32_t getMeasurementTimingBudget()`<br>
  現在の測定時のタイミング･バジェットを ms で返します｡

- `void startContinuous(uint32_t period_ms)`<br>
  連続測定を開始します｡指定した間隔(単位:ms)によりセンサが測定を行う頻度を決定します｡指定したタイミングよりタイミング･バジェットが短い場合はセンサは測定を終えた後､すぐに新しい測定を開始します｡

- `void stopContinuous()`<br>
  連続測定を終了します｡

- `uint16_t read(bool blocking = true)`<br>
  連続測定が開始された後､この関数を呼び出すとミリメートル単位で最後に測定された値が返され､この値によって構造体`ranging_data`が更新されます｡ もし引数`blocking` が true(デフォルトの設定) ならば､この関数は新しいデータを測定するまで待機してから戻り値を返します｡

  もしこの関数において block を用いたくない場合､`read(false)`関数を呼び出す前に`dataReady()`関数を呼び出し､新しいデータが利用可能であるかを確認できます｡新しいデータが利用可能になる前に `read（false）`を呼び出すと 0 の値が返され、 `ranging_data.range_status`の値は`VL53L1X :: None`になり、データの更新がなかったことを示します。

* `uint16_t readRangeContinuousMillimeters(bool blocking = true)`<br>
  `read()`
  `read()` の便宜上のエイリアス

* `bool dataReady()`<br>
  センサからの新しい測定データが利用可能であるかをブール値で返します｡

* `static const char * rangeStatusToString(RangeStatus status)`<br>
  `RangeStatus`を読みやすい文字列に変換します｡

  AVR では、この関数の文字列は RAM（ダイナミックメモリ）に格納されるため、操作が簡単になりますが、200 バイト以上の RAM を使用します（AVR ベースの Arduino の多くは、約 2000 バイトの RAM しか持っていません）。 スケッチでこの関数を呼び出さない場合、このメモリ使用量を回避できます。

- `void setTimeout(uint16_t timeout)`<br>
  もしセンサの準備が完了していない場合､測定が中止されるまでのタイムアウトの時間をミリ秒単位で設定します｡
  指定する値を 0 にするとタイムアウトが無効になります｡

- `uint16_t getTimeout()`<br>
  現在設定されているタイムアウトまでの時間を返します｡

- `bool timeoutOccurred()`<br>
  `timeoutOccurred()`が最後に呼び出されてから､タイムアウトが発生したかどうかを返します｡

## バージョン履歴

- 1.0.1 (2018-09-19): Fix Arduino 101 hanging in init().
- 1.0.0 (2018-05-31): Original release.
