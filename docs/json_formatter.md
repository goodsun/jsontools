# JSON Formatter 仕様書

## 概要

JSON Formatterは、JSONデータの整形、ソート、空白値除去などの加工を行うWebベースツールです。整形されたJSONは値をクリックすることで個別にクリップボードにコピー可能です。

## ファイル情報

- **ファイル名**: `json-formatter.html`
- **タイトル**: "JSON Formatter - Label Sort & Empty Value Removal"
- **言語**: HTML, CSS, JavaScript (Vanilla)

## 機能仕様

### 1. 入力機能

#### JSONテキストエリア
- **目的**: JSONデータの入力
- **プレースホルダー**: "JSONを入力してください..."
- **属性**: リサイズ可能（垂直方向のみ）
- **フォント**: Consolas, Monaco, Courier New (monospace)
- **最小高さ**: 150px

### 2. オプション設定

#### ラベルソート
- **チェックボックス**: デフォルトでON
- **機能**: JSONオブジェクトのキーをアルファベット順にソート
- **処理**: 再帰的にネストしたオブジェクトもソート

#### 空白値除去
- **チェックボックス**: デフォルトでOFF
- **機能**: 以下の値を自動削除
  - 空文字列 (`""`)
  - null値
  - 空配列 (`[]`)
  - 空オブジェクト (`{}`)
- **処理**: 再帰的に全階層で適用

### 3. 操作ボタン

#### 整形表示
- **機能**: 設定されたオプションでJSONを整形・表示
- **キーボードショートカット**: `Ctrl + Enter`
- **エラーハンドリング**: 無効なJSON形式の場合はエラーメッセージを表示

#### 全体をコピー
- **機能**: 整形されたJSON全体をクリップボードにコピー
- **条件**: 整形処理が完了している場合のみ有効
- **フィードバック**: コピー成功時にツールチップ表示

#### クリア
- **機能**: 入力エリア、結果表示、エラーメッセージをすべてクリア
- **色**: 赤色（警告色）

### 4. 結果表示機能

#### シンタックスハイライト
- **キー**: 赤色 (`#d73a49`)、太字
- **文字列値**: 青色 (`#032f62`)
- **数値**: 青色 (`#005cc5`)
- **ブール値**: オレンジ色 (`#e36209`)
- **null値**: 紫色 (`#6f42c1`)
- **括弧・カンマ**: グレー (`#6a737d`)、太字

#### インタラクティブ機能
- **値クリックコピー**: 各値をクリックでクリップボードにコピー
- **キークリックコピー**: キーをクリックで対応する値全体をコピー
- **ホバーエフェクト**: クリック可能要素の背景色変化
- **コピーフィードバック**: マウス位置にツールチップ表示

### 5. エラーハンドリング

#### JSON解析エラー
- **表示**: 赤色の背景でエラーメッセージを表示
- **内容**: "無効なJSON形式です: [詳細エラー]"

#### クリップボードエラー
- **表示**: エラーメッセージで通知
- **内容**: "クリップボードへのコピーに失敗しました"

## UI/UXデザイン

### レイアウト
- **最大幅**: 1200px
- **中央配置**: margin auto
- **背景色**: グレー (`#f5f5f5`)
- **コンテナ**: 白背景、角丸、シャドウ付き

### ヘッダー
- **背景色**: ライトグレー (`#e9ecef`)
- **タイトル**: "JSON Formatter"
- **ナビゲーション**: "← メニューに戻る" リンク
- **位置**: 右寄せ

### フッター
- **内容**: "Copyright 2025 goodsun all rights reserved"
- **リンク**: GitHubリポジトリへのリンク
- **位置**: 中央配置

## 技術実装

### データ処理

#### 空白値除去アルゴリズム
```javascript
function isEmpty(value) {
    if (value === null || value === undefined) return true;
    if (typeof value === 'string') return value.trim() === '';
    if (Array.isArray(value)) return value.length === 0;
    if (typeof value === 'object') return Object.keys(value).length === 0;
    return false;
}
```

#### キーソートアルゴリズム
```javascript
function sortObjectKeys(obj) {
    if (Array.isArray(obj)) {
        return obj.map(item => sortObjectKeys(item));
    } else if (obj !== null && typeof obj === 'object') {
        const result = {};
        const sortedKeys = Object.keys(obj).sort();
        for (const key of sortedKeys) {
            result[key] = sortObjectKeys(obj[key]);
        }
        return result;
    }
    return obj;
}
```

### DOM操作

#### インタラクティブJSON生成
- **方法**: DOM要素を動的に生成
- **クリック処理**: イベントリスナーを各要素に追加
- **値のコピー**: Clipboard API使用

### パフォーマンス

#### 最適化要素
- **遅延処理**: なし（即座に実行）
- **メモリ管理**: 大きなJSONオブジェクトの処理を考慮
- **DOM更新**: 一括更新で描画を最適化

## ブラウザ対応

### 必須要件
- **JavaScript**: ES6+ 対応
- **Clipboard API**: コピー機能のため
- **DOM API**: 動的要素生成のため

### 対応ブラウザ
- Chrome 63+
- Firefox 53+
- Safari 13.1+
- Edge 79+

## 制約事項

### 処理制限
- **ファイルサイズ**: ブラウザのメモリ制限に依存
- **JSONサイズ**: 大きなJSONは処理時間が増加
- **ネスト深度**: 極端に深いネストは処理が遅くなる可能性

### セキュリティ
- **XSS対策**: textContentを使用してHTMLエスケープ
- **CSP準拠**: インラインスクリプトなし
- **データ保護**: ローカル処理のみ、外部送信なし

## 使用例

### 基本的な使用方法
1. JSONデータを入力エリアに貼り付け
2. 必要に応じてオプションを選択
3. 「整形表示」ボタンをクリック
4. 結果の値をクリックしてコピー

### 想定ユースケース
- **開発時のJSON確認**: API レスポンスの構造確認
- **データクリーニング**: 不要な空白値の除去
- **JSON整理**: キーの並び替えで可読性向上
- **データ抽出**: 特定の値を素早くコピー

## 保守・拡張性

### 今後の拡張可能性
- **フォーマットオプション**: インデント設定の追加
- **検索機能**: 特定のキーや値の検索
- **履歴機能**: 過去の入力データの保存
- **エクスポート機能**: ファイルダウンロード機能