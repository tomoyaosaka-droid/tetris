# Tetris

ブラウザで動くテトリスゲームです。HTML5 Canvas + Vanilla JS で実装しており、外部ライブラリ不要でそのまま動きます。

## デモ

`tetris.html` をブラウザで開くだけでプレイできます。

## 機能

- **7-bag ランダマイザー** — TetrisWiki 準拠。同じピースが連続で出にくい
- **SRS 回転 + Wall Kick** — I 専用・JLSTZ 専用のキックテーブルを完全実装
- **ゴーストピース** — 落下先を半透明で表示
- **ホールドピース** — C / Shift で現在のピースを保持
- **Next キュー** — 次の 3 個を表示
- **スコアリング** — ライン数 × レベル倍率、コンボボーナス、Back-to-Back Tetris (+50%)
- **Lock Delay** — 500ms、最大 15 回リセット
- **DAS (Delayed Auto Shift)** — 167ms 初回、33ms 間隔の自動シフト
- **レベル進行** — 10 ライン毎にレベルアップ (最大 20)、Guideline 速度式
- **レスポンシブ** — ビューポートに応じてブロックサイズ自動調整
- **モバイル対応** — タッチボタン付き

## 操作方法

| キー | 操作 |
|------|------|
| `← →` | 移動 |
| `↑` / `X` | 時計回りに回転 |
| `Z` | 反時計回りに回転 |
| `↓` | ソフトドロップ |
| `Space` | ハードドロップ |
| `C` / `Shift` | ホールド |
| `P` / `Esc` | ポーズ |

## 技術スタック

- HTML5 Canvas (2D)
- Vanilla JavaScript (ES2020)
- CSS3 (Grid / Flexbox)
- 外部依存なし・単一ファイル

## 参考

- [Super Rotation System — TetrisWiki](https://tetris.wiki/Super_Rotation_System)
- [Random Generator — TetrisWiki](https://tetris.wiki/Random_Generator)
- [Scoring — TetrisWiki](https://tetris.wiki/Scoring)
