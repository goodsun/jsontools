# JSON Tools

JSONデータを効率的に操作・閲覧するためのWebベースツール集

## 概要

JSON Toolsは、JSONデータの整形、階層表示、値の選択的コピーなどを行うためのシンプルなWebアプリケーションです。ブラウザで動作し、インストール不要で利用できます。

## オンライン版

GitHub Pagesでホストされているオンライン版を利用できます：

**メインページ**: https://goodsun.github.io/jsontools/

### 直接リンク
- [JSON Formatter](https://goodsun.github.io/jsontools/json-formatter.html) - JSONの整形・加工
- [JSON Hierarchy Viewer](https://goodsun.github.io/jsontools/json-hierarchy-viewer.html) - 階層構造の可視化
- [XPath Researcher](https://goodsun.github.io/jsontools/xpath-researcher.html) - XPath抽出テスト
- [JSON Diff Viewer](https://goodsun.github.io/jsontools/json-diff.html) - JSON差分比較

## ツール一覧

### 1. JSON Formatter
**ファイル**: `json-formatter.html`

JSONデータの整形と加工を行うツールです。

#### 主な機能
- **ラベルソート**: JSONオブジェクトのキーをアルファベット順にソート
- **空白値除去**: 空文字列、null、空配列、空オブジェクトを自動削除
- **シンタックスハイライト**: 見やすい色分け表示
- **値クリックコピー**: 任意の値をクリックしてクリップボードにコピー
- **全体コピー**: 整形されたJSON全体を一括コピー

#### 使用方法
1. テキストエリアにJSONデータを入力
2. 必要に応じてオプション（ラベルソート、空白値除去）をチェック
3. 「整形表示」ボタンをクリック
4. 表示された値をクリックして個別にコピー、または「全体をコピー」で一括コピー

#### キーボードショートカット
- `Ctrl + Enter`: 整形実行

### 2. JSON Hierarchy Viewer
**ファイル**: `json-hierarchy-viewer.html`

JSONの階層構造を美しく表示し、階層単位でのコピーが可能なツールです。

#### 主な機能
- **階層構造の可視化**: ネストした構造を見やすく表示
- **値クリックコピー**: 個別の値をクリックしてコピー
- **キークリックコピー**: キーをクリックして、そのキーに対応する値全体（オブジェクトや配列）をコピー
- **シンタックスハイライト**: データ型に応じた色分け表示
- **ホバーエフェクト**: クリック可能な要素をハイライト表示

#### 使用方法
1. テキストエリアにJSONデータを入力
2. 「整形表示」ボタンをクリック
3. 個別の値をクリックして値のみをコピー
4. キーをクリックして階層全体をコピー
5. 「全体をコピー」で元のJSON全体をコピー

#### キーボードショートカット
- `Ctrl + Enter`: 整形実行

### 3. XPath Researcher
**ファイル**: `xpath-researcher.html`

HTMLソースからXPathを使用して要素を抽出・検証するツールです。スクレイピング前のXPathテストに最適です。

#### 主な機能
- **URL自動取得**: URLを入力してHTMLソースを自動取得（CORSプロキシ使用）
- **XPathによる要素抽出**: 複数のXPathを一括実行して結果を表示
- **インライン編集**: XPath列を直接編集してリアルタイムで結果を更新
- **JSON同期**: XPathを編集すると元のJSON設定も自動更新
- **抽出結果のコピー**: 各抽出結果をクリックでコピー可能
- **エラーハンドリング**: XPath構文エラーや取得エラーの詳細表示

#### 使用方法
1. URLを入力して「HTML取得」ボタンをクリック、またはHTMLソースを直接貼り付け
2. XPath設定をJSON形式で入力（サンプル読み込み機能あり）
3. 「XPath抽出実行」ボタンをクリック
4. 結果テーブルでXPathを直接編集してリアルタイムテスト
5. 抽出結果をクリックしてクリップボードにコピー

#### XPath設定例（求人サイト向け）
```json
{
    "facility_name": "//dt[contains(text(),'施設名')]/following-sibling::dd[1]",
    "price": "//dt[contains(text(),'給与')]/following-sibling::dd[1]",
    "occupation": "//dt[contains(text(),'職種')]/following-sibling::dd[1]",
    "access": "//dt[contains(text(),'アクセス')]/following-sibling::dd[1]"
}
```

#### キーボードショートカット
- `Ctrl + Enter`: XPath抽出実行

#### 制約事項
- CORS制約により一部のサイトはURL取得できません
- その場合はHTMLソースを手動でコピー&ペーストしてください

### 4. JSON Diff Viewer
**ファイル**: `json-diff.html`

2つのJSONを比較して差分を視覚的に表示するツールです。

#### 主な機能
- **差分の視覚的表示**: 元のJSON構造を保ちながら差分をハイライト
- **色分け表示**: 追加（緑）、削除（赤）、変更（黄）で差分を表示
- **差分サマリー**: 追加・削除・変更の件数を上部に表示
- **ネスト構造対応**: 深い階層のオブジェクトも正確に比較
- **変更内容の詳細表示**: 変更前後の値を「旧値 → 新値」形式で表示

#### 使用方法
1. 左側のテキストエリアに元のJSON（Before）を入力
2. 右側のテキストエリアに比較するJSON（After）を入力
3. 「差分を表示 (Show Diff)」ボタンをクリック
4. 差分結果が色分けされて表示される

#### 表示形式
- **追加された項目**: 緑色の背景と左側のボーダーで表示
- **削除された項目**: 赤色の背景と左側のボーダーで表示
- **変更された項目**: 黄色の背景と左側のボーダーで表示、値は「旧値 → 新値」形式
- **変更なしの項目**: グレー色のテキストで表示

## 技術仕様

### 対応ブラウザ
- Chrome/Edge (推奨)
- Firefox
- Safari
- その他のモダンブラウザ

### 必要な機能
- JavaScript有効
- Clipboard API対応（コピー機能のため）
- XPath対応（XPath Researcherのため）

### セキュリティ
- 完全にクライアントサイドで動作
- データはサーバーに送信されません
- プライベートな情報も安全に処理可能

## インストール・使用方法

### 推奨：オンライン版の利用
GitHub Pagesでホストされているオンライン版を使用することを推奨します：
- **URL**: https://goodsun.github.io/jsontools/
- **利点**: インストール不要、常に最新版、高速アクセス

### ローカルでの利用
1. リポジトリをクローンまたはダウンロード
2. `index.html`をブラウザで直接開く

### Webサーバーでの利用
1. リポジトリをクローンまたはダウンロード
2. WebサーバーのDocumentRootに配置
3. ブラウザで`index.html`にアクセス

## ファイル構成

```
jsontools/
├── index.html                    # メインページ（ツール選択）
├── json-formatter.html           # JSON整形ツール
├── json-hierarchy-viewer.html    # JSON階層ビューアー
├── xpath-researcher.html         # XPath研究ツール
├── json-diff.html                # JSON差分ビューアー
├── docs/
│   ├── index.md                  # メインページ仕様書
│   ├── json_formatter.md         # JSON Formatter仕様書
│   ├── json_hierarchy_viewer.md  # JSON Hierarchy Viewer仕様書
│   └── xpath_researcher.md       # XPath Researcher仕様書
├── README.md                     # このファイル
└── LICENSE                       # ライセンス
```

## 開発情報

- **言語**: HTML, CSS, JavaScript (Vanilla)
- **依存関係**: なし
- **ライセンス**: MIT License

## 更新履歴

- **2025年**: 初期リリース
  - JSON Formatter機能実装
  - JSON Hierarchy Viewer機能実装
  - XPath Researcher機能実装
  - JSON Diff Viewer機能実装
  - 統一されたUI/UXデザイン
  - インライン編集とリアルタイム更新機能

## ライセンス

Copyright 2025 goodsun all rights reserved

詳細は[LICENSE](LICENSE)ファイルを参照してください。

## 貢献・フィードバック

プロジェクトURL: https://github.com/goodsun/jsontools

バグ報告や機能要望は、GitHubのIssuesからお願いします。
