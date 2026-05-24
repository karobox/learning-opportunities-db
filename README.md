# 建設関連業 公開研修情報（試験版）

[karobox](https://github.com/karobox) が個人実験環境として公開・維持している AI 観測プロジェクトです。  
土木・建築・測量・地質・補償分野を中心に、公開研修情報を AI が自動収集した参考情報 DB です。  
`docs/data/events.csv` の差し替えだけでページが更新されます。

**公開ページ：** https://karobox.github.io/learning-opportunities-db/

## 免責事項

- AI 自動収集による**参考情報**であり、正確性・最新性は保証しません
- 各主催団体とは**無関係**です。主催団体の承認を得たものではありません
- 開催詳細・費用・申込方法は必ず各主催者の**公式サイト**でご確認ください
- CPD ポイントの認定・換算・証明には**一切関与しません**

## フォルダ構成

```
learning-opportunities-db/
├── docs/                    # GitHub Pages 公開ルート
│   ├── index.html           # 公開ページ
│   └── data/
│       └── events.csv       # 研修データ（差し替えで更新）
├── scripts/                 # 将来のスクレイパー置き場
│   └── README.md
├── .github/
│   └── workflows/
│       └── update.yml       # 自動更新ワークフロー（テンプレート）
└── README.md
```

## ローカル確認方法

`fetch()` の CORS 制限のため、ファイルを直接ブラウザで開いても CSV が読み込めません。
簡易 HTTP サーバー経由でアクセスしてください。

```bash
# Python（推奨）
cd learning-opportunities-db/docs
python -m http.server 8080
# → http://localhost:8080

# Node.js
npx serve docs
```

## GitHub Pages 公開手順

1. リポジトリを GitHub にプッシュ（`github.com/karobox/learning-opportunities-db`）
2. **Settings → Pages** → Source: `Deploy from a branch`
3. Branch: `main` / Folder: `/docs` → **Save**
4. 数分後に https://karobox.github.io/learning-opportunities-db/ で公開

**公開前の確認：**
- サンプルデータを実データに差し替え済みか
- `docs/index.html` の `sample-notice` ブロックを削除したか

## CSV 列の説明

| 列名 | 内容 |
|------|------|
| 主催団体 | 団体の名称 |
| プログラム名 | 研修名 |
| 分野カテゴリ | 補償業務・測量・建築設計・構造・施工管理 等 |
| 開催日 | `YYYY-MM-DD` |
| 開始時刻 | `HH:MM` |
| 終了時刻 | `HH:MM` |
| 実受講時間 | 例：`3.0h` |
| 開催形式 | `オンライン` / `対面` / `ハイブリッド` |
| 開催場所 | 会場名・住所またはオンライン旨（自由記述） |
| region_pref | 開催地域（正規化）。都道府県名 / `オンライン` / `全国` |
| 費用 | 文字列。例：`"会員3,000円／非会員5,000円"` |
| 会員限定有無 | `会員限定` / `一般参加可` |
| 申込期限 | `YYYY-MM-DD` |
| 公式情報URL | 主催者の公式 URL（個別ページ優先・必須） |
| 掲載確認日 | `YYYY-MM-DD` |

> CPD ポイント数・換算値は記載しない。  
> カンマを含む費用フィールドはダブルクォートで囲む（例：`"会員3,000円"`）。

---

## AI 収集設計について

### このプロジェクトが観測しようとしているもの

- 建設関連業の公開研修情報のうち、AI が自動収集できる範囲とその構造
- 分野・形式・費用帯の分布（供給側の観測）
- AI 収集が困難な情報の分布（業界のデジタル情報公開成熟度の観測）

### 収集対象

主催団体名・研修名・開催日時・受講時間・開催形式・開催場所・費用・申込期限・公式 URL

### 収集対象外

| 項目 | 理由 |
|------|------|
| 講師情報・プロフィール | 著作権・個人情報リスク |
| プログラム詳細・概要本文 | 著作権リスク |
| CPD ポイント数 | 認定との混同を避ける |
| 会員限定情報（ログイン後） | アクセス不可・規約問題 |

### 既知の収集バイアス

| バイアス | 内容 |
|----------|------|
| HTML 依存 | PDF 主体の団体は捕捉困難 |
| 組織規模 | インフラが安定した大規模団体が取得しやすい |
| 構造安定性 | HTML 構造が頻繁に変わる団体は脱落しやすい |
| JS 依存 | SPA・JS 依存サイトは静的スクレイパーで取得できない |

### 「取れないこと」も観測結果

取得失敗・PDF 依存・構造不安定の分布自体が、  
業界の情報公開インフラの観測データになります。

---

## 将来の自動更新

`.github/workflows/update.yml` にワークフローテンプレートを用意しています。  
`scripts/` にスクレイパーを実装後、コメントアウト部分を有効化してください。

## 掲載修正・停止依頼

[Issues](https://github.com/karobox/learning-opportunities-db/issues/new) からご連絡ください。

---

*karobox — 小規模 AI 観測実験環境 / [github.com/karobox](https://github.com/karobox)*
