---
title: "制御フロー"
---

## if (`if cond {} else if cond {} else {}`)

- ifは**式**であり値を返すことができる
- なのでletの値にifを設置して良い
- しかしelseフィールドがないとエラーになる
  - 条件にマッチしなかった際に束縛出来ないためコンパイルエラーになる
- Rustは式内で最後に評価された値を返すので返したい値は`;`で**文にしないよう**にする
- 束縛しないならif単体で使用可能

```rust
let age = 17;

let age_class = if age > 20 {
    println!("you are adbuls");
    "adults"
} else if age > 18 {
    println!("you are child but you have the vote.");
    "adults but child"
} else {
    println!("you are child");
    "child"
};

let age_class = if age > 20 {
    "adults"
}; // elseになった際にbindできなためエラー
```

## match (`match cond {case..., case...,}`)

- 条件に並列的にマッチするか判断する
- フィールドは取りうる値全てを網羅的に記述する必要がある
  - `_`フィールドを作ることでそれ以外を示すフィールドを作れる
- 条件ごとに`=>`に続く処理を行う
- 複数行になる場合は`{}`で囲む
- `match`も式なので値を返せる

```rust
let buying = "apple";

let class = match buying {
    "apple" => "fruits",
    "orange" => "fruits",
    "pumpkin" => {
        println!("more eat fruits!");
        "vegetables"
    }
    _ => "others",
};
```

## for (`for i in iter {}`)

- 繰り返すもの(Rust的にはイテレータを持つ値)を順に繰り返す
- 多くの場合は配列やベクターを回す時に使う
- ただ、`map()`メソッドなどの方が効率的な場合がある
- 値を探すのも`.find()`などの方が便利で効率的な場合が多い
- for(whileもloopも基本的には)は値を返さない(コレクションも)
- 値を返すことはできないが`break;`はあるので抜けることはできる

```rust
let ar = vec![1,2,3,3,5];
let mut sum = 0;
for i in ar {
    println!("i={}"; i);
    sum += i;
}
println!("sum:{}");
```

## while (`while cond {}`)

- 条件がtrueである限り繰り返す
- 値を返さない(コレクションも)
- 値を返すことはできないが`break;`はあるので抜けることはできる

```rust
let mut age = 20;

while age < 20 {
    age += 1;
    println!("you are {}", age);
}
```

## loop (`loop {}`)

- 無限ループ
- loopも値を返すことはできないが、`break value`で返せる

```rust
let mut c= 0;
loop {
    c += 1;
    println!("I CANNOT ESCAPE THIS LOOP!!!!{n}");
}
```

:::details なぜ`while true`とかじゃない?
私の推測ではあるが、無限ループという危ない機能を`while true`のような他のsyntaxと分けることでプログラマーに明示的に無限ループの存在を認識させている。
印象としてRustは明示的な記述を好む気がする。
:::
