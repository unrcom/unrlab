# このサイトをどうやって作ったか

## 目的

6文字以内・ハイフンなしのドメインで、実験記録を公開するサイトを作る。

## この記事の要約

6文字以内・ハイフンなしのドメイン `unrlab.com` を Route 53 で取得し、GitHub Pages で公開した。作業は claude.ai/chat のガイドに従って進め、Web検索で手順を探す従来の方法より、時間を大きく短縮できた。

## やったこと

### 1. ドメイン名を決める

条件は「6文字以内」「ハイフンなし」とした。`unremoted.com` はすでに持っていたが、文字数が多く、`unremote.com` という別のドメインも存在して紛らわしかったため、新しい名前を探した。

候補を調べた結果は次のとおり。

- `ulab` `unlab` `trylab` `unrlabs`: すでに取得されていた
- `unrdev` `unrlab`: 空いていた

会社として「実験的に試す姿勢」を前面に出したかったので、`unrlab` を選んだ。最近 awslabs を触る機会が多く、「◯◯lab」という名前になじみがあったことも理由のひとつ。

### 2. 取得前の確認

- **商標**: J-PlatPat で「unrlab」「アンアールラボ」を検索し、どちらも0件だった
- **既存の名前**: 検索エンジンで「unrlab」を調べたが、同名のサービスや会社はなかった。近い綴りの「unilab」だけがヒットした

### 3. Route 53 でドメインを取得

Route 53 のドメイン登録画面で `unrlab.com` を検索し、「完全一致」で取得できることを確認した。料金は年間16 USD（更新も同額）。登録リクエストを送ると、ステータスは「進行中」になり、しばらくして登録完了のメールが届いた。このとき、ホストゾーンも自動で作成された。

### 4. GitHub リポジトリの作成

`github.com/unrlab` は2018年に他の誰かが作ったアカウントで使われていたため、組織名としては使えなかった。そこで、既存の `unrcom` 組織の下に `unrcom/unrlab` を作ることにした。ドメイン名とGitHubの名前は一致しなくても、独自ドメインで公開するので問題ない。

リポジトリは `gh` コマンドで公開リポジトリとして作成した。

```
gh repo create unrcom/unrlab --public --description "unrlab experiment logs" --clone
```

### 5. GitHub Pages で公開

最小の `index.html` を作って `main` ブランチにpushし、GitHub Pagesの設定で `main` ブランチのルート（`/ (root)`）から配信するようにした。まず `https://unrcom.github.io/unrlab/` で表示されることを確認した。

### 6. 独自ドメインの設定

先にGitHub Pagesの Custom domain に `unrlab.com` を登録し、そのあとRoute 53に次のレコードを追加した。

- `unrlab.com`: Aレコード。GitHub PagesのIPアドレス4つ（`185.199.108.153` `185.199.109.153` `185.199.110.153` `185.199.111.153`）
- `www.unrlab.com`: CNAMEレコード。値は `unrcom.github.io`

`dig` でDNSが反映されたことを確認し、`http://unrlab.com` でサイトが表示されることも確認した。

## 結果

- `https://unrlab.com` でサイトが表示されるようになった。GitHub Pagesの「Enforce HTTPS」も有効にした
- GitHub側の「DNS check successful」が出て、TLS証明書も発行された
- `www.unrlab.com` も、`dig` でGitHub PagesのIPアドレスに解決されることを確認した
- 費用はドメイン代の年間16 USDのみ。GitHub Pagesは公開リポジトリなら無料

## ハマったところ

特になし。

## わかったこと

従来は、Web検索で作業手順を探し、理解してから実際の作業を進めていた。今回は claude.ai/chat のガイドに従って進めたところ、構築にかかる時間をかなり短縮できた。

GitHub のリポジトリを作成したあとの作業は、Claude Code に移すと、さらに作業時間を短縮できたと思われる。
