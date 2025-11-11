Last updated: 2025-11-12

# Project Overview

## プロジェクト概要
- YM2151レジスタイベントログをJSONファイルから読み込み、高精度なYM2151エミュレーション音源を生成します。
- 生成された音源は、起動後すぐにリアルタイムでオーディオデバイスから再生されます。
- リアルタイム再生と同時に、出力音源をWAVファイルとしてキャプチャ保存する機能を提供します。

## 技術スタック
- フロントエンド: なし（コマンドラインインターフェースアプリケーション）
- 音楽・オーディオ:
    - **Nuked-OPM**: YM2151（OPM）チップのレジスタレベルでの高精度なエミュレーションを提供するC言語ライブラリ。
    - **CPAL (Cross-platform Audio Library)**: Rustでリアルタイムオーディオストリームを扱うためのクロスプラットフォームライブラリ。（推測）
    - **ALSA開発ライブラリ**: Linux環境でのオーディオ再生に必要な低レベルのサウンドシステムインターフェース。
    - **WAV クレート**: WAV形式のオーディオファイルを読み書きするためのRustライブラリ。（推測）
- 開発ツール:
    - **Rust**: プロジェクトの主要なプログラミング言語。バージョン1.70以降を使用。
    - **Zig cc**: C言語コード（Nuked-OPM）をクロスコンパイルするためのCコンパイラ。
    - **cargo**: Rustプロジェクトのビルド、テスト、依存関係管理を行う公式ツール。
- テスト:
    - **Rustのテストフレームワーク**: `cargo test`コマンドで実行される標準的な単体テストおよび統合テスト。
- ビルドツール:
    - **build.rs**: Rustのカスタムビルドスクリプトで、C言語ライブラリのコンパイルとFFIバインディング生成を自動化。
- 言語機能:
    - **JSON**: YM2151イベントログの入力ファイル形式。
    - **Foreign Function Interface (FFI)**: RustからC言語のNuked-OPMライブラリを呼び出すためのメカニズム。
- 自動化・CI/CD:
    - **setup_ci_environment.sh**: 継続的インテグレーション（CI）環境をセットアップするためのシェルスクリプト。
- 開発標準:
    - **.editorconfig**: 異なるエディタやIDE間でコードのフォーマット設定（インデントスタイルなど）を統一するためのファイル。
    - **.gitignore**: Gitリポジトリでバージョン管理しないファイルを指定。

## ファイル階層ツリー
```
.
├── .editorconfig
├── .gitignore
├── BUILD.md
├── CI_TDD_GUIDE.md
├── Cargo.lock
├── Cargo.toml
├── IMPLEMENTATION_PLAN.md
├── ISSUE_72_SUMMARY.md
├── LICENSE
├── README.ja.md
├── README.md
├── _config.yml
├── build.rs
├── generated-docs/
├── opm.c
├── opm.h
├── output_ym2151.json
├── sample_events.json
├── setup_ci_environment.sh
├── src/
│   ├── audio.rs
│   ├── events.rs
│   ├── lib.rs
│   ├── main.rs
│   ├── opm.rs
│   ├── opm_ffi.rs
│   ├── player.rs
│   ├── resampler.rs
│   └── wav_writer.rs
├── test_input.json
└── tests/
    ├── duration_test.rs
    ├── fixtures/
    │   ├── complex.json
    │   └── simple.json
    ├── integration_test.rs
    ├── phase3_test.rs
    ├── phase4_test.rs
    ├── phase5_test.rs
    └── tail_generation_test.rs
```

## ファイル詳細説明
- **`.editorconfig`**: コードエディタの設定（インデントスタイル、文字コードなど）を定義し、開発者間で一貫したコーディングスタイルを維持します。
- **`.gitignore`**: Gitが無視すべきファイルやディレクトリ（ビルド生成物、一時ファイルなど）を指定します。
- **`BUILD.md`**: プロジェクトのビルド方法、必要な依存関係、特定のプラットフォーム向けの構築手順などを詳細に説明するドキュメントです。
- **`CI_TDD_GUIDE.md`**: CI環境でのテスト駆動開発（TDD）に関するガイドラインや手順を記述したドキュメントです。
- **`Cargo.lock`**: `Cargo.toml`で指定された依存関係が実際に解決されたバージョンを正確に記録し、再現可能なビルドを保証します。
- **`Cargo.toml`**: Rustプロジェクトのメタデータ（名前、バージョンなど）と、外部クレート（ライブラリ）の依存関係、ビルド設定を定義するファイルです。
- **`IMPLEMENTATION_PLAN.md`**: プロジェクトの実装計画、フェーズごとの目標と完了状況を記述したドキュメントです。
- **`ISSUE_72_SUMMARY.md`**: 特定のIssue (#72) に関する詳細な情報や解決策をまとめたドキュメントです。
- **`LICENSE`**: プロジェクトのライセンス情報（MIT License）を記述したファイルです。
- **`README.ja.md`**: プロジェクトの概要、機能、使い方、ビルド要件などを日本語で説明するメインのドキュメントです。
- **`README.md`**: プロジェクトの概要、機能、使い方、ビルド要件などを英語で説明するメインのドキュメントです。
- **`_config.yml`**: GitHub Pagesなどのサイト設定ファイルである可能性があります。
- **`build.rs`**: Rustのビルドスクリプト。主にNuked-OPM（C言語）ライブラリをコンパイルし、Rustから利用するためのForeign Function Interface (FFI) バインディングを生成するために使用されます。
- **`generated-docs/`**: ドキュメント生成ツールによって作成されたドキュメントが格納されるディレクトリです。
- **`opm.c`**: YM2151エミュレーションを行うNuked-OPMライブラリのC言語ソースコードです。
- **`opm.h`**: `opm.c`に対応するC言語ヘッダファイルで、Nuked-OPMライブラリの関数やデータ構造の宣言を含みます。
- **`output_ym2151.json`**: プロジェクトの実行によって生成された、あるいはテストで使用されるYM2151イベントログの出力例ファイルです。
- **`sample_events.json`**: プログラムの実行例やテストで使用される、YM2151イベントログのサンプル入力ファイルです。
- **`setup_ci_environment.sh`**: 継続的インテグレーション (CI) 環境でプロジェクトをビルド・テストするために必要な依存関係のインストールや環境設定を行うシェルスクリプトです。
- **`src/audio.rs`**: リアルタイムオーディオ再生に関連するロジックを扱います。オーディオデバイスの初期化、オーディオストリームの管理、オーディオコールバックの処理などを担当します。
- **`src/events.rs`**: JSON形式のYM2151イベントログファイルを読み込み、パースしてプログラム内のデータ構造に変換するロジックを定義します。
- **`src/lib.rs`**: プロジェクトがライブラリクレートとして機能する場合のエントリポイントです。共有されるモジュールや型定義を公開します。
- **`src/main.rs`**: プログラムのエントリポイントとなるファイルです。コマンドライン引数の解析、主要なコンポーネントの初期化、イベント処理、オーディオ再生、WAVファイル出力の統合と制御を行います。
- **`src/opm.rs`**: `opm_ffi.rs`で提供される低レベルなC言語バインディングをラップし、Rustらしいより安全で使いやすいYM2151エミュレータのインターフェースを提供します。
- **`src/opm_ffi.rs`**: Nuked-OPMライブラリ（C言語）とのForeign Function Interface (FFI) バインディングを定義します。これにより、RustコードからC言語の関数を直接呼び出すことが可能になります。
- **`src/player.rs`**: YM2151イベントを時間順に処理し、Nuked-OPMエミュレータにレジスタ書き込み指示を送る中心的なロジックを実装します。オーディオサンプル生成のタイミング管理も行います。
- **`src/resampler.rs`**: YM2151エミュレータが生成する固定サンプルレート（例: 55930 Hz）のオーディオデータを、オーディオデバイスやWAVファイルの出力に必要なサンプルレート（例: 48000 Hz）に変換するロジックを実装します。
- **`src/wav_writer.rs`**: 生成されたオーディオデータを標準的なWAVファイル形式でディスクに書き込む機能を提供します。
- **`test_input.json`**: テストケースで使用される、YM2151イベントログの入力ファイルです。
- **`tests/`**: プロジェクトのテストコードを格納するディレクトリです。
    - **`tests/duration_test.rs`**: 特定の再生時間に関するテストケースを含みます。
    - **`tests/fixtures/`**: テストで使用される補助ファイル（例: `complex.json`, `simple.json`などのサンプルイベントログ）。
    - **`tests/integration_test.rs`**: プロジェクト全体の統合的な動作を検証するテストケースを含みます。
    - **`tests/phase3_test.rs`**: 実装計画のPhase 3（イベント処理エンジン）に関連するテストケースを含みます。
    - **`tests/phase4_test.rs`**: 実装計画のPhase 4（WAVファイル出力）に関連するテストケースを含みます。
    - **`tests/phase5_test.rs`**: 実装計画のPhase 5（リアルタイムオーディオ再生）に関連するテストケースを含みます。
    - **`tests/tail_generation_test.rs`**: 音源の末尾（テール）部分の生成に関するテストケースを含みます。

## 関数詳細説明
- **`main()`** (src/main.rs):
    - **役割**: プログラムのエントリポイント。
    - **引数**: なし（コマンドライン引数は環境から取得）。
    - **戻り値**: `Result<(), Box<dyn Error>>`。エラーが発生した場合はエラーオブジェクトを返す。
    - **機能**: コマンドライン引数の解析、JSONイベントログの読み込み、`Player`とオーディオストリームの初期化、リアルタイム再生とWAV出力の実行、リソースのクリーンアップを行います。
- **`events::load_events(path: &Path)`** (src/events.rs):
    - **役割**: 指定されたパスからJSON形式のYM2151イベントログを読み込み、パースする。
    - **引数**: `path: &Path` - イベントログファイルのパス。
    - **戻り値**: `Result<LogFile, Box<dyn Error>>`。パースされた`LogFile`構造体、またはエラー。
    - **機能**: ファイルをオープンし、JSONをデシリアライズしてイベントのリストを取得します。
- **`player::Player::new(events: Vec<Event>)`** (src/player.rs):
    - **役割**: YM2151イベントプレイヤーの新しいインスタンスを作成。
    - **引数**: `events: Vec<Event>` - 読み込まれたYM2151イベントのベクタ。
    - **戻り値**: `Player`インスタンス。
    - **機能**: Nuked-OPMエミュレータを初期化し、イベントをソート・前処理して、再生準備を行います。
- **`player::Player::generate_samples(&mut self, output_buffer: &mut [f32])`** (src/player.rs):
    - **役割**: 現在の再生位置から指定されたバッファサイズ分のオーディオサンプルを生成。
    - **引数**: `output_buffer: &mut [f32]` - 生成されたサンプルを書き込むバッファ。
    - **戻り値**: `usize` - 実際に生成されたサンプル数。
    - **機能**: 次のイベントを処理し、Nuked-OPMエミュレータからオーディオをミックスし、リサンプラーを通して`output_buffer`に書き込みます。
- **`player::Player::process_events(&mut self)`** (src/player.rs):
    - **役割**: 現在の再生時刻に達したYM2151イベントを処理し、エミュレータにレジスタ書き込みを行う。
    - **引数**: `&mut self`。
    - **戻り値**: なし。
    - **機能**: イベントキューを走査し、タイムスタンプが現在のエミュレータ時刻に一致または先行するイベントを見つけ、`write_register`を呼び出します。
- **`player::Player::write_register(&mut self, addr: u8, data: u8)`** (src/player.rs):
    - **役割**: YM2151エミュレータの指定されたレジスタにデータを書き込む。
    - **引数**: `addr: u8` - YM2151レジスタアドレス、`data: u8` - 書き込むデータ。
    - **戻り値**: なし。
    - **機能**: `opm::OpmEmulator`の`write`メソッドを呼び出して、YM2151エミュレータの状態を更新します。
- **`opm_ffi::opm_init(sample_rate: u32, clock: u32)`** (src/opm_ffi.rs, C FFI):
    - **役割**: Nuked-OPMエミュレータのインスタンスを初期化するC関数。
    - **引数**: `sample_rate: u32` - エミュレータが生成するオーディオのサンプルレート、`clock: u32` - YM2151のマスタークロック周波数。
    - **戻り値**: `*mut c_void` - 初期化されたエミュレータインスタンスへのポインタ。
    - **機能**: C言語のNuked-OPMライブラリが内部でエミュレータ構造体を割り当て、初期設定を行います。
- **`opm_ffi::opm_write(opm: *mut c_void, addr: u8, data: u8)`** (src/opm_ffi.rs, C FFI):
    - **役割**: YM2151エミュレータのレジスタにデータを書き込むC関数。
    - **引数**: `opm: *mut c_void` - エミュレータインスタンスへのポインタ、`addr: u8` - レジスタアドレス、`data: u8` - データ。
    - **戻り値**: なし。
    - **機能**: YM2151エミュレータの内部状態を指定されたレジスタ書き込みで更新します。
- **`opm_ffi::opm_mix(opm: *mut c_void, buffer: *mut f32, samples: i32)`** (src/opm_ffi.rs, C FFI):
    - **役割**: YM2151エミュレータからオーディオサンプルをミックスして取得するC関数。
    - **引数**: `opm: *mut c_void` - エミュレータインスタンスへのポインタ、`buffer: *mut f32` - サンプルを書き込むバッファへのポインタ、`samples: i32` - 生成するサンプル数。
    - **戻り値**: なし。
    - **機能**: Nuked-OPMエミュレータがサウンドを生成し、提供されたバッファにステレオ浮動小数点サンプルとして書き込みます。
- **`opm::OpmEmulator::new()`** (src/opm.rs):
    - **役割**: `opm_ffi`を介してNuked-OPMエミュレータを安全に初期化するRustラッパー。
    - **引数**: なし。
    - **戻り値**: `Self`インスタンス。
    - **機能**: 内部で`opm_ffi::opm_init`を呼び出し、そのポインタを安全に管理します。
- **`opm::OpmEmulator::write(&mut self, addr: u8, data: u8)`** (src/opm.rs):
    - **役割**: YM2151レジスタにデータを書き込むRustインターフェース。
    - **引数**: `addr: u8`, `data: u8`。
    - **戻り値**: なし。
    - **機能**: 内部で`opm_ffi::opm_write`を呼び出し、C側のエミュレータにコマンドを送ります。
- **`opm::OpmEmulator::mix(&mut self, buffer: &mut [f32])`** (src/opm.rs):
    - **役割**: YM2151エミュレータからオーディオサンプルを取得するRustインターフェース。
    - **引数**: `buffer: &mut [f32]` - サンプルを書き込むバッファ。
    - **戻り値**: なし。
    - **機能**: 内部で`opm_ffi::opm_mix`を呼び出し、C側のエミュレータからオーディオデータを取得します。
- **`audio::start_audio_stream(player: Arc<Mutex<Player>>, sample_rate: u32, wav_writer: Arc<Mutex<Option<WavWriter>>>)`** (src/audio.rs):
    - **役割**: オーディオデバイスを初期化し、リアルタイム再生ストリームを開始する。
    - **引数**: `player`: プレイヤーロジックへの共有参照、`sample_rate`: オーディオ出力サンプルレート、`wav_writer`: WAV書き込みロジックへの共有参照（オプション）。
    - **戻り値**: `Result<cpal::Stream, cpal::BuildStreamError>`。開始されたオーディオストリーム、またはエラー。
    - **機能**: `cpal`ライブラリを使用してオーディオデバイスを選択し、コールバック関数を設定してストリームを開始します。
- **`audio::audio_callback(player_mutex: &Arc<Mutex<Player>>, wav_writer_mutex: &Arc<Mutex<Option<WavWriter>>>)`** (src/audio.rs, クロージャまたは関数ポインタ):
    - **役割**: オーディオデバイスからの要求に応じて、オーディオデータを生成してバッファに書き込むコールバック。
    - **引数**: `player_mutex`: プレイヤーへのミューテックス保護された参照、`wav_writer_mutex`: WAVライターへのミューテックス保護された参照。
    - **戻り値**: `impl FnMut(&mut [f32], &cpal::OutputCallbackInfo)` - `cpal`が必要とするコールバック関数シグネチャ。
    - **機能**: `player::Player::generate_samples`を呼び出してサンプルを生成し、必要に応じて`wav_writer::WavWriter::write_samples`に書き込みます。
- **`resampler::Resampler::new(input_rate: u32, output_rate: u32)`** (src/resampler.rs):
    - **役割**: オーディオリサンプラーのインスタンスを作成。
    - **引数**: `input_rate: u32` - 入力サンプルレート、`output_rate: u32` - 出力サンプルレート。
    - **戻り値**: `Resampler`インスタンス。
    - **機能**: サンプルレート変換に必要な内部状態を初期化します。
- **`resampler::Resampler::process_samples(&mut self, input: &[f32], output: &mut [f32])`** (src/resampler.rs):
    - **役割**: 入力サンプルをリサンプリングし、出力バッファに書き込む。
    - **引数**: `input: &[f32]` - 入力サンプル、`output: &mut [f32]` - リサンプリングされたサンプルを書き込むバッファ。
    - **戻り値**: `usize` - `output`に書き込まれたサンプル数。
    - **機能**: 高品質なアルゴリズムを用いてサンプルレート変換を実行します。
- **`wav_writer::WavWriter::new(path: &Path, sample_rate: u32, channels: u16)`** (src/wav_writer.rs):
    - **役割**: 新しいWAVファイルを作成し、書き込み準備をする。
    - **引数**: `path: &Path` - 出力WAVファイルのパス、`sample_rate: u32` - サンプルレート、`channels: u16` - チャンネル数。
    - **戻り値**: `Result<WavWriter, io::Error>`。初期化された`WavWriter`インスタンス、またはエラー。
    - **機能**: 指定されたパスにWAVファイルを作成し、ヘッダ情報を書き込みます。
- **`wav_writer::WavWriter::write_samples(&mut self, samples: &[f32])`** (src/wav_writer.rs):
    - **役割**: オーディオサンプルをWAVファイルに書き込む。
    - **引数**: `samples: &[f32]` - 書き込むオーディオサンプル。
    - **戻り値**: `Result<(), io::Error>`。書き込みが成功した場合は`Ok(())`、エラーの場合は`Err`。
    - **機能**: 提供された浮動小数点サンプルを適切なWAV形式（例: 16bit PCM）に変換してファイルに追記します。
- **`wav_writer::WavWriter::finalize(&mut self)`** (src/wav_writer.rs):
    - **役割**: WAVファイルの書き込みを終了し、ヘッダを更新してファイルを閉じる。
    - **引数**: `&mut self`。
    - **戻り値**: `Result<(), io::Error>`。
    - **機能**: 書き込まれたデータ量に基づいてWAVファイルのヘッダ情報を最終的に更新し、ファイルを閉じます。

## 関数呼び出し階層ツリー
```
main()
├── events::load_events()
├── player::Player::new()
│   └── opm::OpmEmulator::new()
│       └── opm_ffi::opm_init()
├── wav_writer::WavWriter::new() (オプション、WAV出力が有効な場合)
├── audio::start_audio_stream()
│   └── (cpal内部スレッド)
│       └── audio::audio_callback() (リアルタイムで繰り返し呼ばれる)
│           ├── player::Player::generate_samples()
│           │   ├── player::Player::process_events()
│           │   │   └── player::Player::write_register()
│           │   │       └── opm::OpmEmulator::write()
│           │   │           └── opm_ffi::opm_write()
│           │   ├── opm::OpmEmulator::mix()
│           │   │   └── opm_ffi::opm_mix()
│           │   └── resampler::Resampler::process_samples()
│           └── wav_writer::WavWriter::write_samples() (オプション、WAV出力が有効な場合)
└── wav_writer::WavWriter::finalize() (再生終了後、オプション)

---
Generated at: 2025-11-12 07:06:50 JST
