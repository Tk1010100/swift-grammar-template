# 日専祭アプリ：（アプリ名をここに書く）

> **猫のクボ太　メンバー：窪田、伍、黒田** （メンバーの氏名。個人制作なら自分の名前だけ）
> **AI利用レベル：** 2（生成コード可）
> **最終更新：** 2026/00/00

> 💡 制作の進め方は担任の先生から案内があります。このファイルの項目は、担任の先生の指示で増えることがあります。

---

## 何ができるアプリか（3行）
パズルゲーム2048風のマージゲーム。
マス目上でタイルをスライドさせ、同じアイテムを重ね合体。
ハロウィンの世界の中で遊べる。

（例：買い物リストを作るアプリ。品名を入力して追加できる。買ったものにチェックを付けると一覧から消える。）


<img width="1206" height="2622" alt="Simulator Screenshot - iPhone 17 Pro - 2026-10-26 at 08 12 53" src="https://github.com/user-attachments/assets/aef6c00d-455b-4f99-9c41-a383220af6d7" />
<img width="1206" height="2622" alt="Simulator Screenshot - iPhone 17 Pro - 2026-10-26 at 08 12 33" src="https://github.com/user-attachments/assets/78d812e5-5f9a-4e1c-b16e-4bf722d599f6" />
<img width="1206" height="2622" alt="Simulator Screenshot - iPhone 17 Pro - 2026-10-26 at 08 12 39" src="https://github.com/user-attachments/assets/1902d71d-67b5-4e08-8514-78804f2a9468" />
<img width="1206" height="2622" alt="Simulator Screenshot - iPhone 17 Pro - 2026-10-26 at 08 12 45" src="https://github.com/user-attachments/assets/8b79a790-56f7-46e8-a42c-97ed2a0cfb18" />

## 使った文法・技術
@State、@Published、@ObservedObject、VStack / HStack / ZStack、ForEach、List、Text / TextField、Button、2次元配列（[[Int]] など）、for-in 文、if-else 文 / switch 文、guard 文、Class（クラス） / Struct（構造体）、Closure（クロージャ）

## 生成AIの使い方（どの場面で、どう使ったか）
座標変換の設計で使用。
４方向のスワイプの処理に関して本来であれば１方向につき1つの処理を書かなければならないところ。どうにかこうにかコードを簡潔的にさせ短くできないかと相談。結果1つのループにすることができた。


## 展示で工夫した点
・ニックネーム入力時にNGワードが入らないように制御装置をつけた。

・図鑑に関してはホーム画面に戻った際に自動リセットがかかるようにしてあるがランキングなどは削除されないように工夫

・ゲーム説明画面にて説明が不要な人は3秒長押し判定でスキップそれ以外は下までスクロールをしないと進めないようにブロックをかけた。
