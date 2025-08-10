[![godoc reference](https://img.shields.io/badge/godoc-reference-blue.svg)](https://godoc.org/github.com/jfeliu007/goplantuml/parser) [![Go Report Card](https://goreportcard.com/badge/github.com/jfeliu007/goplantuml)](https://goreportcard.com/report/github.com/jfeliu007/goplantuml) [![codecov](https://codecov.io/gh/jfeliu007/goplantuml/branch/master/graph/badge.svg)](https://codecov.io/gh/jfeliu007/goplantuml) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![GitHub release](https://img.shields.io/github/release/jfeliu007/goplantuml.svg)](https://github.com/jfeliu007/goplantuml/releases/)
[![Mentioned in Awesome Go](https://awesome.re/mentioned-badge.svg)](https://github.com/avelino/awesome-go) 
[![DUMELS Diagram](https://www.dumels.com/api/v1/badge/23ff0222-e93b-4e9f-a4ef-4d5d9b7a5c7d)](https://www.dumels.com/diagram/23ff0222-e93b-4e9f-a4ef-4d5d9b7a5c7d) 

# GoPlantUML V2

[English](README.md) | [日本語](README.ja.md)

GoPlantUMLは、Goソースコードから PlantUML図を生成するプロセスを効率化するために開発されたオープンソースツールです。GoPlantUMLを使用することで、開発者はGoプロジェクト内の構造と関係を簡単に視覚化でき、コードの理解とドキュメント化を支援します。Goソースコードを解析してPlantUML図を生成することで、GoPlantUMLは開発者がコードベースアーキテクチャ、パッケージ依存関係、関数相互作用の明確で簡潔な視覚的表現を作成することを可能にします。このツールはドキュメント化プロセスを簡素化し、複雑なGoプロジェクトの視覚的概要を提供することで、チームメンバー間のコラボレーションを強化します。GoPlantUMLは積極的にメンテナンスされており、Goコミュニティからの貢献を歓迎しています。

![class-diagram](example/example.png)

## 目次

- [機能](#機能)
- [インストール](#インストール)
- [基本的な使用方法](#基本的な使用方法)
- [コマンドライン引数](#コマンドライン引数)
- [カスタムキーワード](#カスタムキーワード)
- [YAML設定](#yaml設定)
- [ライブラリとして使用](#ライブラリとして使用)
- [貢献](#貢献)
- [ライセンス](#ライセンス)

## 機能

### 🔄 V2 新機能

- **カスタムキーワードパターン**: 関数やメソッドを特定のカテゴリに分類
- **YAML設定サポート**: 複雑なプロジェクト設定管理
- **改善されたパッケージ依存関係の視覚化**: より正確で読みやすい図
- **関数カテゴリ分け**: 認証、エンティティ、データベース、API、ユーティリティ関数を区別
- **カスタムリソースタイプ**: プロジェクト固有のリソース分類

### ⚙️ 基本機能

- プログラムで依存関係として組み込み可能
- すべてのGo関数・メソッドを完全に分析・表示
- PlantUMLおよび関連ツールとの完全互換性
- 構造体フィールドの詳細表示
- パッケージレベルでの集約とフィルタリング
- ベンダーパッケージの自動除外

## インストール

### バイナリから

リリースページから事前コンパイル済みのバイナリをダウンロード:
https://github.com/jfeliu007/goplantuml/releases

### ソースから

```bash
go install github.com/jfeliu007/goplantuml/cmd/goplantuml@latest
```

### パッケージマネージャー

#### Homebrew (macOS)
```bash
brew install goplantuml
```

#### Scoop (Windows)
```bash
scoop install goplantuml
```

#### Chocolatey (Windows)
```bash
choco install goplantuml
```

#### AUR (Arch Linux)
```bash
yay -S goplantuml
```

## 基本的な使用方法

### 基本コマンド

```bash
# 現在のディレクトリを解析
goplantuml .

# 特定のパッケージを解析
goplantuml path/to/package

# 結果をファイルに出力
goplantuml . > output.puml
```

### 実用例

```bash
# プロジェクト全体を解析して図を生成
goplantuml ./src > project_diagram.puml

# 特定のパッケージのみ分析
goplantuml ./pkg/auth > auth_diagram.puml

# 複数のパッケージを含める
goplantuml ./pkg/auth ./pkg/database > services_diagram.puml
```

## コマンドライン引数

| フラグ | 短縮形 | 説明 | デフォルト |
|-------|-------|-----|----------|
| `--ignore` | `-i` | 無視するディレクトリ（カンマ区切り） | vendor,node_modules |
| `--recursive` | `-r` | サブディレクトリを再帰的に処理 | true |
| `--show-aggregations` | `-a` | パッケージ間の依存関係を表示 | false |
| `--show-compositions` | `-c` | 構造体の組み合わせを表示 | false |
| `--show-implementations` | `-I` | インターフェース実装を表示 | false |
| `--show-aliases` | `-A` | 型エイリアスを表示 | false |
| `--show-connection-labels` | `-l` | 接続ラベルを表示 | false |
| `--exclude-imports` | `-x` | 外部インポートを除外 | false |
| `--title` | `-t` | 図のタイトル | "" |
| `--notes` | `-n` | 図に注釈を追加 | "" |
| `--keywords-file` | `-k` | カスタムキーワードファイルのパス | "" |
| `--config` | | YAML設定ファイルのパス | "" |
| `--output` | `-o` | 出力ファイルパス | stdout |

### 使用例

```bash
# 詳細な図を生成
goplantuml -a -c -I -l ./pkg

# カスタムタイトルと注釈付きで出力
goplantuml -t "システムアーキテクチャ" -n "2024年8月版" ./src

# 特定ディレクトリを無視して出力
goplantuml -i "test,vendor,docs" ./

# カスタムキーワードファイルを使用
goplantuml -k keywords.txt ./pkg
```

## カスタムキーワード

V2では、関数やメソッドを特定のカテゴリに分類するためのカスタムキーワードパターンをサポートしています。

### キーワードファイル形式

`keywords.txt`ファイルを作成:

```
# 認証関連
auth:login,authenticate,authorize,token,jwt
auth:password,hash,bcrypt,verify

# データベース関連  
database:create,insert,update,delete,query
database:transaction,commit,rollback,migrate

# API関連
api:handler,controller,endpoint,route
api:request,response,middleware

# エンティティ関連
entity:model,struct,type,interface

# ユーティリティ関連
utility:helper,util,format,convert,validate
```

### 使用方法

```bash
goplantuml -k keywords.txt ./src
```

### カテゴリ表示

カスタムキーワードを使用すると、PlantUML図では関数が以下のように表示されます:

```plantuml
class UserService {
  + [AUTH] Login(email, password)
  + [AUTH] ValidateToken(token)
  + [DATABASE] CreateUser(user)
  + [DATABASE] UpdateUser(user)
  + [API] HandleLogin(ctx)
  + [UTILITY] ValidateEmail(email)
}
```

## YAML設定

複雑なプロジェクトでは、YAML設定ファイルを使用してより詳細な設定が可能です。

### 設定ファイル例

`goplantuml.yaml`を作成:

```yaml
# プロジェクト基本設定
project:
  name: "マイプロジェクト"
  version: "1.0.0"
  description: "プロジェクトアーキテクチャ図"

# 出力設定
output:
  title: "システムアーキテクチャ"
  notes: "最終更新: 2024年8月"
  show_aggregations: true
  show_compositions: true
  show_implementations: true
  show_connection_labels: true

# 解析設定
analysis:
  recursive: true
  exclude_imports: false
  ignore_directories:
    - vendor
    - node_modules
    - .git
    - test
    - docs

# カスタムキーワード設定
keywords:
  auth:
    - login
    - authenticate
    - authorize
    - token
    - jwt
    - password
    - hash
    - bcrypt
    
  database:
    - create
    - insert
    - update
    - delete
    - query
    - transaction
    - commit
    - rollback
    
  api:
    - handler
    - controller
    - endpoint
    - route
    - request
    - response
    - middleware
    
  entity:
    - model
    - struct
    - type
    - interface
    
  utility:
    - helper
    - util
    - format
    - convert
    - validate

# カスタムリソースタイプ
custom_resources:
  - name: "Configuration"
    pattern: "config|setting|option"
    color: "#FFE4B5"
  
  - name: "Service"
    pattern: "service|manager|provider"
    color: "#E6E6FA"
    
  - name: "Repository"
    pattern: "repository|repo|store|dao"
    color: "#F0FFF0"
```

### 使用方法

```bash
goplantuml --config goplantuml.yaml ./src
```

## ライブラリとして使用

GoPlantUMLをGoプログラム内でライブラリとして使用することも可能です。

### インストール

```bash
go get github.com/jfeliu007/goplantuml/parser
```

### 基本的な使用例

```go
package main

import (
    "fmt"
    "strings"
    
    "github.com/jfeliu007/goplantuml/parser"
)

func main() {
    // パーサーを作成
    classParser := parser.NewClassParser()
    
    // ディレクトリを解析
    result, err := classParser.Parse("./src")
    if err != nil {
        panic(err)
    }
    
    // PlantUML出力を生成
    output := result.String()
    fmt.Println(output)
}
```

### 高度な使用例

```go
package main

import (
    "fmt"
    "github.com/jfeliu007/goplantuml/parser"
)

func main() {
    // カスタム設定でパーサーを作成
    classParser := parser.NewClassParser()
    
    // オプションを設定
    options := &parser.Options{
        ShowAggregations:     true,
        ShowCompositions:     true,
        ShowImplementations:  true,
        ShowConnectionLabels: true,
        IgnoreDirectories:    []string{"vendor", "test"},
        Title:               "システムアーキテクチャ",
        Notes:               "自動生成された図",
    }
    
    // カスタムキーワードを設定
    keywords := map[string][]string{
        "auth":     {"login", "authenticate", "token"},
        "database": {"create", "update", "delete", "query"},
        "api":      {"handler", "controller", "endpoint"},
    }
    classParser.SetKeywords(keywords)
    
    // 解析実行
    result, err := classParser.ParseWithOptions("./src", options)
    if err != nil {
        panic(err)
    }
    
    // 結果を出力
    fmt.Println(result.String())
}
```

## クライアントライブラリ

GoPlantUMLは、プログラムでダイアグラム生成を行うためのクライアントライブラリも提供しています。

### インストール

```bash
go get github.com/jfeliu007/goplantuml/pkg/client
```

### 使用例

```go
package main

import (
    "context"
    "fmt"
    "log"
    
    "github.com/jfeliu007/goplantuml/pkg/client"
    "github.com/jfeliu007/goplantuml/pkg/entity/request"
)

func main() {
    // クライアントを作成
    clientLib := client.NewClient()
    
    // リクエストを準備
    req := &request.Generate{
        Paths: []string{"./src", "./pkg"},
        Options: request.Options{
            ShowAggregations:    true,
            ShowCompositions:    true,
            ShowImplementations: true,
            Title:              "プロジェクト構造図",
            OutputPath:         "diagram.puml",
        },
        Keywords: map[string][]string{
            "auth":     {"login", "authenticate"},
            "database": {"create", "update", "delete"},
        },
    }
    
    // ダイアグラムを生成
    ctx := context.Background()
    result, err := clientLib.Generate(ctx, req)
    if err != nil {
        log.Fatal(err)
    }
    
    fmt.Printf("ダイアグラムが正常に生成されました: %s\n", result.OutputPath)
    fmt.Printf("解析されたパッケージ数: %d\n", len(result.Packages))
    fmt.Printf("解析された構造体数: %d\n", result.StructCount)
}
```

## PlantUMLでの表示

生成されたPlantUMLファイルを画像に変換するには、以下の方法があります:

### PlantUML jar を使用

```bash
# PlantUMLをダウンロード
wget http://sourceforge.net/projects/plantuml/files/plantuml.jar/download -O plantuml.jar

# PNGに変換
java -jar plantuml.jar diagram.puml

# SVGに変換
java -jar plantuml.jar -tsvg diagram.puml
```

### オンラインエディタ

1. [PlantUML Online Server](http://www.plantuml.com/plantuml/uml/) にアクセス
2. 生成されたPlantUMLコードを貼り付け
3. 図を表示・ダウンロード

### VS Code 拡張機能

1. PlantUML拡張機能をインストール
2. `.puml`ファイルを開く
3. `Alt+D`でプレビュー表示

## 貢献

GoPlantUMLプロジェクトへの貢献を歓迎します！

### 貢献手順

1. このリポジトリをフォーク
2. 機能ブランチを作成 (`git checkout -b feature/amazing-feature`)
3. 変更をコミット (`git commit -m 'Add some amazing feature'`)
4. ブランチにプッシュ (`git push origin feature/amazing-feature`)
5. プルリクエストを作成

### 開発環境のセットアップ

```bash
# リポジトリをクローン
git clone https://github.com/jfeliu007/goplantuml.git
cd goplantuml

# 依存関係をインストール
go mod download

# テストを実行
go test ./...

# ビルド
go build ./cmd/goplantuml
```

### コードスタイル

- `go fmt`でコードフォーマット
- `go vet`でコード検証
- テストカバレッジを維持
- ドキュメント更新

## サポート

### 問題報告

バグや機能要求は[GitHub Issues](https://github.com/jfeliu007/goplantuml/issues)で報告してください。

### FAQ

**Q: ベンダーパッケージが図に含まれてしまいます**
A: `-i vendor`オプションを使用してベンダーディレクトリを無視してください。

**Q: 図が複雑すぎて読みにくいです**
A: `-x`オプションで外部インポートを除外するか、特定のパッケージのみを解析してください。

**Q: カスタムキーワードが適用されません**
A: キーワードファイルの形式を確認し、`category:keyword1,keyword2`の形式になっているか確認してください。

## ライセンス

このプロジェクトはMITライセンスの下で公開されています。詳細は[LICENSE](LICENSE)ファイルを参照してください。

## 謝辞

GoPlantUMLの開発に貢献してくださったすべての方々に感謝します。

---

**注記**: このドキュメントは[オリジナルのREADME](README.md)の日本語翻訳版です。最新の情報については英語版を参照してください。
