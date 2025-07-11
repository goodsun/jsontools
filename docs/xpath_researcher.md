# xPath researcher

## 入力
・対象URL
・設定json

## 動作
設定jsonにはxpathが記述されている。

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
    "line": "//dt[text()='勤務地/アクセス']/following-sibling::dd[1]",
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

対象URLのページから各xPathで取得できる値を表示したい。

## 実現可能性の分析

### 技術的な制約

1. **CORS（Cross-Origin Resource Sharing）問題**
   - ブラウザのセキュリティ制約により、異なるドメインのページを直接取得できません
   - 外部URLのHTMLを取得してXPathで解析することは、通常のWebページからは不可能です

2. **Same-Origin Policy**
   - JavaScriptは同一オリジンからのリソースしかアクセスできません
   - 外部サイトのDOM要素に直接アクセスすることはできません

### 実現可能な方法

#### 方法1: プロキシサーバー利用
```
ブラウザ → プロキシサーバー → 対象URL
```
- バックエンドサーバーが必要
- 現在のクライアントオンリー構成では実現困難

#### 方法2: ブラウザ拡張機能
- Chrome/Firefox拡張として実装
- より強い権限でCORSを回避可能
- ただし配布・インストールが必要

#### 方法3: ユーザーがHTMLソースを貼り付け（採用案）
```
1. ユーザーが対象URLのHTMLソースをコピー
2. テキストエリアに貼り付け
3. XPath設定JSONを入力
4. DOMParserでHTMLを解析
5. XPathで要素を抽出して表示
```

## 実装方針

### 採用する方法
**方法3: HTMLソース貼り付け方式**

現在のプロジェクト構成（クライアントサイド完結）を維持しつつ実現可能な方法として採用。

### UI構成
1. **HTMLソース入力エリア**（大きなテキストエリア）
2. **XPath設定JSON入力エリア**
3. **抽出実行ボタン**
4. **結果表示エリア**（各XPathの結果をテーブル表示）

### 処理フロー
```javascript
1. HTMLソースをDOMParserでパース
2. XPath設定JSONをパース
3. 各XPathに対してdocument.evaluate()実行
4. 取得した値を整理して表示
5. 結果をJSONとしてコピー可能にする
```

### 実装の利点
- サーバー不要（現在の構成を維持）
- セキュリティ問題なし
- 即座に結果確認可能
- デバッグ用途に最適

### 使用シナリオ
1. 開発者がスクレイピング前にXPathをテスト
2. Web解析の事前調査
3. XPath式の検証・デバッグ

### 技術仕様
- **言語**: HTML, CSS, JavaScript (Vanilla)
- **依存関係**: なし
- **対応ブラウザ**: XPath対応ブラウザ（Chrome, Firefox, Safari, Edge）
- **主要API**: DOMParser, document.evaluate()
