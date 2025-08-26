---
id:
title: Obsidianのブログ構築
draft: false
created: 2025-08-23T10:38
updated: 2025-08-25T10:09
---




# Obsidianのブログ構築

Obsidian Publish を使用せずにブログを構築する方法を検討します。ブログはSSGを前提に、サーバー管理コストを削減することを目指します。

## 構築方法の選択肢

以下の構成パターンが考えられます：

- Obsidian → GitHub → CloudFlarePages × Astro
- Obsidian → GitHub → CloudFlarePages × Quartz4
- Obsidian → GitHub → GitHub Pages × Astro
- Obsidian → GitHub → GitHub Pages × Quartz4

### 選択のポイント
- **公開サービス**: CloudFlarePages（使い慣れているため）
- **フレームワーク**: Quartz4（Obsidianの独自リンクをMDに変換する手間が少ないため）

## Quartz4の導入

### 参考ドキュメント
- [Quartz公式サイト](https://quartz.jzhao.xyz/)

### 環境設定での問題と解決

#### Node.jsバージョンの更新
```bash
# npmの更新
volta install npm@latest
```

#### Volta環境確認
```bash
volta list
# Node: v23.4.0 (default)
# npm: v11.5.2 (default)
# Yarn: v4.9.3 (default)
```

### Quartzの初期設定
```bash
npx quartz create
```
※ Obsidian Vaultのフルパスをsymlinkとして設定

### ローカルでの確認方法
```bash
# 基本コマンド
npx quartz build --serve

# デバッグ用
npx quartz build --serve -v
```

## 設定カスタマイズ

### slug設定について
- ファイル名がslugになる設定
- 日本語ファイル名はエンコードされるため、英語ファイル名を使用し、titleフロントマターでタイトルを日本語設定

### 日本語化の手順
1. `quartz.config.ts`で以下を変更:
   - `pageTitle`
   - `locale`を日本語に設定

2. `quartz/i18n/locales/ja-JP.ts`で表示名を「一覧」に変更


###  blogテンプレート用意
ファイル名を考えるのがめんどくさいのでUUIDで作りたい。
デフォルトだと日付ベースしかなくて記事の更新に沿ったスラッグも考慮する必要があり面倒。
テンプレートで作成したい。
何なら blogs ディレクトリに移動させたい。デフォルトはNotesディレクトリで新しいファイルができるため。

**理想**
テンプレートで新規ファイル作成（テンプレート展開とファイル移動とUUID設定）
↓
執筆開始
