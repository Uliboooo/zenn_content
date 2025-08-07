---
title: "Rust開発環境の構築, tools"
---

## 注意

このシリーズを通して記事の内容はあくまで私のrustに対する理解であり、なるべく確認はしていますが事実と異なる場合もあります。プロダクトコードに利用する際はご注意ください。
また、非推奨なコードや非効率なコードを見つけた場合、その記事のコメントやXのDMなどで知らせてくれるとありがたいです。

## Rustとは

**コンパイラ型で関数型,手続型,オブジェクト指向を取り入れたマルチパラダイム言語**

- 2006年ごろに、Mozillaの従業員であったGraydon Hoareの個人プロジェクトとして登場
- 2009年にMozillaが開発に関わり始め、2015年にv.1.0がリリース
- それ以降は後方互換を保って6週間間隔でリリース
- c/c++のようなシステムプログラミング言語
- 所有権システムを用いて、GCなしでメモリ安全性やマルチスレッド安全性を保証している

## インストールせず

試すだけなら以下サイトでも。

https://play.rust-lang.org/?version=stable&mode=debug&edition=2024

## コンパイラとか

比較的楽です。

unix系は以下コマンドで、windowsはGUIのインストーラから。一応以下サイトからインストールコマンドは確認して。

https://www.rust-lang.org/ja/learn/get-started

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## IDE

vscodeが楽ではある。一応使ったことがあるものも書いとく。

### vscode

https://code.visualstudio.com

以下の拡張機能をインストール

rustコードの静的解析(必須)

https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer

デバッガ(llvmのデバッガでRustやc, c++などに利用可能, (必須))

https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb

これ以降はあれば便利なものたち。rust関係ないものも多。

catppucion for VSCode & Icons - 個人的に好きなテーマ。可愛さと可読性のバランスがいい

https://marketplace.visualstudio.com/items?itemName=Catppuccin.catppuccin-vsc

https://marketplace.visualstudio.com/items?itemName=Catppuccin.catppuccin-vsc-icons

Code Spell Checker - スペルチェッカー

https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker

:::details 日本語を避ける
デフォでは全角の日本語がチェッカーに引っかかるので、以下設定をvscode設定に追記してオフにするといい。

```json
"cSpell.ignoreRegExpList": [
    "[０-９Ａ-Ｚａ-ｚぁ-んァ-ヶ亜-熙纊-黑]+"
],
```

:::

Dependi - 依存ライブラリの最新情報取得

https://marketplace.visualstudio.com/items?itemName=fill-labs.dependi

Even Better TOML - tomlのサポート。rustはtoml大好きなので触る機会が多い。

https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml

git graph - gitをグラフィカルなツリーにしてくれる

https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph

Rust Test Explorer - あんまり使わないけど、プロジェクト内のテストコードを一箇所から触れる

https://marketplace.visualstudio.com/items?itemName=swellaby.vscode-rust-test-adapter

Vim - vim keybindをvscodeで使いたい人はおすすめ

https://marketplace.visualstudio.com/items?itemName=vscodevim.vim

### Zed

https://zed.dev

Rustで書かれたエディタ。ものすごい勢いで発展している。まだ人口が少ないからvscodeより拡張性が低いが、シンプルさや高速性を求めているなら試してみるといいかも。バイナリ配布はmac, linuxのみなのでwindowsは自前でビルドが必要。全機能が動くは知らないです。

こちらはRust製ということもあり、そのままrustが書ける。また最近デバッガも実装されたので将来性有り。

## vim(というよりNeoVim)

使う人がいるかは知りませんが一応。`"mrcjkb/rustaceanvim"`というプラグインがあるのでそれを導入して、設定をいじればいける。具体的な話は調べてください。(一応動く環境は以下におくけれど私のvim理解度が低いので自信なし)

https://github.com/Uliboooo/dotfiles/tree/main/usr/.config/nvim

## `Cargo` Rustプロジェクトに関するおおよそ全てを担う

先ほどのrustのインストーラによりプロジェクトの作成からbuild, test, libの導入、crates.io[^1]への公開など多くの操作をする。

[^1]: Rustのlibのリポジトリ

```bash
# rustプロジェクトの新規作成 `--lib`をつければライブラリになる
cargo new <Project-name>

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

https://doc.rust-jp.rs/book-ja/ch14-02-publishing-to-crates-io.html

## プロジェクトの新規作成からHello Worldまで

以下コマンドでhelloというプロジェクトを作成

```bash
cargo new hello
```

作ってvscodeなどで開くと以下のファイルが準備されます。

```
.
├── Cargo.lock   👈 依存関係の厳密な定義(基本触らない)
├── Cargo.toml   👈 依存関係の定義
├── src          👈 ソースコードのフォルダ
│   └── main.rs  👈 エントリポイント。main.rsのmain()が呼ばれる
├── .gitignore   👈 勝手に作られる。/targetが自動で設定されている
└── target       👈 buildなどで生成されたものが入る
```

cargo newで生成されるmain.rsにはすでにhelloworldが含まれるので、ちょっと編集してみます。

```rust:main.rs
fn main() {
    let mut name = String::new();     // 空の文字列を生成
    std::io::stdin()
        .read_line(&mut name)         // stdのreadlineを使って標準入力をname変数へ
        .expect("Failed read line");
    println!{"Hello, world! {name}"}; // 表示
}
```

pythonのようなinput()関数はないのでこの辺は若干不便です。
