---
title: "変数 データ型 演算"
---

## 変数はデフォルトで不変

rustにおいて変数はデフォルトで不変(imutable)です。これは関数型の特徴を受けており、データの不変性により処理による副作用を減らします。しかし、データを変更したいことも多いです。そういった際は`mut`キーワードを変数名の前に記述することで、変数の内容が変更される可能性をコードをみた開発者に明示的に知らせます。

```rust
let a = 100;
a += 1; // 👈 コンパイルエラーになる

// aの値を書き換えたいのなら
let mut a = 100;
a += 1; // 101
```

ちなみに「Rust開発環境の構築, tools"」にてすでにmutableな変数は登場しています。

```rust:main.rs
fn main() {
    let mut name = String::new();     // ここで定義した変数を
    std::io::stdin()                  //       👇
        .read_line(&mut name)         // ここでread_line()関数によって書き換え(入力を束縛)する
        .expect("Failed read line");
    println!{"Hello, world! {name}"};
}
```

## データ型

rustは強い静的型付けと言われる言語で、コンパイル時にすべての値の型を決定します。また、rustc(rustのコンパイラ)は強力な型推論を持っており、変数宣言において多くの場合型注釈をする必要はありません。rustで型を書くのは関数定義における引数と戻り値、構造体などのデータ構造表現の場合です。また関数の戻り値の型からも推論してくれたりします。ただし、暗黙的な型変換は行われず、i32->i64なども行われないです。

:::details どれほど強力か

ここで示す例を一旦markdown上で書いていたのですがこのコードはエラーになりません。

一見`num`が`i32`で`num2`が`i64`なのでエラーになりそうですが3行で`i64`と加算することがわかるので、`num`は`i64`として推論されるようです。他言語は知りませんが、割と強力な方なんですかね?

```rust
let num = 100; // 👈rustcは100を`i32`と推論する
let num2: i64 = 100;
let res = num + num2; //型が違うのでエラーになる
```
:::

このコードは`i32`である`num`と`i64`である`num2`を加算しようとして型が合わないため以下のエラーを発生させます。

```rust
let num: i32 = 100;
let num2: i64 = 100;
let res = num + num2;
```

```rust
error[E0308]: mismatched types
 --> src/main.rs:6:21
  |
6 |     let res = num + num2;
  |                     ^^^^ expected `i32`, found `i64`

error[E0277]: cannot add `i64` to `i32`
 --> src/main.rs:6:19
  |
6 |     let res = num + num2;
  |                   ^ no implementation for `i32 + i64`
  |
  = help: the trait `Add<i64>` is not implemented for `i32`
  = help: the following other types implement trait `Add<Rhs>`:
            `&i32` implements `Add<i32>`
            `&i32` implements `Add`
            `i32` implements `Add<&i32>`
            `i32` implements `Add`
```