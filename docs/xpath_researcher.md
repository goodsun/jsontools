# xPath researcher

## 入力

・対象 URL
・設定 json

## 動作

設定 json には xpath が記述されている。

例：
{
"access": "//dt[contains(text(),'アクセス')]/following-sibling::dd[1]",
"bonus": "//dt[contains(text(),'賞与')]/following-sibling::dd[1]",
"city": "//dt[contains(text(),'所在地')]/following-sibling::dd[1]",
"contract": "//dt[contains(text(),'雇用形態')]/following-sibling::dd[1]",
"dept": "//dt[contains(text(),'診療科目')]/following-sibling::dd[1]",
"detail": "//dt[contains(text(),'概要')]/following-sibling::dd[1]",
"facility_name": "//dt[contains(text(),'施設名')]/following-sibling::dd[1]",
"facility_type": "//dt[contains(text(),'施設形態')]/following-sibling::dd[1]",
"holiday": "//dt[contains(text(),'休日')]/following-sibling::dd[1]",
"license": "//dt[contains(text(),'資格')]/following-sibling::dd[1]",
"line": "//dt[text()='アクセス']/following-sibling::dd[1]",
"name": "//title",
"occupation": "//dt[contains(text(),'職種')]/following-sibling::dd[1]",
"position": "//dt[contains(text(),'担当部署')]/following-sibling::dd[1]",
"prefecture": "//dt[contains(text(),'所在地')]/following-sibling::dd[1]",
"price": "//dt[contains(text(),'給与')]/following-sibling::dd[1]",
"required_skill": "//dt[contains(text(),'応募資格')]/following-sibling::dd[1]",
"staff_comment": "//p[contains(text(),'コメント')]",
"staff_comment_title": "//p[contains(text(),'コメント')]/following-sibling::p",
"station": "//dt[contains(text(),'最寄駅')]/following-sibling::dd[1]",
"test_period": "//dt[contains(text(),'試用期間')]/following-sibling::dd[1]",
"title_original": "//title",
"welfare_program": "//dt[contains(text(),'福利厚生')]/following-sibling::dd[1]",
"working_hours": "//dt[contains(text(),'勤務時間')]/following-sibling::dd[1]",
"working_style": "//dt[contains(text(),'勤務形態')]/following-sibling::dd[1]"
}

対象 URL のページから各 xPath で取得できる値を表示したい。

## 実装機能

### 主要機能

1. **URL 自動取得機能**

   - URL を入力して HTML ソースを自動取得
   - 複数の CORS プロキシ（api.allorigins.win、cors-anywhere.herokuapp.com）を使用
   - CORS 制約により一部のサイトは取得できない場合がある

2. **HTML ソース直接入力**

   - テキストエリアに HTML ソースを直接貼り付け可能
   - URL 取得できない場合の代替手段

3. **XPath 抽出実行**

   - JSON 形式で複数の XPath を定義
   - 一括実行して結果をテーブル表示
   - エラーハンドリング（XPath 構文エラー、取得エラー）

4. **インライン編集機能**

   - 結果テーブルの XPath 列を直接編集可能
   - リアルタイムで結果を更新（500ms のデバウンス処理）
   - 編集内容は元の JSON 設定に自動反映

5. **サンプル読み込み機能**

   - 求人サイト向けのサンプル XPath 設定と HTML を自動読み込み
   - 初心者でもすぐに試せる環境を提供

6. **コピー機能**
   - 各抽出結果をクリックでクリップボードにコピー
   - コピー成功時にツールチップ表示

### UI 構成

1. **URL 入力エリア**

   - URL 入力フィールドと「HTML 取得」ボタン
   - CORS 制約の注意書き表示

2. **HTML ソース入力エリア**（大きなテキストエリア）

   - URL 取得または HTML 直接貼り付け
   - プレースホルダーで使用方法を案内

3. **XPath 設定 JSON 入力エリア**

   - JSON 形式で XPath 設定を入力
   - サンプル表示でフォーマットを案内

4. **操作ボタン**

   - 「XPath 抽出実行」ボタン（Ctrl+Enter 対応）
   - 「サンプルを読み込み」ボタン
   - 「クリア」ボタン

5. **結果表示エリア**
   - 3 列のテーブル（キー、XPath、抽出結果）
   - XPath 列は編集可能なテキストエリア
   - 結果値はクリックでコピー可能

### 処理フロー

#### 基本フロー

```javascript
1. URL入力時：CORSプロキシ経由でHTML取得、またはHTMLソース直接入力
2. XPath設定JSONをパース
3. HTMLソースをDOMParserでパース
4. 各XPathに対してdocument.evaluate()実行
5. 結果をテーブルに表示（編集可能なXPath列含む）
6. 結果値のクリックでコピー機能提供
```

#### インライン編集フロー

```javascript
1. XPath列のテキストエリアで編集
2. 500msのデバウンス後に自動実行
3. 新しいXPathで再評価
4. 結果セルをリアルタイム更新
5. 元のJSON設定も自動更新
```

### 実装の利点

- サーバー不要（現在の構成を維持）
- セキュリティ問題なし
- 即座に結果確認可能
- デバッグ用途に最適
- URL 自動取得で利便性向上
- インライン編集で XPath の試行錯誤が容易
- リアルタイム更新で効率的な開発

### 使用シナリオ

1. 開発者がスクレイピング前に XPath をテスト
2. Web 解析の事前調査
3. XPath 式の検証・デバッグ
4. URL から直接 HTML を取得して素早くテスト
5. インライン編集で効率的に XPath を調整
6. 複数の XPath を一括で検証

### 技術仕様

- **言語**: HTML, CSS, JavaScript (Vanilla)
- **依存関係**: なし
- **対応ブラウザ**: XPath 対応ブラウザ（Chrome, Firefox, Safari, Edge）
- **主要 API**:
  - DOMParser (HTML 解析)
  - document.evaluate() (XPath 評価)
  - Fetch API (URL 取得)
  - Clipboard API (コピー機能)
- **外部サービス**: CORS プロキシ（api.allorigins.win 等）
