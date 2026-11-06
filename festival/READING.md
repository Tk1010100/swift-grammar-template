# 読解ノート：自分のアプリのコード3箇所

> **黒田　貴子：** （自分の名前）
> **AI利用レベル：** 1（質問のみ。説明は自分の言葉で書く）
> **最終更新：** 2026-11-6

生成AIに書いてもらったコードでも、自分で書いたコードでもかまいません。自分のアプリの中から3箇所を選んで、次の3つの問いに答えてください。分からないところは、生成AIに「この行は何をしていますか」「この行を消すとどうなりますか」と聞いてかまいません。ただし、書くのは自分の言葉です。

> 💡 担任の先生の指示で、項目が増えることがあります。

---

## 箇所1：（例：品名を追加するボタンの処理）

```swift
if let last = out.last, last.value == t.value, mergedFlags.last == false {
    out[out.count - 1].value *= 2
    unlockPokedex(for: out[out.count - 1].value)
    mergedFlags[mergedFlags.count - 1] = true
    gained += out[out.count - 1].value
    mergeCount += 1
    changed = true
} else {
    out.append(t)
    mergedFlags.append(false)
}
```

**何をしているか：**
スワイプ方向に並べたタイルを１枚ずつ見ていき、直前のタイルと同じ数字で、まだ合体していなかった場合数字を２倍にし合体させる。その時に図鑑の開放、得点の加算、合体回数のカウントも一緒に行う。違う数字の場合そのまま。

**なぜこう書くのか：**
mergedFlagsで合体済みを持つことで、１回のスワイプで、１枚のタイルは１回しか合体しない2048ゲームのルールを守ることができる。
合体と図鑑の開放を同じ場所で行うため、合体が起きた瞬間に必ず図鑑解放も反映される。

**もしこう書かなかったら：**
mergedFlagsの判定がないと、「２、２、４」を左スワイプにしたときに２＋２＝４になった直後に、次に４とも合体して８になってしまう、本来は「４、４」になるところが判定がおかしくなってしまうため必要

---

## 箇所2：（見出し）

```swift
init() {
    Self.resetCollection()
}

var body: some Scene {
    WindowGroup { ContentView() }
        .onChange(of: scenePhase) { newPhase in
            if newPhase == .background { Self.resetCollection() }
        }
}

private static func resetCollection() {
    UserDefaults.standard.removeObject(forKey: "discoveredItems")
```

**何をしているか：**
アプリを起動した時と、ホーム画面などに離れた時に、図鑑の解放データdiscoveredItemsだけを削除する。
**なぜこう書くのか：**
展示会では不特定多数の人が交代で遊ぶため、前の人の図鑑データが残らないようにするために設置。
ホーム画面に離れた時点で消せばアプリ内でホームに戻ったタケでは消えずプレイ中の図鑑が保たれる。
起動時に消すのは、強制終了などで離れた処理を通らなかった場合の保険として設置。
removeObject(forKey:)のみで1つだけ消すため、ランキングやハイスコアなどは別のキーで残ります。
**もしこう書かなかったら：**
この処理がなかった場合次の人が遊び始めた時に、前の人が解放した図鑑が初めから見れてしまうため展示で使用するとなると試してくれる人が減少してしまう傾向が予測されたため設置しました。
ただし保存データをまとめて消す書き方にすると、ランキングからハイスコアまで消えてしまう。
---

## 箇所3：（見出し）

```swift
private func launchScroll() {
    guard !showScroll else { return }
    showScroll = true
    scrollScale = 0.0

    withAnimation(.easeOut(duration: 0.45)) { scrollScale = 1.05 }

    Task {
        try? await Task.sleep(nanoseconds: 450_000_000)
        withAnimation(.easeInOut(duration: 0.15)) { scrollScale = 1.0 }

        try? await Task.sleep(nanoseconds: 150_000_000)
        scrollFinished = true
        refreshTapLock()
    }
}
```

**何をしているか：**
巻紙を表示状態にして、大きさ０から1.05倍まで0.45秒で広げるその後0.15秒かけ、1.0倍に戻す。
最後に「巻紙が出た」という印scrollFinishedを立てて、タップを解禁しても問題ないかrefreshTapLockを呼び出し確認
**なぜこう書くのか：**
1.0倍で止めずに行ったん1.05倍にすることにより勢いよく飛び出して、着地する弾みができる。
先頭guard !showScrollは、煙が吹き出したタイミングの通知と、保険のwatchdogの両方から呼ばれても、飛び出しを１回だけにするため。
Task.sleepで、前のアニメーションが終わるのを待ってから、次を始めている。
scrollFinishedを最後に立てるのは、巻物が出きる前にタップでゲームが始まらないようにするため。
**もしこう書かなかったら：**
guardがなかった場合、２カ所から呼ばれた時に、巻紙がまた０から呼び出し直してしまう可能性がある。
1.05倍がなかった場合はただただ大きくなるだけの動きになるためデザイン。面白みがなくなる。
scrollFinishedをたててrefreshTapLockを呼ぶ部分がなかった場合、タップが解禁されずにゲームを始めることができなくなってしまう。
---

## 自分ではまだ説明できない箇所（正直に）

（アプリの中で「動いているけれど何をしているか分からない」ところを書く。ここに書いたものは、後期の文法の授業で分かるようになります。）
