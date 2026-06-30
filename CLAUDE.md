# Longevity Conference 2027 — Claude Code 作業ガイド

## プロジェクト概要

Claude Design から書き出した自己完結型静的サイト。ビルド不要。
4つの HTML ファイルをそのまま静的ホスティングにデプロイすれば動く。

## ファイル構成

```
website-longevityC/
├── index.html            トップページ（メインコンテンツすべて）
├── speakers.html         登壇者一覧ページ
├── sponsor-form.html     協賛資料請求フォーム（★送信未実装）
├── sponsor-thanks.html   フォーム送信完了ページ
├── CLAUDE.md             このファイル
├── README.md             デプロイ手順・外部依存まとめ
└── _patches/             追加 JS/CSS パッチ置き場（バンドルに直接書かない変更はここ）
```

## バンドル形式の仕組みと編集方針

各 HTML は Claude Design の「バンドル書き出し」形式。構造：

```
<script>                          ← アンパッカー（変更しない）
<script type="__bundler/manifest">← base64 アセット群（変更しない）
<script type="__bundler/template">← JSON 文字列化された内部 HTML（ここを編集）
```

### 内部 HTML を編集する方法

`__bundler/template` スクリプトの中身は JSON.stringify された HTML 文字列。
PowerShell でパース → 編集 → 再エンコードして書き戻す手順を使う：

```powershell
# 例: template の中身を取り出して確認
$html = Get-Content "sponsor-form.html" -Raw
$match = [regex]::Match($html, '<script type="__bundler/template">\s*([\s\S]+?)\s*</script>')
$inner = $match.Groups[1].Value | ConvertFrom-Json
```

**重要制約：**
- HTML ファイルは互いに相対パスでリンク（`href="speakers.html"` 等）。移動・リネーム不可。
- `__bundler/manifest` の base64 バイナリ（フォント・画像）は変更しない。
- `__bundler/template` の JSON を変更するときは必ず JSON として有効な状態を保つ。

## 未対応タスク（優先順）

1. **フォームバックエンド接続** — `sponsor-form.html` の送信処理を Supabase 等に接続
2. **登壇者写真の差し替え** — Unsplash ダミーを実際の写真 URL に変更
3. **外部画像のローカル化** — `this.ne.jp` 参照のロゴ・会場画像をローカルコピーへ

## 外部依存（README.md に詳細あり）

| 種別 | URL | 依存ページ |
|---|---|---|
| 登壇者ダミー写真 | images.unsplash.com | index.html / speakers.html |
| 主催者ロゴ | this.ne.jp/... | index.html |
| 会場マップ画像 | this.ne.jp/... | index.html |
| Google Maps | maps.google.com/embed | index.html |

## 技術スタック（バンドル内部）

React 18、Tailwind CSS v4、Lucide アイコン、Noto Serif JP / Noto Sans JP / Geist Mono（インライン化済み）
