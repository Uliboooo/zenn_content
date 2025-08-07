---
title: "セットアップ"
---

## install

https://www.rust-lang.org/learn/get-started

ulix系osは以下コマンドで。windowsは👆のリンクにGUIのインストーラがあります。visual studio系のインストールを求められます。

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## Editor

vscodeがおそらく最も楽。[rust-analyzer(静的解析)](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)と[lldb(デバッガ)](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)の拡張機能を入れれば最低限の環境がたつ。

個人的に便利な拡張機能。あんま入れすぎても重くなるのと管理できなくなるのでこれくらいで。

- [catppucion for VSCode](https://marketplace.visualstudio.com/items?itemName=Catppuccin.catppuccin-vsc) & [Icons](https://marketplace.visualstudio.com/items?itemName=Catppuccin.catppuccin-vsc-icons) - 個人的に好きなテーマ。可愛さと可読性のバランスがいい
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) - スペルチェッカー
- [Dependi](https://marketplace.visualstudio.com/items?itemName=fill-labs.dependi) - 依存ライブラリの最新情報取得
- [Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) - tomlサポータ。rustはtoml大好きなのであれば便利。
- [git graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph) - git logをグラフィカルに表示可能。
- [Rust Test Explorer](https://marketplace.visualstudio.com/items?itemName=swellaby.vscode-rust-test-adapter) - あんまり使わないけど、プロジェクト内のテストコードを一箇所から触れる
- [Vim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) - vim keybind

:::details Code Spell Checkerで日本語を避ける
デフォでは全角の日本語がチェッカーに引っかかるので、以下設定をvscode設定に追記してオフにするといい。

```json
"cSpell.ignoreRegExpList": [
    "[０-９Ａ-Ｚａ-ｚぁ-んァ-ヶ亜-熙纊-黑]+"
],
```

:::

## Cargo(rustプロジェクトのほぼ全てを握るコマンド)

先ほどのrustインストールにて自動で入った`cargo`コマンドですが、ビルドから実行、crates.ioへのpublishなどおおよそすべての操作を行います。

```bash
# プロジェクトの新規作成
cargo new <Program-name>

# libの作成
cargo new --lib <lib-name>

# 実行ファイルを生成せずにコンパイル可能かの判断。爆速
# rustのコンパイルはそこそこ遅いので有用(とは言いつつrust-analyzerが有能なのであんまり出番はないかも)
cargo check

# build
cargo build

# release build
cargo build --release

# buildで生成されたファイルを削除
cargo clean

## run(buildと実行を一回で)
cargo run

# test
# 関数名を入れなければ全部のテスト(だった気がする)
cargo test [test-func-name]

# libの導入
# 正直vscodeとかサポートツールを使っているとCargo.tomlに書いた情報から勝手に導入されるので、ほぼ使ってない
cargo add [crate-name@version]

# docの生成
# rustでは`///`から始まるコメントは下に記述された関数などの説明用のドキュメントになり、以下コマンドでそれをまとめて生成してくれる
cargo doc

# crates.ioに公開
# 詳細は下記リンク
cargo publish
```

## プロジェクトの新規作成からHello Worldまで

以下コマンドで"hello"というプロジェクトを作成

```zsh
cargo new hello
```

作ってvscodeなどで開くと以下のファイルが準備されます。

```zsh
.
├── Cargo.lock   👈 依存関係の厳密な定義(基本触らない)
├── Cargo.toml   👈 依存関係の定義
├── src          👈 ソースコードのフォルダ
│   └── main.rs  👈 エントリポイント。main.rsのmain()が呼ばれる
├── .gitignore   👈 勝手に作られる。/targetが自動で設定されている
└── target       👈 buildなどで生成されたものが入る
```

cargo newで生成されるmain.rsにはすでにHello Worldなプログラムが含まれるので、ちょっと改造します。

```rust:main.rs
fn main() {
    let mut name = String::new();     // 空の文字列を生成
    std::io::stdin()
        .read_line(&mut name)         // stdのreadlineを使って標準入力をname変数へ
        .expect("Failed read line");
    println!{"Hello, world! {name}"}; // 表示
}
```

終わり。
