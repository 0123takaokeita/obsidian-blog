---
title: Obsidianのブログ構築
draft: false
created: 2025-08-23T10:38
updated: 2025-08-24T19:26
---

Obsidian Publish を使用せずにブログ構築する手段を検討する。
ブログなのでSSGを前提に考える。サーバー管理コストを減らしたい。

### 手段
Obsidian → GitHub → CloudFlarePages x Astro
Obsidian → GitHub → CloudFlarePages x Quartz4
Obsidian → GitHub → GitHub Pages x Astro
Obsidian → GitHub → GitHub Pages x Quartz4

SSG用のFWをどうするか？と
公開用のサービスをどれにするか？が選択肢になる。

使ったこともあるに慣れているのでCloudFlareにする。
GitHub Pagesのほうが手軽らしいが、今回は新しく導入する手間も省きたい。


Astro については Obsidian の独自リンクをMDに変換する手間がかかるらしい。
Quartzは対応しているらしいからこちらで公開する。


公開手順がわかるDocuments
https://quartz.jzhao.xyz/

Version違いで失敗
```bash
└─> pnpm i

   ╭───────────────────────────────────────────────────────────────────╮
   │                                                                   │
   │                Update available! 9.5.0 → 10.15.0.                 │
   │   Changelog: https://github.com/pnpm/pnpm/releases/tag/v10.15.0   │
   │                 Run "pnpm add -g pnpm" to update.                 │
   │                                                                   │
   │         Follow @pnpmjs for updates: https://x.com/pnpmjs          │
   │                                                                   │
   ╰───────────────────────────────────────────────────────────────────╯

 ERR_PNPM_UNSUPPORTED_ENGINE  Unsupported environment (bad pnpm and/or Node.js version)

This error happened while installing a direct dependency of /Volumes/EXSSD/dev/github.com/jackyzha0/quartz

Your Node version is incompatible with "yargs@18.0.0".

Expected version: ^20.19.0 || ^22.12.0 || >=23
Got: v22.4.1

This is happening because the package's manifest has an engines.node field specified.
To fix this issue, install the required Node version.
Progress: resolved 63, reused 0, downloaded 57, added 0
Downloading pixi.js@8.12.0: 15.78 kB/12.32 MB
```

nodeは問題なさそうだけど、npmが古いっぽい
```bash
└─> volta list
⚡️ Currently active tools:

    Node: v23.4.0 (default)
    npm: v8.19.2 (default)
    Yarn: v3.3.1 (default)
    Tool binaries available:
        claude (default)
        swagger-cli (default)
         (default)
        live-server (default)
        neovim-node-host (default)
        npx (default)
        pnpm, pnpx (default)
         (default)
        vc, vercel (default)

See options for more detailed reports by running `volta list --help`.
```


npmがlatestに更新された。
Localマシンなのでバージョンは最新でも良い。
開発するときはDocker使うか、プロジェクト事にPin打つこと。
```bash
└─> volta install npm@latest
success: installed and set npm@11.5.2 as default

└─> volta list
⚡️ Currently active tools:

    Node: v23.4.0 (default)
    npm: v11.5.2 (default)
    Yarn: v4.9.3 (default)
    Tool binaries available:
        claude (default)
        swagger-cli (default)
         (default)
        live-server (default)
        neovim-node-host (default)
        npx (default)
        pnpm, pnpx (default)
         (default)
        vc, vercel (default)

See options for more detailed reports by running `volta list --help`.
```


このコマンドで初期設定ができるみたい。
Obsidian Vault のフルパスをsymlink設定することにした。
```bash
    npx quartz create

┌   Quartz v4.5.1
│
◆  Choose how to initialize the content in `/Volumes/EXSSD/dev/github.com/jackyzha0/quartz/content`
│  ● Empty Quartz
│  ○ Copy an existing folder
│  ○ Symlink an existing folder
└
```


localでの確認方法は以下のコマンド
書き込みするたびにちゃんとHotReloadが走るみたい。
```bash
npx quartz build --serve

or

npx quartz build --serve -v # for debug
```

あとは index.md がHomeになるようなので作っておく必要がある。
全体のリンクとかにする？要検討



### slug設定について
Quartz4 の設定では ファイル名がslugになる設定となっている。
そのためファイル名を日本語にするとエンコードされて意味の読めないSlugとなる。

調整方法を調べた感じだと、 ファイル名はSlugにして フォーマッターにtitle属性を設定すると記事のタイトルはtitle属性が表示されることがわかった。

つまり、この設定の場合はslugがtest
タイトルが `Obsidianのブログ構築` となる。
```test.md
---
title: Obsidianのブログ構築
created: 2025-08-23T10:38
updated: 2025-08-23T11:50
---
```


### 日本語化
デフォルトはもちろんUSなので２箇所設定変更する必要があった。
`quartz.config.ts` で  `pageTitle`   `locale` を変更
localを変更しても サイドバーがカタカナでエクスプローラーとなるだけだったため
`quartz/i18n/locales/ja-JP.ts` で  `一覧` に変更した。
