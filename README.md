# Longevity Conference 2027 — 静的サイト一式

最新のキャンバスの見た目を完全に再現した、デプロイ可能な静的サイトです。
各 HTML はすべての必要コード（ランタイム・スタイル・スクリプト）を 1 ファイルに内包した
**自己完結型ファイル**なので、ビルド工程は不要です。VS Code で開いてダブルクリック、
または任意の静的ホスティング（Netlify / Vercel / S3 / GitHub Pages 等）にそのまま配置できます。

## ディレクトリ構造

```
longevity-conference/
├── index.html            … トップページ（Longevity Conference 本体）
├── speakers.html         … 登壇者一覧ページ
├── sponsor-form.html     … 協賛資料請求フォーム
├── sponsor-thanks.html   … 請求完了（サンクス）ページ
└── README.md             … このファイル
```

ページ間リンクは上記ファイル名で相互参照済みです（`index.html` がトップ）。

## ローカルでの開き方

- **そのまま開く:** `index.html` をブラウザにドラッグ、または VS Code の拡張
  「Live Preview」「Live Server」で開く。
- **簡易サーバ（推奨／任意）:**
  ```bash
  # Python があれば
  python3 -m http.server 8000
  # → http://localhost:8000 を開く
  ```

## デプロイ

ビルド不要。フォルダごとアップロードするだけです。

- **Netlify:** フォルダをドラッグ＆ドロップ（公開ディレクトリ＝このフォルダ）
- **Vercel:** Framework Preset = "Other"、Build Command 空、Output Directory = このフォルダ
- **GitHub Pages:** リポジトリに push し、Pages の公開元をこのフォルダに設定

## 外部依存（オンライン時に読み込まれるもの）

各ファイルは自己完結していますが、以下だけは**外部 URL を参照**しています。
インターネット接続のある通常の公開環境では問題なく表示されます。
完全オフライン運用が必要な場合は、各アセットをローカルへ保存して参照先を差し替えてください。

| 種別 | 参照元 | 使用箇所 |
|---|---|---|
| 登壇者の写真（ダミー） | `images.unsplash.com`（各 `?auto=format...` URL） | トップ SPEAKERS / speakers.html |
| 主催者ロゴ | `https://www.this.ne.jp/wp-content/uploads/Informa_Logo_Indigo-1.png` | トップ ORGANIZER |
| 世界拠点マップ画像 | `https://www.this.ne.jp/wp-content/themes/sng-ubm-this/common/img/img_organizer.jpg` | トップ ORGANIZER |
| 会場の地図 | `https://www.google.com/maps/embed?...`（Google Maps 埋め込み iframe） | トップ VENUE |

※ 登壇者写真はすべて Unsplash の無料素材によるダミーです。確定した講演者の写真に
差し替える場合は、各ページの該当 `img` の URL を置き換えてください。
※ Google Fonts（Noto Sans JP / Noto Serif JP / Geist Mono）と Lucide アイコンは
ファイル内にインライン化済みのため、追加の読み込みは発生しません。

## 協賛資料請求フォームについて（重要）

`sponsor-form.html` の送信処理は現在**擬似送信（ダミー）**です。
入力値はサーバへ送られず、送信に成功すると `sponsor-thanks.html` に遷移します。

本番運用では、フォーム送信先を Supabase など実バックエンドに接続してください。
接続ポイントはフォームの送信ハンドラ（`_save` 相当の処理）で、ここを
実際の API 呼び出し（`fetch` で Supabase の REST / Edge Function へ POST）に
差し替える想定です。自動返信メールは Supabase 側（Edge Function 等）で
送信する構成を想定しています。

## 編集について

これらの `*.html` は配布用に最適化（インライン化）された成果物です。
継続的な編集・デザイン調整は、引き続きキャンバス側のソース（`*.dc.html`）で行い、
更新後に再度この静的ファイルを書き出す運用を推奨します。
