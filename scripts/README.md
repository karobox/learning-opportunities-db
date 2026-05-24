# scripts/

将来のデータ収集スクリプトを格納するディレクトリです。  
現時点ではスクレイパーは未実装です。

## 想定する構成

```
scripts/
├── README.md               # このファイル
├── requirements.txt        # Python 依存パッケージ
├── scrape_all.py           # 全ソース一括収集（エントリポイント）
├── sources/                # ソースごとのスクレイパー
│   ├── base.py             # 共通基底クラス
│   ├── jia.py              # 日本建築家協会
│   ├── aij.py              # 日本建築学会
│   ├── jsce.py             # 土木学会
│   └── ...
└── utils/
    ├── csv_writer.py       # CSV への書き込みユーティリティ
    └── date_utils.py       # 日付パース・正規化
```

## データの流れ（予定）

```
各主催団体の公式サイト
    ↓ (HTTP GET / BeautifulSoup)
sources/*.py
    ↓ (正規化・重複除去)
scrape_all.py
    ↓ (追記 or 全更新)
docs/data/events.csv
    ↓ (GitHub Actions が git commit & push)
GitHub Pages 自動更新
```

## 実装時の注意事項

- スクレイピング前に各サイトの利用規約・`robots.txt` を必ず確認する
- 過度なアクセスを避けるため `time.sleep()` でウェイトを入れる
- 取得するのは**公開イベント情報のみ**（PDF の再配布・主催文面の全文転載は不可）
- CPD ポイント数・換算値は CSV に含めない
- 公式 URL は必ず記録する

## requirements.txt（予定）

```
requests>=2.31.0
beautifulsoup4>=4.12.0
lxml>=5.0.0
pandas>=2.0.0
```
