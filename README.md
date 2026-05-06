# AppControls

AppControls は、.NET Windows Forms アプリケーション向けの再利用可能なカスタムコントロールライブラリです。

業務アプリケーションで共通利用する UI コントロール、レイアウト部品、テーマ関連部品、画面共通部品をまとめて管理することを目的としています。

## 目的

このライブラリの目的は、複数の WinForms アプリケーションで利用する共通 UI 部品を一元管理し、画面デザイン、操作感、実装ルールを統一することです。

主な用途は以下のとおりです。

- 共通入力コントロール
- カスタムタイトルバー
- サイドナビゲーション
- テーマ対応ボタン
- ラベル付き入力部品
- ContextMenuStrip 用レンダラー
- 権限制御対応コントロール
- 共通レイアウト部品

## 対象フレームワーク

```xml
<TargetFramework>net8.0-windows</TargetFramework>
<UseWindowsForms>true</UseWindowsForms>
```

このライブラリは Windows Forms アプリケーション向けです。

## 推奨プロジェクト構成

```text
AppControls
├─ Controls
│  ├─ Buttons
│  ├─ Inputs
│  ├─ Navigation
│  ├─ TitleBar
│  └─ Containers
│
├─ Themes
│  ├─ AppTheme.vb
│  ├─ ButtonTheme.vb
│  ├─ NavigationTheme.vb
│  └─ ContextMenuTheme.vb
│
├─ Renderers
│  └─ AppContextMenuRenderer.vb
│
├─ Permissions
│  └─ CtrlIdSupport.vb
│
└─ README.md
```

## 命名方針

コントロール名は、用途が分かりやすく、特定のアプリケーションに依存しない名前にします。

例：

```text
ActionButton
LabeledTextBox
LabeledDateBox
LabeledNumericBox
SideNavigation
TitleBar
ContentPanel
```

特定の画面名、機能名、業務名に強く依存する名前は避けます。

## 設計方針

このライブラリは以下の方針で設計します。

1. 複数の WinForms アプリケーションで再利用できること
2. Public API はシンプルで安定していること
3. 見た目はテーマクラスで一元管理すること
4. 業務ロジックを UI コントロール内に持たせないこと
5. Visual Studio デザイナーで扱いやすいこと
6. デザイン時と実行時の両方で安全に動作すること
7. 権限制御用のプロパティは任意で利用できること

## 基本的な使い方

開発中はプロジェクト参照で利用します。

```xml
<ProjectReference Include="..\libs\AppControls\AppControls.vbproj" />
```

利用側で名前空間をインポートします。

```vbnet
Imports AppControls
```

使用例：

```vbnet
Dim button As New ActionButton()
button.Text = "検索"
button.Width = 120
button.Height = 36
```

## テーマの利用

各コントロールは、可能な限り共通テーマ定義を利用します。

例：

```vbnet
AppTheme.Apply(button)
```

または、特定コントロール用のテーマを利用します。

```vbnet
NavigationTheme.Apply(menuButton)
```

テーマクラスでは、色、フォント、余白、罫線、ホバー時の見た目などを一元管理します。

## 権限制御

権限制御が必要なコントロールには、必要に応じて `CtrlId` プロパティを持たせます。

例：

```vbnet
button.CtrlId = "F001_SEARCH"
```

アプリケーション側では、現在ログインしているユーザーの権限に応じて、コントロールの表示、非表示、有効、無効を制御します。

権限判定そのものは、通常このライブラリではなく、アプリケーション側で行います。

## デザイナー互換性

カスタムコントロールを作成する場合は、以下の点に注意します。

- Public な引数なしコンストラクタを用意する
- コンストラクタ内でデータベースアクセスを行わない
- `Load` イベント内に業務ロジックを書きすぎない
- デザイン時に実行できない初期化処理を避ける
- 必要に応じて `DesignMode` を確認する
- Public プロパティはシンプルに保つ

例：

```vbnet
Public Sub New()
    InitializeComponent()

    If Me.DesignMode Then
        Return
    End If

    InitializeRuntimeSettings()
End Sub
```

## NuGet パッケージ化

ライブラリが安定した段階で NuGet パッケージとして作成できます。

```bash
dotnet pack -c Release
```

開発中は `ProjectReference` による参照を推奨します。

安定版として利用する場合は、NuGet パッケージ参照に切り替えることもできます。

## バージョン管理方針

このプロジェクトはセマンティックバージョニングを採用します。

```text
Major.Minor.Patch
```

例：

```text
1.0.0
1.1.0
1.1.1
2.0.0
```

バージョン更新の目安：

- Patch：不具合修正
- Minor：新しいコントロール追加、互換性のある改善
- Major：互換性のない API 変更、動作変更

## リポジトリ利用方法

このリポジトリは、以下のいずれかの方法で利用することを想定しています。

1. アプリケーションソリューション内の Git Submodule
2. 開発中のプロジェクト参照
3. 安定後の NuGet パッケージ参照

推奨される開発時の構成：

```text
MainApp
├─ MainApp.sln
└─ src
   ├─ MainApp
   └─ libs
      └─ AppControls
```

## ライセンス

このプロジェクトは現在、個人利用または社内利用を想定しています。

公開リポジトリとして運用する場合は、このセクションを更新してください。
