---
title: "mac上のAlacrittyでウィンドウでなくタブでが開いてしまう"
emoji: "🖥️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["alacritty", "tips"]
published: true
---

chatGPTが調べた結果ですが私の環境では直ったのでメモ。

## 解決方法

macOSの設定で`書類をタブで開く`をNeverにする。

関係あるか知らないけど、一応設定でショトカを追加。

```toml: ~/.config/alacritty/alacritty.toml
[keyboard]
bindings = [
  { key = "N", mods = "Control|Shift", action = "CreateNewWindow" }
]
```

## 原因

あの設定で書類を開いた判定になって、タブで開いてしまうよう。
