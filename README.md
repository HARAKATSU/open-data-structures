<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [open-data-structures](#open-data-structures)
    - [1.2 インターフェース](#12-%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9)
      - [インターフェース interface](#%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9-interface)
      - [実装 implementation](#%E5%AE%9F%E8%A3%85-implementation)
      - [Queueインターフェース :cucumber:](#queue%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9-cucumber)
      - [Listインターフェース：線形シーケンス :bicyclist:](#list%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9%E7%B7%9A%E5%BD%A2%E3%82%B7%E3%83%BC%E3%82%B1%E3%83%B3%E3%82%B9-bicyclist)
      - [USetインターフェース :us:](#uset%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9-us)
      - [SSetインターフェース :bar_magnet:](#sset%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9-bar_magnet)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# open-data-structures
![test](https://cdn.shopify.com/s/files/1/1634/7169/products/cover-0_160x.png?v=1531786466)

### 1.2 インターフェース
- データ構造のインターフェースと実装について
  - インターフェース…データ構造が **何をするか** を表現
  - 実装…データ構造がそれを **どのようにやるか** を表現
- 本書で実装するインターフェースをすべて説明
  - 必ず読むこと！

#### インターフェース interface
- a.k.a **抽象データ型 abstruct data type**
- 何？→以下を定義するもの
  - あるデータ構造が持つ操作一式
  - それらの操作の意味（セマンティクス）

```
## データ構造のインターフェース

インターフェース
┣━Listインターフェース
┃ ┗━Queueインターフェース
┃   ┣━FIFOキュー
┃   ┣━優先度付きキュー
┃   ┣━LIFOキュー（Stack、スタック）
┃   ┗━Deque（双方向キュー）
┗━┳━USetインターフェース
  ┗━SSetインターフェース

```

#### 実装 implementation
- 何？→以下を定義するもの
  - データ構造の内部表現
  - 実際に操作を行うアルゴリズム

---
#### Queueインターフェース :cucumber: 
- 何？→追加、削除ができる要素の集まり
- できる操作
  - `add(x)`: 値xをQueueに追加
  - `remove()`: （以前に追加された）「次の値」yをQueueから削除し、yを返す
    - 引数を取らず、取り出し規則（FIFO、優先度付き、LIFO）に従って削除対象が決まる
- 取り出し規則
  1. **FIFO** キュー
    - first-in-first-out（先入れ先出し）
     - 追加順と同じ順番で要素を削除する
    - 単に「 **キュー** 」といえばこれ
  2. 優先度付きキュー
    - 最小の要素を削除する
  3. **LIFO** キュー
    - last-in-first-out（後入れ ~~サクサク~~ 先出し）
      - 最後に追加された要素が最初に削除される
    - 「 **スタック** 」といえばこれ
  4. Deque（双方向キュー）
    - 先頭または末尾（に/の）要素を（追加/削除）できる

操作の名前

||Queue|FIFO|PQ|LIFO|Deque|
|---|---|---|---|---|---|
|値を追加|add(x)|add(x), enqueue(x)|add(x)|push(x)|addFirst(x), addLast(x)|
|値を削除|remove()|remove(), dequeue()|remove(), deleteMin()|pop()|removeFirst(), removeLast()|

---
#### Listインターフェース：線形シーケンス :bicyclist: 
- 値の列`x_0, ..., x_n-1`と、その列に対する以下の操作からなるインターフェース
  1. `size()`：リストの長さを返す
  2. `get(i)`：`x_i`の値を返す
  3. `set(i, x)`：`x_i`の値を`x`にする
  4. `add(i, x)`：`x`を`i`番目として追加し、`x_i, ..., x_(n-1)`を後ろにずらす
    - すなわち j∈{`i, ..., n-1`} について`x_(j+1) = x_j`とし、`n`を一つ増やし`x_i = x`とする
  5. `remove(i)`：`x_i`を削除し、`x_(i+1), ..., x_(n-1)`を前にずらす
    - すなわち j∈{`i, ..., n-2`} について`x_j = x_(j+1)`とし、`n`を一つ減らす
- Queue, Stack, DequeなどのインターフェースはListインターフェースにまとめられる
  - ex. Dequeインターフェースの実装
    - `addFirst(x)`→`add(0, x)`
    - `removeFirst()`→`remove(0)`
    - `addLast(x)`→`add(size(), x)`
    - `removeLast()`→`remove(size()-1)`

---
#### USetインターフェース :us:
- 重複がなく、順序付けられていない要素の集まり（U…unordered）
  - 数学における集合のようなもの
- 実行できる操作
  1. `size()`：集合の要素数`n`を返す
  2. `add(x)`：要素`x`が集合に入っていなければ集合に追加する
    - `x=y`を満たす集合の要素`y`が存在しないなら集合に`x`を加える
    - `x`が集合に追加されたら`true`、そうでなければ`false`を返す
  3. `remove()`：集合から`x`を削除する
    - `x=y`を満たす集合の要素`y`を探し、集合から取り除く
    - そのような要素を発見したら`y`、しなければ`null`を返す
  4. `find(x)`：集合に`x`があればそれを見つける
    - `x=y`を満たす集合の要素`y`を探す
    - そのような要素を発見したら`y`、しなければ`null`を返す
- 探したい`x`と見つける要素`y`をわざわざ区別する理由
  - 別のモノ(object)である両者を、 __何らかの基準で等しいと判別したい場合__ を想定しているため
    - キーを値に対応づけるインターフェース（ **辞書** 、 **マップ** ）の実装に都合がよくなる

---
#### SSetインターフェース :bar_magnet: 
- 順序付けられた要素の集まり（S…sorted）
- 全順序集合（任意の2要素`x`, `y`について大小を比較できる集合）の要素が入る
  - 本書では下記のように定義される`compare(x, y)`メソッドで比較する

<img src="https://latex.codecogs.com/gif.latex?compare(x,&space;y)&space;=&space;\left\{&space;\begin{array}{rcl}&space;<0&space;&&space;if&space;&&space;x<y\\\&space;>0&space;&&space;if&space;&&space;x>y&space;\\\&space;=0&space;&&space;if&space;&&space;x=y&space;\end{array}&space;\right."/>

- 実行できる操作
  1. `size()`：USetと同じセマンティクス
  2. `add(x)`：同上
  3. `remove()`：同上
  4. `find(x)`：集合における`x`の位置を特定する
    - `y≥x`を満たす最小の要素`y`を探す
    - そのような要素を発見したら`y`、しなければ`null`を返す
    - **後継探索** （ **successor search** ）とも呼ばれる
    - `x`と等しい`y`がなくても値を返す点でUSetの`find(x)`と異なる
- USetより実装が複雑なため実行時間が長くなりがち

---
各インターフェースがサポートしている操作とその引数

||add|remove|size|get|set|find|
|---|---|---|---|---|---|---|
|Queue|(x)|()|-|-|-|-|
|List|(i, x)|(i)|()|(i)|(i)|-|
|USet|(x)|(x)|()|-|-|(x)|
|SSet|(x)|(x)|()|-|-|(x)|
