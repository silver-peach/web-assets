# web-assets

# Web Assets Management

このリポジトリは、`silver-peach` 組織の各アプリ（`arcnum` など）が共通で参照する、静的ファイル（利用規約、プライバシーポリシー、リモート設定用のJSON等）を管理・配信するためのリポジトリです。

GitHub Actions を利用し、ブランチごとに自動で配信URLが仕分けられる仕組みになっています。

---

## 📂 リポジトリ内のフォルダ構成

ファイルを編集・追加する場合は、必ず `assets/` ディレクトリの配下に配置してください。ルート直下のファイルはWeb上に公開されません。

```text
web-assets (リポジトリ直下)
├── .github/workflows/deploy.yml  # 自動デプロイ指示書（編集不要）
├── assets/                       # ★この中身だけがWeb公開対象になります
│   └── arcnum/                   # 各アプリ名のフォルダを作成
│       ├── config.json           # アプリの設定・バージョン管理用JSON
│       ├── ja/
│       │   ├── eula.html         # 利用規約本文
│       │   └── pp.html           # プライバシーポリシー本文
│       └── en/
│           ├── eula.html         # 利用規約本文
│           └── pp.html           # プライバシーポリシー本文
└── README.md                     # 本ドキュメント
```

---

## config.json ファイルのフォーマット

```text
{
  "eula_version": "1.0.0",
  "eula_urls": {
    "ja": "ja/eula.html",
    "en": "en/eula.html"
  },
  "pp_version": "1.0.0",
  "pp_urls": {
    "ja": "ja/pp.html",
    "en": "en/pp.html"
  }
}
```

---

## 🌐 ブランチ運用と配信URL（完成形）

`main` ブランチと `develop` ブランチへのプッシュ（またはマージ）をトリガーに、GitHub Actions が自動でフォルダ構造を組み替えて GitHub Pages へデプロイします。

### 1. main ブランチ（本番環境用）
本番アプリが一生見に行く固定URLです。絶対に直接テストなどで数値を汚さないでください。

* **規約・設定JSON**: `https://silver-peach.github.io/web-assets/arcnum/config.json`
* **利用規約HTML**: `https://silver-peach.github.io/web-assets/arcnum/ja/eula.html`
* **プライバシーポリシーHTML**: `https://silver-peach.github.io/web-assets/arcnum/en/pp.html`

### 2. develop ブランチ（開発・テスト環境用）
手元の開発PCやシミュレータでテストするためのURLです。自動でパスの途中に `/dev/` が挟み込まれます。

* **規約・設定JSON**: `https://silver-peach.github.io/web-assets/arcnum/dev/config.json`
* **利用規約HTML**: `https://silver-peach.github.io/web-assets/arcnum/dev/en/eula.html`
* **プライバシーポリシーHTML**: `https://silver-peach.github.io/web-assets/arcnum/dev/ja/pp.html`

---

## 🚀 開発とリリースの流れ

1. **開発・検証時**
   `develop` ブランチにチェックアウトし、`assets/` 内のファイルを編集してプッシュ（`git push`）します。
   自動的にテスト環境用（`/dev/` 付き）のURLが更新されるため、Flutterのデバッグビルド等で動作確認を行います。
2. **本番リリース時**
   テストが完了したら、GitHub上で `develop` ブランチから `main` ブランチへの Pull Request を作成し、マージ（Merge）します。
   これにより本番用のURLが一斉に更新され、世界中の本番ユーザーに新しい設定が反映されます。

