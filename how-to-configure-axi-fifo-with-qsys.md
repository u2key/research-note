# Qsys（Platform Designer）での自作UART/FIFOモジュール設定ガイド

Intel（旧Altera）の **Platform Designer（旧Qsys）** を使って、独自で作成したUART/FIFOモジュールを組み込み、Armプロセッサ（HPS: Hard Processor System）からアクセスできるようにするための設定手順です。

---

## ステップ 1: 自作モジュールをQsysのコンポーネントにする（Component Editor）

まずは、作成したVerilogのラッパーモジュールを、Qsysが認識できる「部品（コンポーネント）」として登録します。

1. **Component Editorの起動**
   * QuartusからPlatform Designer（Qsys）を開きます。
   * **[Component Library]** タブの上部にある **[New Component...]** をクリックします。

2. **[Files] タブの設定**
   * `Synthesis Files` に、作成したVerilogファイル（AXI-LiteやAvalon-MMのラッパー付きUARTモジュール）を追加します。
   * **[Analyze Synthesis Files]** をクリックし、エラーがないかチェックします。

3. **[Signals & Interfaces] タブの設定**
   * 解析されたポート（`clk`, `rst_n`, `address`, `read`, `readdata`, `irq` など）が、正しくバスの規格にマッピングされているか確認します。
   * **対応表（例）:**
     * クロックピン ➔ `Clock Input`
     * リセットピン ➔ `Reset Input`
     * データバスピン ➔ `Avalon Memory Mapped Slave` または `AXI4Lite Slave`
     * 割り込みピン ➔ `Interrupt Sender`

4. **保存**
   * **[Finish]** を押して保存します。これでComponent Libraryに自作UARTが表示されるようになります。

---

## ステップ 2: HPS（Arm）と自作UARTを画面に配置する

Qsysのメイン画面（System View）にパーツを並べます。

1. **HPSの配置**
   * Component Libraryから **[Hard Processor System]** を探してダブルクリックし、システムに追加します。
2. **自作UARTの配置**
   * 先ほど登録した自作UARTコンポーネントを探してダブルクリックし、システムに追加します。

---

## ステップ 3: 線を結ぶ（コネクションの接続）

画面上に並んだ部品の「白丸（○）」をマウスでクリックし、配線を接続します。以下の4つの接続を必ず行ってください。

| 接続元（HPS / 共通） | 接続先（自作UART） | 役割 |
| :--- | :--- | :--- |
| **`h2f_axi_clock`** (または外部クロック) | **`clock`** | クロックの同期（同じ波形で動作） |
| **`h2f_axi_reset`** (または外部リセット) | **`reset`** | リセット信号の同期 |
| **`h2f_lw_axi_master`**（※1） | **`altera_axi_slave`** (またはAvalon Slave) | ArmからUARTレジスタを読み書きする主導線 |
| **`f2h_irq0`** (HPSの割り込み入力) | **`interrupt_sender`** | FIFOにデータが来たらArmに知らせる割り込み線 |

> 💡 **※1: `h2f_lw_axi_master` とは？**
> 「HPS-to-FPGA Lightweight AXI Master」の略です。周辺回路（UARTやGPIOなど）のレジスタ制御用に用意された、低速だけど手軽に使えるSoC・FPGA間の専用バスです。自作UARTはここに繋ぐのがセオリーです。

---

## ステップ 4: アドレスの割り当てと生成（Generate）

1. **ベースアドレスの確定**
   * 画面右側の **[Base Address]** 列を確認します。
   * 自作UARTの行のベースアドレスをダブルクリックし、重複しないアドレス（例：`0x0000_0000` など）を設定します。
   * ※これはLightweight AXI全体のベースアドレス `0xFF20_0000` からのオフセット値になります。

2. **システムの生成**
   * 画面右下の **[Generate HDL...]** をクリックします。
   * 出力形式（Verilog）を選んで **[Generate]** を押します。

---

## 🚀 設定完了後の流れ

Qsysでの作業が終わると、システム全体のVerilogファイル（`.qsys` または `.ip`）が自動生成されます。これをQuartusのトップモジュールでインスタンス化してコンパイルすれば、ハードウェア側は完成です。

また、Qsysはアドレスマップが書かれたC言語用のヘッダファイル（**`hps_0.h`** など）も自動生成します。

```c
// 自動生成されるマクロのイメージ
#define UART_BASE 0x00000000 // Qsysで設定したアドレス
