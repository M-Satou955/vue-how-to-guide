# Vue3 のドキュメント　のガイドを見ながら Vue をやっていく

https://ja.vuejs.org/guide/introduction.html

## 現状＆目標

現状

- Vue を触ったことはあるが、かなりなんとなくで書いている。

目標

- 振り返りを兼ねて＆案件で Vue2 が多かったので Compositon Api と仲良くなる

メモの取り方

- このファイルにメモる
  - 一番雑にかける
- 他の.md ファイル（例: vue_frontend_terms）にメモする
  - これは用語などある程度まとまりがあるもの
- .vue ファイル内のカスタムブロックに lang="md"を設定してかく
  - ファイルで使っている記法について書く場合

## Vue についての基礎知識

### Vue が備える次の 2 つのコア機能

- 宣言的レンダリング

  - 公式の説明 : Vue では、標準的な HTML を拡張したテンプレート構文を使って、HTML の出力を宣言的に記述することができます。この出力は、JavaScript の状態に基づきます。

- リアクティビティー
  - 公式の説明 : Vue は JavaScript の状態の変化を自動的に追跡し、変化が起きると効率的に DOM を更新します。

### Vue のできること

Vue は、フロントエンド開発に必要な一般的な機能のほとんどをカバーするフレームワークであり、エコシステムだが web は極めて多様。構築するものもめちゃくちゃあるので色々できるように作られている

- ビルドステップなしで静的な HTML を拡充する
- 任意のページに Web コンポーネントとして埋め込む
- シングルページアプリケーション（SPA）
- フルスタック / サーバーサイドレンダリング（SSR）
- Jamstack / 静的サイトジェネレーション（SSG）: 今って Jamstack って言うのか？
- デスクトップ、モバイル、WebGL、さらにはターミナルをターゲットとする開発

### 単一ファイルコンポーネント(SFC)

単一ファイルコンポーネントの役割

- Vue コンポーネントのテンプレート、ロジック、スタイルを一つのファイルにカプセル化できる特別なファイル形式
- 関心の分離してなくねという疑問にも答えてて、フロントエンドの複雑化に伴い、ファイルタイプの分離では、関心の分離の目的であるコードベースの保守性の向上にあまり役に立たない。
- 現代の UI 開発では、コードベースを互いに織り交ぜる 3 つの巨大なレイヤーに分割するのではなく、それらを疎結合なコンポーネントに分割して構成する方がはるかに理にかなっているとのこと

### API スタイルが２つ？どういうこと？

Vue コンポーネントを作成する際に 2 種類の API スタイルが選択できるらしい

- Options API
- Composition API

#### Options API

Options API では,data、methods、mounted といった数々のオプションからなる
1 つのオブジェクトを用いてコンポーネントのロジックを定義(めちゃくちゃ見覚えがある)

data、method などのオプションで定義されたプロパティには、コンポーネントのインスタンスを指す this を使って関数内からアクセスする

※プロパティとは名前（キー）と値（バリュー）が対になったもの。ここでは count という key と 0 というバリューが定義されている
https://jsprimer.net/basic/object/#property-access

例

```vue
<script>
export default {
  // data() で返すプロパティはリアクティブな状態になり、
  // `this` 経由でアクセスすることができます。
  data() {
    return {
      count: 0,
    };
  },

  // メソッドの中身は、状態を変化させ、更新をトリガーさせる関数です。
  // 各メソッドは、テンプレート内のイベントハンドラーにバインドすることができます。
  methods: {
    increment() {
      this.count++;
    },
  },

  // ライフサイクルフックは、コンポーネントのライフサイクルの
  // 特定のステージで呼び出されます。
  // 以下の関数は、コンポーネントが「マウント」されたときに呼び出されます。
  mounted() {
    console.log(`The initial count is ${this.count}.`);
  },
};
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

#### Composition API

Composition API では、インポートした各種 API 関数を使用してコンポーネントのロジックを定義する。
SFC において、Composition API は通常`<script setup>` と組み合わせて使用する。
setup という属性をつけることで,Vue にコンパイル時の変形操作（コンパイルして CSS,JavaScript と HTML のファイルにすること？）を実行してほしいというヒントを伝えられるらしい。

これによって定型的な書式がすくなり、記述が少なくなる

options API と同じコンポーネントを、テンプレート部分は同一のまま、Composition API と `<script setup>` に置き換えたサンプル

```vue
<script setup>
import { ref, onMounted } from "vue";

// リアクティブな状態
const count = ref(0);

// 状態を変更し、更新をトリガーする関数。
function increment() {
  count.value++;
}

// ライフサイクルフック
onMounted(() => {
  console.log(`The initial count is ${count.value}.`);
});
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

Composition API は React Hooks と同レベルのロジック構成機能があるが重要な違いがある
React Hooks はコンポーネントが更新されるたびに繰り返し実行されるなど、熟練した開発者でも予期せぬ挙動が起こり得るパターンがいくつもある。

- Hooks は呼び出し順序に敏感で、条件付きではない

Vue Composition API
setup() または`<script setup>`
のコードを一度だけ呼ぶ出すので、stale クロージャーなどの問題などは発生しない。
Composition API の呼び出しは、呼び出しの順番に関係なく、条件付きで呼び出せる

Vue Composition API のほうがフレームワーク側でよしなにしてくれるのっぽい？

### Vue アプリケーションの作成

アプリケーションのインスタンス
すべての Vue アプリケーションは createApp 関数で新しい アプリケーションのインスタンス を作成することから始まりまる

```ts
import { createApp } from "vue";

const app = createApp({
  /* ルートコンポーネント オプション */
});
```

ルートコンポーネント
すべてのアプリには、「ルートコンポーネント」という、他のコンポーネントを子要素として保持できるものが必要

SFC を利用している場合は、ルートコンポーネントを他のファイルからインポートする

例

```ts
import { createApp } from "vue";
// ルートコンポーネントを単一ファイルコンポーネントからインポートする
import App from "./App.vue";

const app = createApp(App);
```

Vue のガイドにある多くの例では単一コンポーネントしか必要としないが、
実際ののほとんどのアプリケーションではネスト化された、再利用性のあるコンポーネントツリーで構成
ガイドの後半ではそれも扱うらしい。

TODO アプリケーションのコンポーネントツリーの例

```
App (root component)
├─ TodoList
│ └─ TodoItem
│ ├─ TodoDeleteButton
│ └─ TodoEditButton
└─ TodoFooter
├─ TodoClearButton
└─ TodoStatistics
```

アプリのマウント

アプリケーションのインスタンスは.mount()メソッドが呼ばれるまでは何もレンダリングしない。
インスタンスには「コンテナ」引数という、実際の DOM 要素か、セレクター文字列が必要

`<div id="app"></div>`

`app.mount('#app')`

この .mount() メソッドはすべてのアプリの設定やアセットの登録が完了した後、常に呼ばれる必要がある。また、アセット登録をするメソッドとは異なり、返り値はアプリケーションのインスタンスではなく、ルートコンポーネントインスタンスであるということに注意。

常に.mount()メソッドが呼ばれていないと行けない。

複数アプリケーションのインスタンス
createAPP API は同じページ内に複数の Vue コンポーネントが共存できる
それぞれ独自の設定やグローバルアセットを備えたスコープを持てる

もし Vue をサーバーレンダリングされた HTML を拡張するために使用していたり、大きなページの中で特定の
