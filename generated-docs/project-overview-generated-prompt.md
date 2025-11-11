Last updated: 2025-11-12


# プロジェクト概要生成プロンプト（来訪者向け）

## 生成するもの：
- projectを3行で要約する
- プロジェクトで使用されている技術スタックをカテゴリ別に整理して説明する
- プロジェクト全体のファイル階層ツリー（ディレクトリ構造を図解）
- プロジェクト全体のファイルそれぞれの説明
- プロジェクト全体の関数それぞれの説明
- プロジェクト全体の関数の呼び出し階層ツリー

## 生成しないもの：
- Issues情報（開発者向け情報のため）
- 次の一手候補（開発者向け情報のため）
- ハルシネーションしそうなもの（例、存在しない機能や計画を勝手に妄想する等）

## 出力フォーマット：
以下のMarkdown形式で出力してください：

```markdown
# Project Overview

## プロジェクト概要
[以下の形式で3行でプロジェクトを要約]
- [1行目の説明]
- [2行目の説明]
- [3行目の説明]

## 技術スタック
[使用している技術をカテゴリ別に整理して説明]
- フロントエンド: [フロントエンド技術とその説明]
- 音楽・オーディオ: [音楽・オーディオ関連技術とその説明]
- 開発ツール: [開発支援ツールとその説明]
- テスト: [テスト関連技術とその説明]
- ビルドツール: [ビルド・パース関連技術とその説明]
- 言語機能: [言語仕様・機能とその説明]
- 自動化・CI/CD: [自動化・継続的統合関連技術とその説明]
- 開発標準: [コード品質・統一ルール関連技術とその説明]

## ファイル階層ツリー
```
[プロジェクトのディレクトリ構造をツリー形式で表現]
```

## ファイル詳細説明
[各ファイルの役割と機能を詳細に説明]

## 関数詳細説明
[各関数の役割、引数、戻り値、機能を詳細に説明]

## 関数呼び出し階層ツリー
```
[関数間の呼び出し関係をツリー形式で表現]
```
```


以下のプロジェクト情報を参考にして要約を生成してください：

## プロジェクト情報
名前: 
説明: # ym2151-log-player-rust

YM2151（OPM）レジスタイベントログをJSONファイルから読み込んで、リアルタイム再生とWAVファイル出力を行うプログラムのRust実装版。

[ym2151-log-player](https://github.com/cat2151/ym2151-log-player) のRust版です。

## 状況

音は鳴っていますが不具合があります。issuesを参照ください。

pass2 json出力は、シンプル化のために削除済みです。必要になったらagentに実装させてください。

## ステータス

✅ **全フェーズ完了** - すべての機能が実装され、動作可能です。

- ✅ Phase 1: Nuked-OPM FFIバインディング
- ✅ Phase 2: JSONイベント読み込み
- ✅ Phase 3: イベント処理エンジン
- ✅ Phase 4: WAVファイル出力
- ✅ Phase 5: リアルタイムオーディオ再生
- ✅ Phase 6: メインアプリケーション統合
- ✅ Phase 7: Windows ビルドとテスト

詳細は [IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md) を参照してください。

## 機能

- ✅ JSONログファイルからイベントを読み込み
- ✅ **起動直後から即時リアルタイムオーディオ再生**（デフォルト、C言語版と同じロジック）
- ✅ **再生と同時にWAVファイルをキャプチャ保存**（output.wav）
- ✅ Nuked-OPMライブラリによる正確なYM2151エミュレーション
- ✅ 高品質サンプルレート変換（55930 Hz → 48000 Hz、再生用）

## クイックスタート / Quick Start

詳細なビルド手順については **[BUILD.md](BUILD.md)** を参照してください。

For detailed build instructions, please refer to **[BUILD.md](BUILD.md)**.

## 使い方

### 基本的な使い方（リアルタイム再生 + WAV保存）

```bash
cargo run --release sample_events.json
```

または、ビルド済みの場合：

```bash
./target/release/ym2151-log-player-rust sample_events.json
```

**動作:**
1. イベントログを読み込み
2. **即座にリアルタイム音声再生を開始**（C言語版と同じ）
3. 再生と同時にWAVファイルをキャプチャ
4. 再生完了後、`output.wav` を保存

### CI/ヘッドレス環境での実行

音声デバイスが利用できない環境（CI/ヘッドレス環境）では、ALSA設定ファイルを使用して音声出力をファイルにリダイレクトできます：

```bash
# ALSA設定ファイルを作成
cat <<'EOF' > ~/.asoundrc
pcm.!default {
  type file
  slave.pcm "null"
  file "/tmp/alsa_capture.wav"
  format "wav"
}
EOF

# 通常通りプログラムを実行
cargo run --release sample_events.json
```

この設定により、音声デバイスなしでもプログラムが正常に動作します。
音声出力は `/tmp/alsa_capture.wav` に保存され、同時に `output.wav` も生成されます。

### コマンドライン引数

```
使用方法:
  ym2151-log-player-rust <json_log_file>

例:
  ym2151-log-player-rust sample_events.json
  ym2151-log-player-rust events.json
```

### JSONイベントログファイル形式

```json
{
  "event_count": 100,
  "events": [
    {"time": 0, "addr": "0x08", "data": "0x00"},
    {"time": 2, "addr": "0x20", "data": "0xC7"}
  ]
}
```

- `event_count`: イベント総数
- `events`: イベント配列
  - `time`: サンプル時刻（絶対時刻）
  - `addr`: YM2151レジスタアドレス（16進数文字列）
  - `data`: レジスタに書き込むデータ（16進数文字列）
  - `is_data`: （オプション、読み込み時は無視されます）

**注意:** プログラムは入力イベントを自動的に2段階（アドレス書き込み→データ書き込み）に分割し、必要な遅延を挿入します。

### ビルド要件

**注意:** リアルタイムオーディオ再生（デフォルト）には音声出力デバイスが必要です。
Linux環境では、ALSA開発ライブラリのインストールが必要です：

```bash
# Ubuntu/Debian
sudo apt-get install libasound2-dev

# Fedora
sudo dnf install alsa-lib-devel
```

### リリースビルド

```bash
cargo build --release
./target/release/ym2151-log-player-rust sample_events.json
```

### テストの実行

```bash
# 標準テスト（realtime-audio機能込み）
cargo test
```

## ビルド要件

- Rust 1.70以降
- zig cc（Cコンパイラとして使用）
- （オプション）ALSA開発ライブラリ（Linux環境でrealtime-audio機能を使用する場合）

**詳細なビルド手順は [BUILD.md](BUILD.md) を参照してください。**

その他の詳細は [IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md) を参照してください。

## ライセンス

MIT License

## 利用ライブラリ

- Nuked-OPM: LGPL 2.1
- その他のRustクレート: 各クレートのライセンスに従う


依存関係:
{}

## ファイル階層ツリー
📄 .editorconfig
📄 .gitignore
📖 BUILD.md
📖 CI_TDD_GUIDE.md
📄 Cargo.lock
📄 Cargo.toml
📖 IMPLEMENTATION_PLAN.md
📖 ISSUE_72_SUMMARY.md
📄 LICENSE
📖 README.ja.md
📖 README.md
📁 _codeql_detected_source_root/
  📄 .editorconfig
  📄 .gitignore
  📖 BUILD.md
  📖 CI_TDD_GUIDE.md
  📄 Cargo.lock
  📄 Cargo.toml
  📖 IMPLEMENTATION_PLAN.md
  📖 ISSUE_72_SUMMARY.md
  📄 LICENSE
  📖 README.ja.md
  📖 README.md
  📁 _codeql_detected_source_root/
    📄 .editorconfig
    📄 .gitignore
    📖 BUILD.md
    📖 CI_TDD_GUIDE.md
    📄 Cargo.lock
    📄 Cargo.toml
    📖 IMPLEMENTATION_PLAN.md
    📖 ISSUE_72_SUMMARY.md
    📄 LICENSE
    📖 README.ja.md
    📖 README.md
    📁 _codeql_detected_source_root/
      📄 .editorconfig
      📄 .gitignore
      📖 BUILD.md
      📖 CI_TDD_GUIDE.md
      📄 Cargo.lock
      📄 Cargo.toml
      📖 IMPLEMENTATION_PLAN.md
      📖 ISSUE_72_SUMMARY.md
      📄 LICENSE
      📖 README.ja.md
      📖 README.md
      📁 _codeql_detected_source_root/
        📄 .editorconfig
        📄 .gitignore
        📖 BUILD.md
        📖 CI_TDD_GUIDE.md
        📄 Cargo.lock
        📄 Cargo.toml
        📖 IMPLEMENTATION_PLAN.md
        📖 ISSUE_72_SUMMARY.md
        📄 LICENSE
        📖 README.ja.md
        📖 README.md
        📁 _codeql_detected_source_root/
          📄 .editorconfig
          📄 .gitignore
          📖 BUILD.md
          📖 CI_TDD_GUIDE.md
          📄 Cargo.lock
          📄 Cargo.toml
          📖 IMPLEMENTATION_PLAN.md
          📖 ISSUE_72_SUMMARY.md
          📄 LICENSE
          📖 README.ja.md
          📖 README.md
          📁 _codeql_detected_source_root/
            📄 .editorconfig
            📄 .gitignore
            📖 BUILD.md
            📖 CI_TDD_GUIDE.md
            📄 Cargo.lock
            📄 Cargo.toml
            📖 IMPLEMENTATION_PLAN.md
            📖 ISSUE_72_SUMMARY.md
            📄 LICENSE
            📖 README.ja.md
            📖 README.md
            📁 _codeql_detected_source_root/
              📄 .editorconfig
              📄 .gitignore
              📖 BUILD.md
              📖 CI_TDD_GUIDE.md
              📄 Cargo.lock
              📄 Cargo.toml
              📖 IMPLEMENTATION_PLAN.md
              📖 ISSUE_72_SUMMARY.md
              📄 LICENSE
              📖 README.ja.md
              📖 README.md
              📁 _codeql_detected_source_root/
                📄 .editorconfig
                📄 .gitignore
                📖 BUILD.md
                📖 CI_TDD_GUIDE.md
                📄 Cargo.lock
                📄 Cargo.toml
                📖 IMPLEMENTATION_PLAN.md
                📖 ISSUE_72_SUMMARY.md
                📄 LICENSE
                📖 README.ja.md
                📖 README.md
                📁 _codeql_detected_source_root/
                  📄 .editorconfig
                  📄 .gitignore
                  📖 BUILD.md
                  📖 CI_TDD_GUIDE.md
                  📄 Cargo.lock
                  📄 Cargo.toml
                  📖 IMPLEMENTATION_PLAN.md
                  📖 ISSUE_72_SUMMARY.md
                  📄 LICENSE
                  📖 README.ja.md
                  📖 README.md
                  📁 _codeql_detected_source_root/
                    📄 .editorconfig
                    📄 .gitignore
                    📖 BUILD.md
                    📖 CI_TDD_GUIDE.md
                    📄 Cargo.lock
                    📄 Cargo.toml
                    📖 IMPLEMENTATION_PLAN.md
                    📖 ISSUE_72_SUMMARY.md
                    📄 LICENSE
                    📖 README.ja.md
                    📖 README.md
                    📁 _codeql_detected_source_root/
                      📄 .editorconfig
                      📄 .gitignore
                      📖 BUILD.md
                      📖 CI_TDD_GUIDE.md
                      📄 Cargo.lock
                      📄 Cargo.toml
                      📖 IMPLEMENTATION_PLAN.md
                      📖 ISSUE_72_SUMMARY.md
                      📄 LICENSE
                      📖 README.ja.md
                      📖 README.md
                      📁 _codeql_detected_source_root/
                        📄 .editorconfig
                        📄 .gitignore
                        📖 BUILD.md
                        📖 CI_TDD_GUIDE.md
                        📄 Cargo.lock
                        📄 Cargo.toml
                        📖 IMPLEMENTATION_PLAN.md
                        📖 ISSUE_72_SUMMARY.md
                        📄 LICENSE
                        📖 README.ja.md
                        📖 README.md
                        📁 _codeql_detected_source_root/
                          📄 .editorconfig
                          📄 .gitignore
                          📖 BUILD.md
                          📖 CI_TDD_GUIDE.md
                          📄 Cargo.lock
                          📄 Cargo.toml
                          📖 IMPLEMENTATION_PLAN.md
                          📖 ISSUE_72_SUMMARY.md
                          📄 LICENSE
                          📖 README.ja.md
                          📖 README.md
                          📁 _codeql_detected_source_root/
                            📄 .editorconfig
                            📄 .gitignore
                            📖 BUILD.md
                            📖 CI_TDD_GUIDE.md
                            📄 Cargo.lock
                            📄 Cargo.toml
                            📖 IMPLEMENTATION_PLAN.md
                            📖 ISSUE_72_SUMMARY.md
                            📄 LICENSE
                            📖 README.ja.md
                            📖 README.md
                            📁 _codeql_detected_source_root/
                              📄 .editorconfig
                              📄 .gitignore
                              📖 BUILD.md
                              📖 CI_TDD_GUIDE.md
                              📄 Cargo.lock
                              📄 Cargo.toml
                              📖 IMPLEMENTATION_PLAN.md
                              📖 ISSUE_72_SUMMARY.md
                              📄 LICENSE
                              📖 README.ja.md
                              📖 README.md
                              📁 _codeql_detected_source_root/
                                📄 .editorconfig
                                📄 .gitignore
                                📖 BUILD.md
                                📖 CI_TDD_GUIDE.md
                                📄 Cargo.lock
                                📄 Cargo.toml
                                📖 IMPLEMENTATION_PLAN.md
                                📖 ISSUE_72_SUMMARY.md
                                📄 LICENSE
                                📖 README.ja.md
                                📖 README.md
                                📁 _codeql_detected_source_root/
                                  📄 .editorconfig
                                  📄 .gitignore
                                  📖 BUILD.md
                                  📖 CI_TDD_GUIDE.md
                                  📄 Cargo.lock
                                  📄 Cargo.toml
                                  📖 IMPLEMENTATION_PLAN.md
                                  📖 ISSUE_72_SUMMARY.md
                                  📄 LICENSE
                                  📖 README.ja.md
                                  📖 README.md
                                  📁 _codeql_detected_source_root/
                                    📄 .editorconfig
                                    📄 .gitignore
                                    📖 BUILD.md
                                    📖 CI_TDD_GUIDE.md
                                    📄 Cargo.lock
                                    📄 Cargo.toml
                                    📖 IMPLEMENTATION_PLAN.md
                                    📖 ISSUE_72_SUMMARY.md
                                    📄 LICENSE
                                    📖 README.ja.md
                                    📖 README.md
                                    📁 _codeql_detected_source_root/
                                      📄 .editorconfig
                                      📄 .gitignore
                                      📖 BUILD.md
                                      📖 CI_TDD_GUIDE.md
                                      📄 Cargo.lock
                                      📄 Cargo.toml
                                      📖 IMPLEMENTATION_PLAN.md
                                      📖 ISSUE_72_SUMMARY.md
                                      📄 LICENSE
                                      📖 README.ja.md
                                      📖 README.md
                                      📁 _codeql_detected_source_root/
                                        📄 .editorconfig
                                        📄 .gitignore
                                        📖 BUILD.md
                                        📖 CI_TDD_GUIDE.md
                                        📄 Cargo.lock
                                        📄 Cargo.toml
                                        📖 IMPLEMENTATION_PLAN.md
                                        📖 ISSUE_72_SUMMARY.md
                                        📄 LICENSE
                                        📖 README.ja.md
                                        📖 README.md
                                        📁 _codeql_detected_source_root/
                                          📄 .editorconfig
                                          📄 .gitignore
                                          📖 BUILD.md
                                          📖 CI_TDD_GUIDE.md
                                          📄 Cargo.lock
                                          📄 Cargo.toml
                                          📖 IMPLEMENTATION_PLAN.md
                                          📖 ISSUE_72_SUMMARY.md
                                          📄 LICENSE
                                          📖 README.ja.md
                                          📖 README.md
                                          📁 _codeql_detected_source_root/
                                            📄 .editorconfig
                                            📄 .gitignore
                                            📖 BUILD.md
                                            📖 CI_TDD_GUIDE.md
                                            📄 Cargo.lock
                                            📄 Cargo.toml
                                            📖 IMPLEMENTATION_PLAN.md
                                            📖 ISSUE_72_SUMMARY.md
                                            📄 LICENSE
                                            📖 README.ja.md
                                            📖 README.md
                                            📁 _codeql_detected_source_root/
                                              📄 .editorconfig
                                              📄 .gitignore
                                              📖 BUILD.md
                                              📖 CI_TDD_GUIDE.md
                                              📄 Cargo.lock
                                              📄 Cargo.toml
                                              📖 IMPLEMENTATION_PLAN.md
                                              📖 ISSUE_72_SUMMARY.md
                                              📄 LICENSE
                                              📖 README.ja.md
                                              📖 README.md
                                              📁 _codeql_detected_source_root/
                                                📄 .editorconfig
                                                📄 .gitignore
                                                📖 BUILD.md
                                                📖 CI_TDD_GUIDE.md
                                                📄 Cargo.lock
                                                📄 Cargo.toml
                                                📖 IMPLEMENTATION_PLAN.md
                                                📖 ISSUE_72_SUMMARY.md
                                                📄 LICENSE
                                                📖 README.ja.md
                                                📖 README.md
                                                📁 _codeql_detected_source_root/
                                                  📄 .editorconfig
                                                  📄 .gitignore
                                                  📖 BUILD.md
                                                  📖 CI_TDD_GUIDE.md
                                                  📄 Cargo.lock
                                                  📄 Cargo.toml
                                                  📖 IMPLEMENTATION_PLAN.md
                                                  📖 ISSUE_72_SUMMARY.md
                                                  📄 LICENSE
                                                  📖 README.ja.md
                                                  📖 README.md
                                                  📁 _codeql_detected_source_root/
                                                    📄 .editorconfig
                                                    📄 .gitignore
                                                    📖 BUILD.md
                                                    📖 CI_TDD_GUIDE.md
                                                    📄 Cargo.lock
                                                    📄 Cargo.toml
                                                    📖 IMPLEMENTATION_PLAN.md
                                                    📖 ISSUE_72_SUMMARY.md
                                                    📄 LICENSE
                                                    📖 README.ja.md
                                                    📖 README.md
                                                    📁 _codeql_detected_source_root/
                                                      📄 .editorconfig
                                                      📄 .gitignore
                                                      📖 BUILD.md
                                                      📖 CI_TDD_GUIDE.md
                                                      📄 Cargo.lock
                                                      📄 Cargo.toml
                                                      📖 IMPLEMENTATION_PLAN.md
                                                      📖 ISSUE_72_SUMMARY.md
                                                      📄 LICENSE
                                                      📖 README.ja.md
                                                      📖 README.md
                                                      📁 _codeql_detected_source_root/
                                                        📄 .editorconfig
                                                        📄 .gitignore
                                                        📖 BUILD.md
                                                        📖 CI_TDD_GUIDE.md
                                                        📄 Cargo.lock
                                                        📄 Cargo.toml
                                                        📖 IMPLEMENTATION_PLAN.md
                                                        📖 ISSUE_72_SUMMARY.md
                                                        📄 LICENSE
                                                        📖 README.ja.md
                                                        📖 README.md
                                                        📁 _codeql_detected_source_root/
                                                          📄 .editorconfig
                                                          📄 .gitignore
                                                          📖 BUILD.md
                                                          📖 CI_TDD_GUIDE.md
                                                          📄 Cargo.lock
                                                          📄 Cargo.toml
                                                          📖 IMPLEMENTATION_PLAN.md
                                                          📖 ISSUE_72_SUMMARY.md
                                                          📄 LICENSE
                                                          📖 README.ja.md
                                                          📖 README.md
                                                          📁 _codeql_detected_source_root/
                                                            📄 .editorconfig
                                                            📄 .gitignore
                                                            📖 BUILD.md
                                                            📖 CI_TDD_GUIDE.md
                                                            📄 Cargo.lock
                                                            📄 Cargo.toml
                                                            📖 IMPLEMENTATION_PLAN.md
                                                            📖 ISSUE_72_SUMMARY.md
                                                            📄 LICENSE
                                                            📖 README.ja.md
                                                            📖 README.md
                                                            📁 _codeql_detected_source_root/
                                                              📄 .editorconfig
                                                              📄 .gitignore
                                                              📖 BUILD.md
                                                              📖 CI_TDD_GUIDE.md
                                                              📄 Cargo.lock
                                                              📄 Cargo.toml
                                                              📖 IMPLEMENTATION_PLAN.md
                                                              📖 ISSUE_72_SUMMARY.md
                                                              📄 LICENSE
                                                              📖 README.ja.md
                                                              📖 README.md
                                                              📁 _codeql_detected_source_root/
                                                                📄 .editorconfig
                                                                📄 .gitignore
                                                                📖 BUILD.md
                                                                📖 CI_TDD_GUIDE.md
                                                                📄 Cargo.lock
                                                                📄 Cargo.toml
                                                                📖 IMPLEMENTATION_PLAN.md
                                                                📖 ISSUE_72_SUMMARY.md
                                                                📄 LICENSE
                                                                📖 README.ja.md
                                                                📖 README.md
                                                                📁 _codeql_detected_source_root/
                                                                  📄 .editorconfig
                                                                  📄 .gitignore
                                                                  📖 BUILD.md
                                                                  📖 CI_TDD_GUIDE.md
                                                                  📄 Cargo.lock
                                                                  📄 Cargo.toml
                                                                  📖 IMPLEMENTATION_PLAN.md
                                                                  📖 ISSUE_72_SUMMARY.md
                                                                  📄 LICENSE
                                                                  📖 README.ja.md
                                                                  📖 README.md
                                                                  📁 _codeql_detected_source_root/
                                                                    📄 .editorconfig
                                                                    📄 .gitignore
                                                                    📖 BUILD.md
                                                                    📖 CI_TDD_GUIDE.md
                                                                    📄 Cargo.lock
                                                                    📄 Cargo.toml
                                                                    📖 IMPLEMENTATION_PLAN.md
                                                                    📖 ISSUE_72_SUMMARY.md
                                                                    📄 LICENSE
                                                                    📖 README.ja.md
                                                                    📖 README.md
                                                                    📁 _codeql_detected_source_root/
                                                                      📄 .editorconfig
                                                                      📄 .gitignore
                                                                      📖 BUILD.md
                                                                      📖 CI_TDD_GUIDE.md
                                                                      📄 Cargo.lock
                                                                      📄 Cargo.toml
                                                                      📖 IMPLEMENTATION_PLAN.md
                                                                      📖 ISSUE_72_SUMMARY.md
                                                                      📄 LICENSE
                                                                      📖 README.ja.md
                                                                      📖 README.md
                                                                      📁 _codeql_detected_source_root/
                                                                        📄 .editorconfig
                                                                        📄 .gitignore
                                                                        📖 BUILD.md
                                                                        📖 CI_TDD_GUIDE.md
                                                                        📄 Cargo.lock
                                                                        📄 Cargo.toml
                                                                        📖 IMPLEMENTATION_PLAN.md
                                                                        📖 ISSUE_72_SUMMARY.md
                                                                        📄 LICENSE
                                                                        📖 README.ja.md
                                                                        📖 README.md
                                                                        📁 _codeql_detected_source_root/
                                                                          📄 .editorconfig
                                                                          📄 .gitignore
                                                                          📖 BUILD.md
                                                                          📖 CI_TDD_GUIDE.md
                                                                          📄 Cargo.lock
                                                                          📄 Cargo.toml
                                                                          📖 IMPLEMENTATION_PLAN.md
                                                                          📖 ISSUE_72_SUMMARY.md
                                                                          📄 LICENSE
                                                                          📖 README.ja.md
                                                                          📖 README.md
                                                                          📁 _codeql_detected_source_root/
                                                                            📄 .editorconfig
                                                                            📄 .gitignore
                                                                            📖 BUILD.md
                                                                            📖 CI_TDD_GUIDE.md
                                                                            📄 Cargo.lock
                                                                            📄 Cargo.toml
                                                                            📖 IMPLEMENTATION_PLAN.md
                                                                            📖 ISSUE_72_SUMMARY.md
                                                                            📄 LICENSE
                                                                            📖 README.ja.md
                                                                            📖 README.md
                                                                            📁 _codeql_detected_source_root/
                                                                              📄 .editorconfig
                                                                              📄 .gitignore
                                                                              📖 BUILD.md
                                                                              📖 CI_TDD_GUIDE.md
                                                                              📄 Cargo.lock
                                                                              📄 Cargo.toml
                                                                              📖 IMPLEMENTATION_PLAN.md
                                                                              📖 ISSUE_72_SUMMARY.md
                                                                              📄 LICENSE
                                                                              📖 README.ja.md
                                                                              📖 README.md
                                                                              📁 _codeql_detected_source_root/
                                                                                📄 .editorconfig
                                                                                📄 .gitignore
                                                                                📖 BUILD.md
                                                                                📖 CI_TDD_GUIDE.md
                                                                                📄 Cargo.lock
                                                                                📄 Cargo.toml
                                                                                📖 IMPLEMENTATION_PLAN.md
                                                                                📖 ISSUE_72_SUMMARY.md
                                                                                📄 LICENSE
                                                                                📖 README.ja.md
                                                                                📖 README.md
                                                                                📄 _config.yml
                                                                                📄 build.rs
                                                                                📁 generated-docs/
                                                                                📄 opm.c
                                                                                📄 opm.h
                                                                                📊 output_ym2151.json
                                                                                📊 sample_events.json
                                                                                📄 setup_ci_environment.sh
                                                                                📁 src/
                                                                                  📄 audio.rs
                                                                                  📄 events.rs
                                                                                  📄 lib.rs
                                                                                  📄 main.rs
                                                                                  📄 opm.rs
                                                                                  📄 opm_ffi.rs
                                                                                  📄 player.rs
                                                                                  📄 resampler.rs
                                                                                  📄 wav_writer.rs
                                                                                📊 test_input.json
                                                                                📁 tests/
                                                                                  📄 duration_test.rs
                                                                                  📁 fixtures/
                                                                                    📊 complex.json
                                                                                    📊 simple.json
                                                                                  📄 integration_test.rs
                                                                                  📄 phase3_test.rs
                                                                                  📄 phase4_test.rs
                                                                                  📄 phase5_test.rs
                                                                                  📄 tail_generation_test.rs
                                                                              📄 _config.yml
                                                                              📄 build.rs
                                                                              📁 generated-docs/
                                                                              📄 opm.c
                                                                              📄 opm.h
                                                                              📊 output_ym2151.json
                                                                              📊 sample_events.json
                                                                              📄 setup_ci_environment.sh
                                                                              📁 src/
                                                                                📄 audio.rs
                                                                                📄 events.rs
                                                                                📄 lib.rs
                                                                                📄 main.rs
                                                                                📄 opm.rs
                                                                                📄 opm_ffi.rs
                                                                                📄 player.rs
                                                                                📄 resampler.rs
                                                                                📄 wav_writer.rs
                                                                              📊 test_input.json
                                                                              📁 tests/
                                                                                📄 duration_test.rs
                                                                                📁 fixtures/
                                                                                  📊 complex.json
                                                                                  📊 simple.json
                                                                                📄 integration_test.rs
                                                                                📄 phase3_test.rs
                                                                                📄 phase4_test.rs
                                                                                📄 phase5_test.rs
                                                                                📄 tail_generation_test.rs
                                                                            📄 _config.yml
                                                                            📄 build.rs
                                                                            📁 generated-docs/
                                                                            📄 opm.c
                                                                            📄 opm.h
                                                                            📊 output_ym2151.json
                                                                            📊 sample_events.json
                                                                            📄 setup_ci_environment.sh
                                                                            📁 src/
                                                                              📄 audio.rs
                                                                              📄 events.rs
                                                                              📄 lib.rs
                                                                              📄 main.rs
                                                                              📄 opm.rs
                                                                              📄 opm_ffi.rs
                                                                              📄 player.rs
                                                                              📄 resampler.rs
                                                                              📄 wav_writer.rs
                                                                            📊 test_input.json
                                                                            📁 tests/
                                                                              📄 duration_test.rs
                                                                              📁 fixtures/
                                                                                📊 complex.json
                                                                                📊 simple.json
                                                                              📄 integration_test.rs
                                                                              📄 phase3_test.rs
                                                                              📄 phase4_test.rs
                                                                              📄 phase5_test.rs
                                                                              📄 tail_generation_test.rs
                                                                          📄 _config.yml
                                                                          📄 build.rs
                                                                          📁 generated-docs/
                                                                          📄 opm.c
                                                                          📄 opm.h
                                                                          📊 output_ym2151.json
                                                                          📊 sample_events.json
                                                                          📄 setup_ci_environment.sh
                                                                          📁 src/
                                                                            📄 audio.rs
                                                                            📄 events.rs
                                                                            📄 lib.rs
                                                                            📄 main.rs
                                                                            📄 opm.rs
                                                                            📄 opm_ffi.rs
                                                                            📄 player.rs
                                                                            📄 resampler.rs
                                                                            📄 wav_writer.rs
                                                                          📊 test_input.json
                                                                          📁 tests/
                                                                            📄 duration_test.rs
                                                                            📁 fixtures/
                                                                              📊 complex.json
                                                                              📊 simple.json
                                                                            📄 integration_test.rs
                                                                            📄 phase3_test.rs
                                                                            📄 phase4_test.rs
                                                                            📄 phase5_test.rs
                                                                            📄 tail_generation_test.rs
                                                                        📄 _config.yml
                                                                        📄 build.rs
                                                                        📁 generated-docs/
                                                                        📄 opm.c
                                                                        📄 opm.h
                                                                        📊 output_ym2151.json
                                                                        📊 sample_events.json
                                                                        📄 setup_ci_environment.sh
                                                                        📁 src/
                                                                          📄 audio.rs
                                                                          📄 events.rs
                                                                          📄 lib.rs
                                                                          📄 main.rs
                                                                          📄 opm.rs
                                                                          📄 opm_ffi.rs
                                                                          📄 player.rs
                                                                          📄 resampler.rs
                                                                          📄 wav_writer.rs
                                                                        📊 test_input.json
                                                                        📁 tests/
                                                                          📄 duration_test.rs
                                                                          📁 fixtures/
                                                                            📊 complex.json
                                                                            📊 simple.json
                                                                          📄 integration_test.rs
                                                                          📄 phase3_test.rs
                                                                          📄 phase4_test.rs
                                                                          📄 phase5_test.rs
                                                                          📄 tail_generation_test.rs
                                                                      📄 _config.yml
                                                                      📄 build.rs
                                                                      📁 generated-docs/
                                                                      📄 opm.c
                                                                      📄 opm.h
                                                                      📊 output_ym2151.json
                                                                      📊 sample_events.json
                                                                      📄 setup_ci_environment.sh
                                                                      📁 src/
                                                                        📄 audio.rs
                                                                        📄 events.rs
                                                                        📄 lib.rs
                                                                        📄 main.rs
                                                                        📄 opm.rs
                                                                        📄 opm_ffi.rs
                                                                        📄 player.rs
                                                                        📄 resampler.rs
                                                                        📄 wav_writer.rs
                                                                      📊 test_input.json
                                                                      📁 tests/
                                                                        📄 duration_test.rs
                                                                        📁 fixtures/
                                                                          📊 complex.json
                                                                          📊 simple.json
                                                                        📄 integration_test.rs
                                                                        📄 phase3_test.rs
                                                                        📄 phase4_test.rs
                                                                        📄 phase5_test.rs
                                                                        📄 tail_generation_test.rs
                                                                    📄 _config.yml
                                                                    📄 build.rs
                                                                    📁 generated-docs/
                                                                    📄 opm.c
                                                                    📄 opm.h
                                                                    📊 output_ym2151.json
                                                                    📊 sample_events.json
                                                                    📄 setup_ci_environment.sh
                                                                    📁 src/
                                                                      📄 audio.rs
                                                                      📄 events.rs
                                                                      📄 lib.rs
                                                                      📄 main.rs
                                                                      📄 opm.rs
                                                                      📄 opm_ffi.rs
                                                                      📄 player.rs
                                                                      📄 resampler.rs
                                                                      📄 wav_writer.rs
                                                                    📊 test_input.json
                                                                    📁 tests/
                                                                      📄 duration_test.rs
                                                                      📁 fixtures/
                                                                        📊 complex.json
                                                                        📊 simple.json
                                                                      📄 integration_test.rs
                                                                      📄 phase3_test.rs
                                                                      📄 phase4_test.rs
                                                                      📄 phase5_test.rs
                                                                      📄 tail_generation_test.rs
                                                                  📄 _config.yml
                                                                  📄 build.rs
                                                                  📁 generated-docs/
                                                                  📄 opm.c
                                                                  📄 opm.h
                                                                  📊 output_ym2151.json
                                                                  📊 sample_events.json
                                                                  📄 setup_ci_environment.sh
                                                                  📁 src/
                                                                    📄 audio.rs
                                                                    📄 events.rs
                                                                    📄 lib.rs
                                                                    📄 main.rs
                                                                    📄 opm.rs
                                                                    📄 opm_ffi.rs
                                                                    📄 player.rs
                                                                    📄 resampler.rs
                                                                    📄 wav_writer.rs
                                                                  📊 test_input.json
                                                                  📁 tests/
                                                                    📄 duration_test.rs
                                                                    📁 fixtures/
                                                                      📊 complex.json
                                                                      📊 simple.json
                                                                    📄 integration_test.rs
                                                                    📄 phase3_test.rs
                                                                    📄 phase4_test.rs
                                                                    📄 phase5_test.rs
                                                                    📄 tail_generation_test.rs
                                                                📄 _config.yml
                                                                📄 build.rs
                                                                📁 generated-docs/
                                                                📄 opm.c
                                                                📄 opm.h
                                                                📊 output_ym2151.json
                                                                📊 sample_events.json
                                                                📄 setup_ci_environment.sh
                                                                📁 src/
                                                                  📄 audio.rs
                                                                  📄 events.rs
                                                                  📄 lib.rs
                                                                  📄 main.rs
                                                                  📄 opm.rs
                                                                  📄 opm_ffi.rs
                                                                  📄 player.rs
                                                                  📄 resampler.rs
                                                                  📄 wav_writer.rs
                                                                📊 test_input.json
                                                                📁 tests/
                                                                  📄 duration_test.rs
                                                                  📁 fixtures/
                                                                    📊 complex.json
                                                                    📊 simple.json
                                                                  📄 integration_test.rs
                                                                  📄 phase3_test.rs
                                                                  📄 phase4_test.rs
                                                                  📄 phase5_test.rs
                                                                  📄 tail_generation_test.rs
                                                              📄 _config.yml
                                                              📄 build.rs
                                                              📁 generated-docs/
                                                              📄 opm.c
                                                              📄 opm.h
                                                              📊 output_ym2151.json
                                                              📊 sample_events.json
                                                              📄 setup_ci_environment.sh
                                                              📁 src/
                                                                📄 audio.rs
                                                                📄 events.rs
                                                                📄 lib.rs
                                                                📄 main.rs
                                                                📄 opm.rs
                                                                📄 opm_ffi.rs
                                                                📄 player.rs
                                                                📄 resampler.rs
                                                                📄 wav_writer.rs
                                                              📊 test_input.json
                                                              📁 tests/
                                                                📄 duration_test.rs
                                                                📁 fixtures/
                                                                  📊 complex.json
                                                                  📊 simple.json
                                                                📄 integration_test.rs
                                                                📄 phase3_test.rs
                                                                📄 phase4_test.rs
                                                                📄 phase5_test.rs
                                                                📄 tail_generation_test.rs
                                                            📄 _config.yml
                                                            📄 build.rs
                                                            📁 generated-docs/
                                                            📄 opm.c
                                                            📄 opm.h
                                                            📊 output_ym2151.json
                                                            📊 sample_events.json
                                                            📄 setup_ci_environment.sh
                                                            📁 src/
                                                              📄 audio.rs
                                                              📄 events.rs
                                                              📄 lib.rs
                                                              📄 main.rs
                                                              📄 opm.rs
                                                              📄 opm_ffi.rs
                                                              📄 player.rs
                                                              📄 resampler.rs
                                                              📄 wav_writer.rs
                                                            📊 test_input.json
                                                            📁 tests/
                                                              📄 duration_test.rs
                                                              📁 fixtures/
                                                                📊 complex.json
                                                                📊 simple.json
                                                              📄 integration_test.rs
                                                              📄 phase3_test.rs
                                                              📄 phase4_test.rs
                                                              📄 phase5_test.rs
                                                              📄 tail_generation_test.rs
                                                          📄 _config.yml
                                                          📄 build.rs
                                                          📁 generated-docs/
                                                          📄 opm.c
                                                          📄 opm.h
                                                          📊 output_ym2151.json
                                                          📊 sample_events.json
                                                          📄 setup_ci_environment.sh
                                                          📁 src/
                                                            📄 audio.rs
                                                            📄 events.rs
                                                            📄 lib.rs
                                                            📄 main.rs
                                                            📄 opm.rs
                                                            📄 opm_ffi.rs
                                                            📄 player.rs
                                                            📄 resampler.rs
                                                            📄 wav_writer.rs
                                                          📊 test_input.json
                                                          📁 tests/
                                                            📄 duration_test.rs
                                                            📁 fixtures/
                                                              📊 complex.json
                                                              📊 simple.json
                                                            📄 integration_test.rs
                                                            📄 phase3_test.rs
                                                            📄 phase4_test.rs
                                                            📄 phase5_test.rs
                                                            📄 tail_generation_test.rs
                                                        📄 _config.yml
                                                        📄 build.rs
                                                        📁 generated-docs/
                                                        📄 opm.c
                                                        📄 opm.h
                                                        📊 output_ym2151.json
                                                        📊 sample_events.json
                                                        📄 setup_ci_environment.sh
                                                        📁 src/
                                                          📄 audio.rs
                                                          📄 events.rs
                                                          📄 lib.rs
                                                          📄 main.rs
                                                          📄 opm.rs
                                                          📄 opm_ffi.rs
                                                          📄 player.rs
                                                          📄 resampler.rs
                                                          📄 wav_writer.rs
                                                        📊 test_input.json
                                                        📁 tests/
                                                          📄 duration_test.rs
                                                          📁 fixtures/
                                                            📊 complex.json
                                                            📊 simple.json
                                                          📄 integration_test.rs
                                                          📄 phase3_test.rs
                                                          📄 phase4_test.rs
                                                          📄 phase5_test.rs
                                                          📄 tail_generation_test.rs
                                                      📄 _config.yml
                                                      📄 build.rs
                                                      📁 generated-docs/
                                                      📄 opm.c
                                                      📄 opm.h
                                                      📊 output_ym2151.json
                                                      📊 sample_events.json
                                                      📄 setup_ci_environment.sh
                                                      📁 src/
                                                        📄 audio.rs
                                                        📄 events.rs
                                                        📄 lib.rs
                                                        📄 main.rs
                                                        📄 opm.rs
                                                        📄 opm_ffi.rs
                                                        📄 player.rs
                                                        📄 resampler.rs
                                                        📄 wav_writer.rs
                                                      📊 test_input.json
                                                      📁 tests/
                                                        📄 duration_test.rs
                                                        📁 fixtures/
                                                          📊 complex.json
                                                          📊 simple.json
                                                        📄 integration_test.rs
                                                        📄 phase3_test.rs
                                                        📄 phase4_test.rs
                                                        📄 phase5_test.rs
                                                        📄 tail_generation_test.rs
                                                    📄 _config.yml
                                                    📄 build.rs
                                                    📁 generated-docs/
                                                    📄 opm.c
                                                    📄 opm.h
                                                    📊 output_ym2151.json
                                                    📊 sample_events.json
                                                    📄 setup_ci_environment.sh
                                                    📁 src/
                                                      📄 audio.rs
                                                      📄 events.rs
                                                      📄 lib.rs
                                                      📄 main.rs
                                                      📄 opm.rs
                                                      📄 opm_ffi.rs
                                                      📄 player.rs
                                                      📄 resampler.rs
                                                      📄 wav_writer.rs
                                                    📊 test_input.json
                                                    📁 tests/
                                                      📄 duration_test.rs
                                                      📁 fixtures/
                                                        📊 complex.json
                                                        📊 simple.json
                                                      📄 integration_test.rs
                                                      📄 phase3_test.rs
                                                      📄 phase4_test.rs
                                                      📄 phase5_test.rs
                                                      📄 tail_generation_test.rs
                                                  📄 _config.yml
                                                  📄 build.rs
                                                  📁 generated-docs/
                                                  📄 opm.c
                                                  📄 opm.h
                                                  📊 output_ym2151.json
                                                  📊 sample_events.json
                                                  📄 setup_ci_environment.sh
                                                  📁 src/
                                                    📄 audio.rs
                                                    📄 events.rs
                                                    📄 lib.rs
                                                    📄 main.rs
                                                    📄 opm.rs
                                                    📄 opm_ffi.rs
                                                    📄 player.rs
                                                    📄 resampler.rs
                                                    📄 wav_writer.rs
                                                  📊 test_input.json
                                                  📁 tests/
                                                    📄 duration_test.rs
                                                    📁 fixtures/
                                                      📊 complex.json
                                                      📊 simple.json
                                                    📄 integration_test.rs
                                                    📄 phase3_test.rs
                                                    📄 phase4_test.rs
                                                    📄 phase5_test.rs
                                                    📄 tail_generation_test.rs
                                                📄 _config.yml
                                                📄 build.rs
                                                📁 generated-docs/
                                                📄 opm.c
                                                📄 opm.h
                                                📊 output_ym2151.json
                                                📊 sample_events.json
                                                📄 setup_ci_environment.sh
                                                📁 src/
                                                  📄 audio.rs
                                                  📄 events.rs
                                                  📄 lib.rs
                                                  📄 main.rs
                                                  📄 opm.rs
                                                  📄 opm_ffi.rs
                                                  📄 player.rs
                                                  📄 resampler.rs
                                                  📄 wav_writer.rs
                                                📊 test_input.json
                                                📁 tests/
                                                  📄 duration_test.rs
                                                  📁 fixtures/
                                                    📊 complex.json
                                                    📊 simple.json
                                                  📄 integration_test.rs
                                                  📄 phase3_test.rs
                                                  📄 phase4_test.rs
                                                  📄 phase5_test.rs
                                                  📄 tail_generation_test.rs
                                              📄 _config.yml
                                              📄 build.rs
                                              📁 generated-docs/
                                              📄 opm.c
                                              📄 opm.h
                                              📊 output_ym2151.json
                                              📊 sample_events.json
                                              📄 setup_ci_environment.sh
                                              📁 src/
                                                📄 audio.rs
                                                📄 events.rs
                                                📄 lib.rs
                                                📄 main.rs
                                                📄 opm.rs
                                                📄 opm_ffi.rs
                                                📄 player.rs
                                                📄 resampler.rs
                                                📄 wav_writer.rs
                                              📊 test_input.json
                                              📁 tests/
                                                📄 duration_test.rs
                                                📁 fixtures/
                                                  📊 complex.json
                                                  📊 simple.json
                                                📄 integration_test.rs
                                                📄 phase3_test.rs
                                                📄 phase4_test.rs
                                                📄 phase5_test.rs
                                                📄 tail_generation_test.rs
                                            📄 _config.yml
                                            📄 build.rs
                                            📁 generated-docs/
                                            📄 opm.c
                                            📄 opm.h
                                            📊 output_ym2151.json
                                            📊 sample_events.json
                                            📄 setup_ci_environment.sh
                                            📁 src/
                                              📄 audio.rs
                                              📄 events.rs
                                              📄 lib.rs
                                              📄 main.rs
                                              📄 opm.rs
                                              📄 opm_ffi.rs
                                              📄 player.rs
                                              📄 resampler.rs
                                              📄 wav_writer.rs
                                            📊 test_input.json
                                            📁 tests/
                                              📄 duration_test.rs
                                              📁 fixtures/
                                                📊 complex.json
                                                📊 simple.json
                                              📄 integration_test.rs
                                              📄 phase3_test.rs
                                              📄 phase4_test.rs
                                              📄 phase5_test.rs
                                              📄 tail_generation_test.rs
                                          📄 _config.yml
                                          📄 build.rs
                                          📁 generated-docs/
                                          📄 opm.c
                                          📄 opm.h
                                          📊 output_ym2151.json
                                          📊 sample_events.json
                                          📄 setup_ci_environment.sh
                                          📁 src/
                                            📄 audio.rs
                                            📄 events.rs
                                            📄 lib.rs
                                            📄 main.rs
                                            📄 opm.rs
                                            📄 opm_ffi.rs
                                            📄 player.rs
                                            📄 resampler.rs
                                            📄 wav_writer.rs
                                          📊 test_input.json
                                          📁 tests/
                                            📄 duration_test.rs
                                            📁 fixtures/
                                              📊 complex.json
                                              📊 simple.json
                                            📄 integration_test.rs
                                            📄 phase3_test.rs
                                            📄 phase4_test.rs
                                            📄 phase5_test.rs
                                            📄 tail_generation_test.rs
                                        📄 _config.yml
                                        📄 build.rs
                                        📁 generated-docs/
                                        📄 opm.c
                                        📄 opm.h
                                        📊 output_ym2151.json
                                        📊 sample_events.json
                                        📄 setup_ci_environment.sh
                                        📁 src/
                                          📄 audio.rs
                                          📄 events.rs
                                          📄 lib.rs
                                          📄 main.rs
                                          📄 opm.rs
                                          📄 opm_ffi.rs
                                          📄 player.rs
                                          📄 resampler.rs
                                          📄 wav_writer.rs
                                        📊 test_input.json
                                        📁 tests/
                                          📄 duration_test.rs
                                          📁 fixtures/
                                            📊 complex.json
                                            📊 simple.json
                                          📄 integration_test.rs
                                          📄 phase3_test.rs
                                          📄 phase4_test.rs
                                          📄 phase5_test.rs
                                          📄 tail_generation_test.rs
                                      📄 _config.yml
                                      📄 build.rs
                                      📁 generated-docs/
                                      📄 opm.c
                                      📄 opm.h
                                      📊 output_ym2151.json
                                      📊 sample_events.json
                                      📄 setup_ci_environment.sh
                                      📁 src/
                                        📄 audio.rs
                                        📄 events.rs
                                        📄 lib.rs
                                        📄 main.rs
                                        📄 opm.rs
                                        📄 opm_ffi.rs
                                        📄 player.rs
                                        📄 resampler.rs
                                        📄 wav_writer.rs
                                      📊 test_input.json
                                      📁 tests/
                                        📄 duration_test.rs
                                        📁 fixtures/
                                          📊 complex.json
                                          📊 simple.json
                                        📄 integration_test.rs
                                        📄 phase3_test.rs
                                        📄 phase4_test.rs
                                        📄 phase5_test.rs
                                        📄 tail_generation_test.rs
                                    📄 _config.yml
                                    📄 build.rs
                                    📁 generated-docs/
                                    📄 opm.c
                                    📄 opm.h
                                    📊 output_ym2151.json
                                    📊 sample_events.json
                                    📄 setup_ci_environment.sh
                                    📁 src/
                                      📄 audio.rs
                                      📄 events.rs
                                      📄 lib.rs
                                      📄 main.rs
                                      📄 opm.rs
                                      📄 opm_ffi.rs
                                      📄 player.rs
                                      📄 resampler.rs
                                      📄 wav_writer.rs
                                    📊 test_input.json
                                    📁 tests/
                                      📄 duration_test.rs
                                      📁 fixtures/
                                        📊 complex.json
                                        📊 simple.json
                                      📄 integration_test.rs
                                      📄 phase3_test.rs
                                      📄 phase4_test.rs
                                      📄 phase5_test.rs
                                      📄 tail_generation_test.rs
                                  📄 _config.yml
                                  📄 build.rs
                                  📁 generated-docs/
                                  📄 opm.c
                                  📄 opm.h
                                  📊 output_ym2151.json
                                  📊 sample_events.json
                                  📄 setup_ci_environment.sh
                                  📁 src/
                                    📄 audio.rs
                                    📄 events.rs
                                    📄 lib.rs
                                    📄 main.rs
                                    📄 opm.rs
                                    📄 opm_ffi.rs
                                    📄 player.rs
                                    📄 resampler.rs
                                    📄 wav_writer.rs
                                  📊 test_input.json
                                  📁 tests/
                                    📄 duration_test.rs
                                    📁 fixtures/
                                      📊 complex.json
                                      📊 simple.json
                                    📄 integration_test.rs
                                    📄 phase3_test.rs
                                    📄 phase4_test.rs
                                    📄 phase5_test.rs
                                    📄 tail_generation_test.rs
                                📄 _config.yml
                                📄 build.rs
                                📁 generated-docs/
                                📄 opm.c
                                📄 opm.h
                                📊 output_ym2151.json
                                📊 sample_events.json
                                📄 setup_ci_environment.sh
                                📁 src/
                                  📄 audio.rs
                                  📄 events.rs
                                  📄 lib.rs
                                  📄 main.rs
                                  📄 opm.rs
                                  📄 opm_ffi.rs
                                  📄 player.rs
                                  📄 resampler.rs
                                  📄 wav_writer.rs
                                📊 test_input.json
                                📁 tests/
                                  📄 duration_test.rs
                                  📁 fixtures/
                                    📊 complex.json
                                    📊 simple.json
                                  📄 integration_test.rs
                                  📄 phase3_test.rs
                                  📄 phase4_test.rs
                                  📄 phase5_test.rs
                                  📄 tail_generation_test.rs
                              📄 _config.yml
                              📄 build.rs
                              📁 generated-docs/
                              📄 opm.c
                              📄 opm.h
                              📊 output_ym2151.json
                              📊 sample_events.json
                              📄 setup_ci_environment.sh
                              📁 src/
                                📄 audio.rs
                                📄 events.rs
                                📄 lib.rs
                                📄 main.rs
                                📄 opm.rs
                                📄 opm_ffi.rs
                                📄 player.rs
                                📄 resampler.rs
                                📄 wav_writer.rs
                              📊 test_input.json
                              📁 tests/
                                📄 duration_test.rs
                                📁 fixtures/
                                  📊 complex.json
                                  📊 simple.json
                                📄 integration_test.rs
                                📄 phase3_test.rs
                                📄 phase4_test.rs
                                📄 phase5_test.rs
                                📄 tail_generation_test.rs
                            📄 _config.yml
                            📄 build.rs
                            📁 generated-docs/
                            📄 opm.c
                            📄 opm.h
                            📊 output_ym2151.json
                            📊 sample_events.json
                            📄 setup_ci_environment.sh
                            📁 src/
                              📄 audio.rs
                              📄 events.rs
                              📄 lib.rs
                              📄 main.rs
                              📄 opm.rs
                              📄 opm_ffi.rs
                              📄 player.rs
                              📄 resampler.rs
                              📄 wav_writer.rs
                            📊 test_input.json
                            📁 tests/
                              📄 duration_test.rs
                              📁 fixtures/
                                📊 complex.json
                                📊 simple.json
                              📄 integration_test.rs
                              📄 phase3_test.rs
                              📄 phase4_test.rs
                              📄 phase5_test.rs
                              📄 tail_generation_test.rs
                          📄 _config.yml
                          📄 build.rs
                          📁 generated-docs/
                          📄 opm.c
                          📄 opm.h
                          📊 output_ym2151.json
                          📊 sample_events.json
                          📄 setup_ci_environment.sh
                          📁 src/
                            📄 audio.rs
                            📄 events.rs
                            📄 lib.rs
                            📄 main.rs
                            📄 opm.rs
                            📄 opm_ffi.rs
                            📄 player.rs
                            📄 resampler.rs
                            📄 wav_writer.rs
                          📊 test_input.json
                          📁 tests/
                            📄 duration_test.rs
                            📁 fixtures/
                              📊 complex.json
                              📊 simple.json
                            📄 integration_test.rs
                            📄 phase3_test.rs
                            📄 phase4_test.rs
                            📄 phase5_test.rs
                            📄 tail_generation_test.rs
                        📄 _config.yml
                        📄 build.rs
                        📁 generated-docs/
                        📄 opm.c
                        📄 opm.h
                        📊 output_ym2151.json
                        📊 sample_events.json
                        📄 setup_ci_environment.sh
                        📁 src/
                          📄 audio.rs
                          📄 events.rs
                          📄 lib.rs
                          📄 main.rs
                          📄 opm.rs
                          📄 opm_ffi.rs
                          📄 player.rs
                          📄 resampler.rs
                          📄 wav_writer.rs
                        📊 test_input.json
                        📁 tests/
                          📄 duration_test.rs
                          📁 fixtures/
                            📊 complex.json
                            📊 simple.json
                          📄 integration_test.rs
                          📄 phase3_test.rs
                          📄 phase4_test.rs
                          📄 phase5_test.rs
                          📄 tail_generation_test.rs
                      📄 _config.yml
                      📄 build.rs
                      📁 generated-docs/
                      📄 opm.c
                      📄 opm.h
                      📊 output_ym2151.json
                      📊 sample_events.json
                      📄 setup_ci_environment.sh
                      📁 src/
                        📄 audio.rs
                        📄 events.rs
                        📄 lib.rs
                        📄 main.rs
                        📄 opm.rs
                        📄 opm_ffi.rs
                        📄 player.rs
                        📄 resampler.rs
                        📄 wav_writer.rs
                      📊 test_input.json
                      📁 tests/
                        📄 duration_test.rs
                        📁 fixtures/
                          📊 complex.json
                          📊 simple.json
                        📄 integration_test.rs
                        📄 phase3_test.rs
                        📄 phase4_test.rs
                        📄 phase5_test.rs
                        📄 tail_generation_test.rs
                    📄 _config.yml
                    📄 build.rs
                    📁 generated-docs/
                    📄 opm.c
                    📄 opm.h
                    📊 output_ym2151.json
                    📊 sample_events.json
                    📄 setup_ci_environment.sh
                    📁 src/
                      📄 audio.rs
                      📄 events.rs
                      📄 lib.rs
                      📄 main.rs
                      📄 opm.rs
                      📄 opm_ffi.rs
                      📄 player.rs
                      📄 resampler.rs
                      📄 wav_writer.rs
                    📊 test_input.json
                    📁 tests/
                      📄 duration_test.rs
                      📁 fixtures/
                        📊 complex.json
                        📊 simple.json
                      📄 integration_test.rs
                      📄 phase3_test.rs
                      📄 phase4_test.rs
                      📄 phase5_test.rs
                      📄 tail_generation_test.rs
                  📄 _config.yml
                  📄 build.rs
                  📁 generated-docs/
                  📄 opm.c
                  📄 opm.h
                  📊 output_ym2151.json
                  📊 sample_events.json
                  📄 setup_ci_environment.sh
                  📁 src/
                    📄 audio.rs
                    📄 events.rs
                    📄 lib.rs
                    📄 main.rs
                    📄 opm.rs
                    📄 opm_ffi.rs
                    📄 player.rs
                    📄 resampler.rs
                    📄 wav_writer.rs
                  📊 test_input.json
                  📁 tests/
                    📄 duration_test.rs
                    📁 fixtures/
                      📊 complex.json
                      📊 simple.json
                    📄 integration_test.rs
                    📄 phase3_test.rs
                    📄 phase4_test.rs
                    📄 phase5_test.rs
                    📄 tail_generation_test.rs
                📄 _config.yml
                📄 build.rs
                📁 generated-docs/
                📄 opm.c
                📄 opm.h
                📊 output_ym2151.json
                📊 sample_events.json
                📄 setup_ci_environment.sh
                📁 src/
                  📄 audio.rs
                  📄 events.rs
                  📄 lib.rs
                  📄 main.rs
                  📄 opm.rs
                  📄 opm_ffi.rs
                  📄 player.rs
                  📄 resampler.rs
                  📄 wav_writer.rs
                📊 test_input.json
                📁 tests/
                  📄 duration_test.rs
                  📁 fixtures/
                    📊 complex.json
                    📊 simple.json
                  📄 integration_test.rs
                  📄 phase3_test.rs
                  📄 phase4_test.rs
                  📄 phase5_test.rs
                  📄 tail_generation_test.rs
              📄 _config.yml
              📄 build.rs
              📁 generated-docs/
              📄 opm.c
              📄 opm.h
              📊 output_ym2151.json
              📊 sample_events.json
              📄 setup_ci_environment.sh
              📁 src/
                📄 audio.rs
                📄 events.rs
                📄 lib.rs
                📄 main.rs
                📄 opm.rs
                📄 opm_ffi.rs
                📄 player.rs
                📄 resampler.rs
                📄 wav_writer.rs
              📊 test_input.json
              📁 tests/
                📄 duration_test.rs
                📁 fixtures/
                  📊 complex.json
                  📊 simple.json
                📄 integration_test.rs
                📄 phase3_test.rs
                📄 phase4_test.rs
                📄 phase5_test.rs
                📄 tail_generation_test.rs
            📄 _config.yml
            📄 build.rs
            📁 generated-docs/
            📄 opm.c
            📄 opm.h
            📊 output_ym2151.json
            📊 sample_events.json
            📄 setup_ci_environment.sh
            📁 src/
              📄 audio.rs
              📄 events.rs
              📄 lib.rs
              📄 main.rs
              📄 opm.rs
              📄 opm_ffi.rs
              📄 player.rs
              📄 resampler.rs
              📄 wav_writer.rs
            📊 test_input.json
            📁 tests/
              📄 duration_test.rs
              📁 fixtures/
                📊 complex.json
                📊 simple.json
              📄 integration_test.rs
              📄 phase3_test.rs
              📄 phase4_test.rs
              📄 phase5_test.rs
              📄 tail_generation_test.rs
          📄 _config.yml
          📄 build.rs
          📁 generated-docs/
          📄 opm.c
          📄 opm.h
          📊 output_ym2151.json
          📊 sample_events.json
          📄 setup_ci_environment.sh
          📁 src/
            📄 audio.rs
            📄 events.rs
            📄 lib.rs
            📄 main.rs
            📄 opm.rs
            📄 opm_ffi.rs
            📄 player.rs
            📄 resampler.rs
            📄 wav_writer.rs
          📊 test_input.json
          📁 tests/
            📄 duration_test.rs
            📁 fixtures/
              📊 complex.json
              📊 simple.json
            📄 integration_test.rs
            📄 phase3_test.rs
            📄 phase4_test.rs
            📄 phase5_test.rs
            📄 tail_generation_test.rs
        📄 _config.yml
        📄 build.rs
        📁 generated-docs/
        📄 opm.c
        📄 opm.h
        📊 output_ym2151.json
        📊 sample_events.json
        📄 setup_ci_environment.sh
        📁 src/
          📄 audio.rs
          📄 events.rs
          📄 lib.rs
          📄 main.rs
          📄 opm.rs
          📄 opm_ffi.rs
          📄 player.rs
          📄 resampler.rs
          📄 wav_writer.rs
        📊 test_input.json
        📁 tests/
          📄 duration_test.rs
          📁 fixtures/
            📊 complex.json
            📊 simple.json
          📄 integration_test.rs
          📄 phase3_test.rs
          📄 phase4_test.rs
          📄 phase5_test.rs
          📄 tail_generation_test.rs
      📄 _config.yml
      📄 build.rs
      📁 generated-docs/
      📄 opm.c
      📄 opm.h
      📊 output_ym2151.json
      📊 sample_events.json
      📄 setup_ci_environment.sh
      📁 src/
        📄 audio.rs
        📄 events.rs
        📄 lib.rs
        📄 main.rs
        📄 opm.rs
        📄 opm_ffi.rs
        📄 player.rs
        📄 resampler.rs
        📄 wav_writer.rs
      📊 test_input.json
      📁 tests/
        📄 duration_test.rs
        📁 fixtures/
          📊 complex.json
          📊 simple.json
        📄 integration_test.rs
        📄 phase3_test.rs
        📄 phase4_test.rs
        📄 phase5_test.rs
        📄 tail_generation_test.rs
    📄 _config.yml
    📄 build.rs
    📁 generated-docs/
    📄 opm.c
    📄 opm.h
    📊 output_ym2151.json
    📊 sample_events.json
    📄 setup_ci_environment.sh
    📁 src/
      📄 audio.rs
      📄 events.rs
      📄 lib.rs
      📄 main.rs
      📄 opm.rs
      📄 opm_ffi.rs
      📄 player.rs
      📄 resampler.rs
      📄 wav_writer.rs
    📊 test_input.json
    📁 tests/
      📄 duration_test.rs
      📁 fixtures/
        📊 complex.json
        📊 simple.json
      📄 integration_test.rs
      📄 phase3_test.rs
      📄 phase4_test.rs
      📄 phase5_test.rs
      📄 tail_generation_test.rs
  📄 _config.yml
  📄 build.rs
  📁 generated-docs/
  📄 opm.c
  📄 opm.h
  📊 output_ym2151.json
  📊 sample_events.json
  📄 setup_ci_environment.sh
  📁 src/
    📄 audio.rs
    📄 events.rs
    📄 lib.rs
    📄 main.rs
    📄 opm.rs
    📄 opm_ffi.rs
    📄 player.rs
    📄 resampler.rs
    📄 wav_writer.rs
  📊 test_input.json
  📁 tests/
    📄 duration_test.rs
    📁 fixtures/
      📊 complex.json
      📊 simple.json
    📄 integration_test.rs
    📄 phase3_test.rs
    📄 phase4_test.rs
    📄 phase5_test.rs
    📄 tail_generation_test.rs
📄 _config.yml
📄 build.rs
📁 generated-docs/
📄 opm.c
📄 opm.h
📊 output_ym2151.json
📊 sample_events.json
📄 setup_ci_environment.sh
📁 src/
  📄 audio.rs
  📄 events.rs
  📄 lib.rs
  📄 main.rs
  📄 opm.rs
  📄 opm_ffi.rs
  📄 player.rs
  📄 resampler.rs
  📄 wav_writer.rs
📊 test_input.json
📁 tests/
  📄 duration_test.rs
  📁 fixtures/
    📊 complex.json
    📊 simple.json
  📄 integration_test.rs
  📄 phase3_test.rs
  📄 phase4_test.rs
  📄 phase5_test.rs
  📄 tail_generation_test.rs

## ファイル詳細分析


## 関数呼び出し階層
関数呼び出し階層を分析できませんでした

## プロジェクト構造（ファイル一覧）
BUILD.md
CI_TDD_GUIDE.md
IMPLEMENTATION_PLAN.md
ISSUE_72_SUMMARY.md
README.ja.md
README.md
_codeql_detected_source_root/BUILD.md
_codeql_detected_source_root/CI_TDD_GUIDE.md
_codeql_detected_source_root/IMPLEMENTATION_PLAN.md
_codeql_detected_source_root/ISSUE_72_SUMMARY.md
_codeql_detected_source_root/README.ja.md
_codeql_detected_source_root/README.md
_codeql_detected_source_root/_codeql_detected_source_root/BUILD.md
_codeql_detected_source_root/_codeql_detected_source_root/CI_TDD_GUIDE.md
_codeql_detected_source_root/_codeql_detected_source_root/IMPLEMENTATION_PLAN.md
_codeql_detected_source_root/_codeql_detected_source_root/ISSUE_72_SUMMARY.md
_codeql_detected_source_root/_codeql_detected_source_root/README.ja.md
_codeql_detected_source_root/_codeql_detected_source_root/README.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/BUILD.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/CI_TDD_GUIDE.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/IMPLEMENTATION_PLAN.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/ISSUE_72_SUMMARY.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/README.ja.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/README.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/BUILD.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/CI_TDD_GUIDE.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/IMPLEMENTATION_PLAN.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/ISSUE_72_SUMMARY.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/README.ja.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/README.md
_codeql_detected_source_root/_codeql_detected_source_root/_codeql_detected_source_root/output_ym2151.json
_codeql_detected_source_root/_codeql_detected_source_root/output_ym2151.json
_codeql_detected_source_root/output_ym2151.json
output_ym2151.json

上記の情報を基に、プロンプトで指定された形式でプロジェクト概要を生成してください。
特に以下の点を重視してください：
- 技術スタックは各カテゴリごとに整理して説明
- ファイル階層ツリーは提供された構造をそのまま使用
- ファイルの説明は各ファイルの実際の内容と機能に基づく
- 関数の説明は実際に検出された関数の役割に基づく
- 関数呼び出し階層は実際の呼び出し関係に基づく


---
Generated at: 2025-11-12 07:06:08 JST
