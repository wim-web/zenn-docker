# zenn docker

素早くzenn + textlint + reveiwdogの環境を作成できます。

PRを出すことによってtextlint + reviewdogが走るようになります。

## 始め方

### タスクランナーについて

このリポジトリではタスクランナーに[cargo-make](https://github.com/sagiegurari/cargo-make)を採用しています。cargo-makeを使用することで簡単に構築できるようになるので使用をオススメします。（OS毎のバイナリも用意されていますので簡単にインストール可能です。）

バイナリをインストールした場合、`makers` コマンドは使用できず `cargo-make make` コマンドしか利用できないため、 `makers` スクリプトでラップしてあげることで同じように使うことができるようになります。

```
echo -e '#!/bin/bash\ncargo-make make "$@"' > /usr/local/bin/makers
chmod +x /usr/local/bin/makers
```

### cargo-makeを使う場合

```
git clone git@github.com:wim-web/zenn-docker.git .
```

```
makers welcome
```

```
makers preview
```

以下にアクセスしてpreviewがでれば成功です。

http://localhost:8888

### cargo-makeを使わない場合

```
git clone git@github.com:wim-web/zenn-docker.git .
```

```
docker build -t zenn:latest ./.docker
```

```
docker run --rm -v $(pwd):/work zenn:latest init
```

```
docker run --rm -v $(pwd):/work --entrypoint "npm" zenn:latest i
```

```
docker run --rm -v $(pwd):/work -p 8888:8000 zenn:latest preview
```

以下にアクセスしてpreviewがでれば成功です。

http://localhost:8888

### GitHubとの連携

zennとGitHubとの連携は以下を参考に設定してください。

https://zenn.dev/zenn/articles/connect-to-github

以上で準備は完了です。ブランチを切ってmasterにPRを出すことでtexlint + reviewdogが動き、mergeすることでzennに投稿されるようになります。

## cargo-make tasks

`makers --list-all-steps` - 定義済みタスク一覧を表示させる。

`makers preview` - port:8888でpreviewする。

`makers article` - 記事を作成する。引数可。

```
makers article -- --slug this-is-slug
```

`makers book` - 本を作成する。引数可。

```
makers book -- --slug this-is-slug
```

`makers tl` - textlintを走らせる。対象は `articles/**` & `books/**` 。引数可。

`makers attach` - コンテナーにattachする。