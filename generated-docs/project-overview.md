Last updated: 2025-11-11

# Project Overview

## プロジェクト概要
- YM2151（OPM）レジスタイベントログをJSONファイルから読み込み、リアルタイムで音源を再生します。
- 再生と同時にWAVファイルとしてオーディオデータをキャプチャ・保存する機能を備えています。
- Rust言語で実装されており、高いパフォーマンスと信頼性でYM2151エミュレーションを提供します。

## 技術スタック
- フロントエンド: このプロジェクトはCUIアプリケーションであり、特定のGUIフロントエンド技術は使用していません。
- 音楽・オーディオ:
    - `Nuked-OPM`: YM2151（OPM）チップの精密なエミュレーションを行うC言語ライブラリです。RustからFFI（Foreign Function Interface）を通じて利用されています。
    - `cpal`: クロスプラットフォームなオーディオ入出力ライブラリで、リアルタイムオーディオ再生に利用されます。
    - `hound`: WAVフォーマットのオーディオファイルを読み書きするためのRustクレートです。
    - `ringbuf`: ロックフリーのリングバッファ実装を提供し、オーディオスレッド間のデータ連携に使用されます。
    - `resampler`: サンプルレート変換（例: 55930 Hzから48000 Hz）を実行し、エミュレータ出力とオーディオデバイスの要件を適合させます。
- 開発ツール:
    - `Rust`: プロジェクトの主要な開発言語。安全性とパフォーマンスを重視しています。
    - `cargo`: Rustプロジェクトのビルド、テスト、依存関係管理を行う公式ツールです。
    - `zig cc`: C言語コード（Nuked-OPM）をコンパイルするために使用されるコンパイラです。
    - `clap`: コマンドライン引数をパースし、アプリケーションの使いやすさを向上させるためのクレートです。
    - `serde`, `serde_json`: JSONデータのシリアライズおよびデシリアライズを効率的に行うためのクレートです。
- テスト:
    - `cargo test`: Rust標準のテストフレームワークを使用して、ユニットテストと統合テストを実行します。
- ビルドツール:
    - `build.rs`: Rustプロジェクトのコンパイル時にカスタムビルドロジック（Nuked-OPMのCコードコンパイルなど）を実行するためのスクリプトです。
- 言語機能:
    - `Rust FFI (Foreign Function Interface)`: RustコードからC言語ライブラリ（Nuked-OPM）を呼び出すために使用されます。
- 自動化・CI/CD:
    - `setup_ci_environment.sh`: 継続的インテグレーション（CI）環境をセットアップするためのシェルスクリプトです。
- 開発標準:
    - `.editorconfig`: 複数のエディタやIDE間で一貫したコーディングスタイルを維持するための設定ファイルです。

## ファイル階層ツリー
```
.
├── Cargo.toml
├── Cargo.lock
├── build.rs
├── setup_ci_environment.sh
├── opm.c
├── opm.h
├── _config.yml
├── .editorconfig
├── .gitignore
├── LICENSE
├── README.md
├── README.ja.md
├── BUILD.md
├── IMPLEMENTATION_PLAN.md
├── CI_TDD_GUIDE.md
├── ISSUE_72_SUMMARY.md
├── sample_events.json
├── output_ym2151.json
├── test_input.json
├── generated-docs/
├── src/
│   ├── main.rs
│   ├── lib.rs
│   ├── audio.rs
│   ├── events.rs
│   ├── opm.rs
│   ├── opm_ffi.rs
│   ├── player.rs
│   ├── resampler.rs
│   └── wav_writer.rs
└── tests/
    ├── integration_test.rs
    ├── duration_test.rs
    ├── phase3_test.rs
    ├── phase4_test.rs
    ├── phase5_test.rs
    ├── tail_generation_test.rs
    └── fixtures/
        ├── simple.json
        └── complex.json
```

## ファイル詳細説明
- `Cargo.toml`: Rustプロジェクトのマニフェストファイル。プロジェクト名、バージョン、依存クレート、ビルド設定などが定義されています。
- `Cargo.lock`: プロジェクトのビルドに使用されたすべての依存クレートとその正確なバージョンを記録します。再現性のあるビルドを保証します。
- `build.rs`: ビルドスクリプト。Nuked-OPMのCソースコードをコンパイルし、RustからC関数を呼び出すためのバインディングコード（`opm_ffi.rs`など）を生成します。
- `setup_ci_environment.sh`: 継続的インテグレーション（CI）環境でプロジェクトをセットアップするために使用されるシェルスクリプトです。
- `opm.c`, `opm.h`: YM2151エミュレータであるNuked-OPMライブラリのC言語ソースファイルとヘッダーファイルです。
- `_config.yml`: GitHub Pagesなどの静的サイトジェネレータで使用される設定ファイルである可能性があります。
- `.editorconfig`: コードのインデントスタイルや文字エンコーディングなど、エディタの設定をプロジェクト間で統一するためのファイルです。
- `.gitignore`: Gitが追跡しないファイルやディレクトリを指定するファイルです（例: コンパイルされたバイナリ、一時ファイル）。
- `LICENSE`: プロジェクトのライセンス情報（MIT License）を記載したファイルです。
- `README.md`, `README.ja.md`: プロジェクトの概要、ビルド方法、使い方、機能などを説明する多言語対応のドキュメントファイルです。
- `BUILD.md`: プロジェクトの詳細なビルド手順が記載されたドキュメントです。
- `IMPLEMENTATION_PLAN.md`: プロジェクトの実装計画やフェーズごとの進捗が記載されたドキュメントです。
- `CI_TDD_GUIDE.md`: CI環境におけるテスト駆動開発（TDD）に関するガイドラインが記載されたドキュメントである可能性があります。
- `ISSUE_72_SUMMARY.md`: 特定のIssue（Issue #72）に関する要約や経緯が記載されたドキュメントである可能性があります。
- `sample_events.json`: YM2151レジスタイベントログのサンプルデータを含むJSONファイルです。プログラムの動作確認に使用されます。
- `output_ym2151.json`: プロジェクトがJSON形式で出力するイベントログのサンプルまたは一時的な出力ファイルである可能性があります。
- `test_input.json`: テストスイートで使用される入力データを含むJSONファイルです。
- `generated-docs/`: ドキュメント生成ツールによって作成されたドキュメントが格納されるディレクトリです。
- `src/main.rs`: プログラムのエントリポイント。コマンドライン引数のパース、イベントログの読み込み、オーディオプレイヤーとWAVライターの初期化、再生処理の開始と制御を行います。
- `src/lib.rs`: このクレートがライブラリとして機能する場合の主要なモジュールを定義します。メインバイナリが内部モジュールとして利用することもあります。
- `src/audio.rs`: オーディオデバイスとのインタラクションを管理するモジュール。`cpal`クレートを使用してリアルタイムオーディオストリームを設定し、オーディオデータを供給します。
- `src/events.rs`: JSONファイルからのYM2151イベントログの読み込みとパース、およびイベントデータを表現する構造体を定義するモジュールです。
- `src/opm.rs`: Nuked-OPM CライブラリをRustから安全に利用するためのラッパーモジュール。YM2151エミュレータの初期化、レジスタ書き込み、サンプル生成などの機能を提供します。
- `src/opm_ffi.rs`: `build.rs`によって生成される、Nuked-OPM Cライブラリへの生のFFIバインディングを定義するモジュールです。
- `src/player.rs`: YM2151イベントログの再生ロジックをカプセル化するモジュール。イベントのスケジューリング、エミュレータへの適用、オーディオデータの生成とバッファリングを担当します。
- `src/resampler.rs`: 生成されたオーディオデータのサンプルレートを、ターゲットのオーディオデバイスが要求するレートに変換するモジュールです。
- `src/wav_writer.rs`: 生成されたオーディオデータをWAVファイル形式で保存する機能を提供するモジュールです。
- `tests/`: 統合テスト、ユニットテスト、特定の機能テストなどが格納されるディレクトリです。
- `tests/fixtures/`: テストで使用される固定の入力データファイルが格納されるディレクトリです。

## 関数詳細説明
このプロジェクトはRustで実装されており、主要なモジュールとそれらの役割から以下の主要な関数が推測されます。

- **`main()`** (in `src/main.rs`)
    - 役割: アプリケーションのエントリポイント。コマンドライン引数の解析、主要コンポーネントの初期化、イベント再生ループの開始を orchestrate します。
    - 引数: なし (または環境からコマンドライン引数を自動的に取得)
    - 戻り値: `Result<(), Box<dyn Error>>` (成功またはエラー)
    - 機能: コマンドライン引数をパースし、JSONイベントログを読み込み、`Player`と`WavWriter`を初期化し、オーディオストリームを開始して、再生が完了するまで待機します。
- **`events::load_events_from_json(path: &Path)`** (in `src/events.rs`)
    - 役割: 指定されたJSONファイルからYM2151イベントログを読み込み、パースします。
    - 引数: `path` - JSONログファイルへのパス。
    - 戻り値: `Result<Vec<Ym2151Event>, io::Error>` (パースされたイベントリストまたはエラー)
    - 機能: `serde_json` を使用して、定義された`Event`構造体にJSONデータをデシリアライズします。
- **`opm::OpmEmulator::new()`** (in `src/opm.rs`)
    - 役割: Nuked-OPMエミュレータの新しいインスタンスを初期化します。
    - 引数: なし
    - 戻り値: `Self` (初期化された`OpmEmulator`インスタンス)
    - 機能: C言語の`Nuked-OPM`ライブラリのFFIを呼び出してエミュレータをセットアップし、内部状態を準備します。
- **`opm::OpmEmulator::write_reg(addr: u8, data: u8)`** (in `src/opm.rs`)
    - 役割: YM2151レジスタにデータを書き込みます。
    - 引数: `addr` - 書き込むレジスタのアドレス、`data` - レジスタに書き込むデータ。
    - 戻り値: なし
    - 機能: YM2151エミュレータの指定されたレジスタに値を設定し、音色やパラメータを変更します。
- **`opm::OpmEmulator::generate_samples(output_buffer: &mut [f32])`** (in `src/opm.rs`)
    - 役割: YM2151エミュレータから指定されたバッファにオーディオサンプルを生成します。
    - 引数: `output_buffer` - 生成されたサンプルを書き込む`f32`型のバッファ。
    - 戻り値: なし
    - 機能: エミュレータの内部状態に基づいて、一定時間のオーディオデータを計算し、提供されたバッファにステレオサンプルとして書き込みます。
- **`player::Player::new(...)`** (in `src/player.rs`)
    - 役割: YM2151イベントを再生するためのプレイヤーインスタンスを作成します。
    - 引数: イベントリスト、WAVライター、リサンプラー、その他設定。
    - 戻り値: `Self` (初期化された`Player`インスタンス)
    - 機能: イベントキュー、Nuked-OPMエミュレータ、WAVファイル書き込み器、サンプルレート変換器を内部に保持し、再生に必要な状態を構築します。
- **`player::Player::fill_audio_buffer(output_buffer: &mut [f32], current_sample_rate: u32)`** (in `src/player.rs`)
    - 役割: オーディオ再生デバイスに供給するオーディオバッファを埋めます。この関数は通常、オーディオデバイスのコールバック関数から呼び出されます。
    - 引数: `output_buffer` - 埋めるオーディオバッファ、`current_sample_rate` - 現在のオーディオデバイスのサンプルレート。
    - 戻り値: `bool` (再生が継続するかどうか)
    - 機能: 次のイベントを処理し、Nuked-OPMからサンプルを生成し、リサンプラーを通して、最終的なオーディオバッファに書き込みます。同時にWAVファイルにもデータを供給します。
- **`audio::start_audio_stream<F>(sample_rate: u32, channels: u16, data_callback: F)`** (in `src/audio.rs`)
    - 役割: リアルタイムオーディオ再生ストリームを開始します。
    - 引数: `sample_rate` - オーディオデバイスのサンプルレート、`channels` - チャンネル数、`data_callback` - オーディオデータを生成するためのコールバック関数。
    - 戻り値: `Result<cpal::Stream, cpal::BuildStreamError>` (オーディオストリームまたはエラー)
    - 機能: `cpal`ライブラリを使用してシステムオーディオデバイスを検出し、指定されたフォーマットでオーディオストリームを開き、再生を開始します。
- **`resampler::Resampler::new(...)`** (in `src/resampler.rs`)
    - 役割: サンプルレート変換器を初期化します。
    - 引数: 入力/出力サンプルレート、バッファサイズなど。
    - 戻り値: `Self` (初期化された`Resampler`インスタンス)
    - 機能: オーディオデータを異なるサンプルレート間で効率的に変換するための内部バッファとアルゴリズムを設定します。
- **`resampler::Resampler::process(input_buffer: &[f32], output_buffer: &mut [f32])`** (in `src/resampler.rs`)
    - 役割: 入力バッファのオーディオデータを変換し、出力バッファに書き込みます。
    - 引数: `input_buffer` - 変換元のデータ、`output_buffer` - 変換結果を書き込むデータ。
    - 戻り値: なし
    - 機能: YM2151エミュレータから得られたサンプルを、オーディオデバイスの要求するサンプルレートに変換します。
- **`wav_writer::WavWriter::new(filename: &str, sample_rate: u32, channels: u16)`** (in `src/wav_writer.rs`)
    - 役割: WAVファイルへの書き込み器を初期化します。
    - 引数: `filename` - 出力ファイル名、`sample_rate` - サンプルレート、`channels` - チャンネル数。
    - 戻り値: `Result<Self, io::Error>` (初期化された`WavWriter`またはエラー)
    - 機能: 指定されたファイル名でWAVファイルを開き、ヘッダ情報を準備します。
- **`wav_writer::WavWriter::write_samples(samples: &[f32])`** (in `src/wav_writer.rs`)
    - 役割: オーディオサンプルをWAVファイルに書き込みます。
    - 引数: `samples` - 書き込むオーディオサンプルのスライス。
    - 戻り値: `Result<(), io::Error>` (成功またはエラー)
    - 機能: 提供された浮動小数点サンプルを16ビット整数に変換し、WAVファイルのデータチャンクに追記します。
- **`wav_writer::WavWriter::finalize()`** (in `src/wav_writer.rs`)
    - 役割: WAVファイルの書き込みを終了し、最終的なファイルサイズやデータチャンクサイズをヘッダに更新します。
    - 引数: なし
    - 戻り値: `Result<(), io::Error>` (成功またはエラー)
    - 機能: ファイルをクローズする前に、WAVヘッダのチャンクサイズを正確に書き込み、ファイルが正しくフォーマットされるようにします。

## 関数呼び出し階層ツリー
```
main()
├── clap::Parser::parse()  (コマンドライン引数を解析)
├── events::load_events_from_json()  (JSONイベントログを読み込み)
│   └── serde_json::from_reader()
├── player::Player::new()  (プレイヤーを初期化)
│   ├── opm::OpmEmulator::new()
│   │   └── opm_ffi::init_opm()  (Nuked-OPM Cライブラリを呼び出し)
│   ├── resampler::Resampler::new()
│   └── wav_writer::WavWriter::new()
│       └── hound::WavWriter::new()
├── audio::start_audio_stream()  (オーディオストリームを開始)
│   └── cpal::Stream::play()  (内部でデータコールバックを登録)
│       └── [data_callback closure/method (e.g., player::Player::fill_audio_buffer)]
│           ├── player::Player::process_next_event()  (次のイベントを処理)
│           │   └── opm::OpmEmulator::write_reg()
│           │       └── opm_ffi::ym2151_write()  (Nuked-OPM Cライブラリを呼び出し)
│           ├── opm::OpmEmulator::generate_samples()  (YM2151からサンプル生成)
│           │   └── opm_ffi::ym2151_update()  (Nuked-OPM Cライブラリを呼び出し)
│           ├── resampler::Resampler::process()  (サンプルレート変換)
│           ├── wav_writer::WavWriter::write_samples()  (WAVファイルに書き込み)
│           │   └── hound::WavWriter::write_sample()
│           └── ringbuf::Producer::push_slice() (オーディオバッファへ)
└── wav_writer::WavWriter::finalize()  (WAVファイルを最終化)
    └── hound::WavWriter::finalize()

---
Generated at: 2025-11-11 09:30:59 JST
