
<<<<<<< HEAD
# URL オブジェクト

組み込みの [URL](https://url.spec.whatwg.org/#api) クラスは URL の作成や解析に関する便利なインタフェースを提供します。

なお、必ずしもこれを使う必要はありません。厳密に `URL` オブジェクトを必要とするネットワークメソッドはなく、文字列でも十分です。しかし非常に役立つときもあります。

## URL を作成する

新しい URL オブジェクトを作成する構文です:
=======
# URL objects

The built-in [URL](https://url.spec.whatwg.org/#api) class provides a convenient interface for creating and parsing URLs.

There are no networking methods that require exactly an `URL` object, strings are good enough. So technically we don't have to use `URL`. But sometimes it can be really helpful.

## Creating an URL

The syntax to create a new URL object:
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09

```js
new URL(url, [base])
```

<<<<<<< HEAD
- **`url`** -- テキスト url です
- **`base`** -- `url` のオプションのベースです

`URL` オブジェクトを使うと、すぐにその構成要素にアクセスできます。そのため、これは url を解析する良い方法です。e.g:

```js run
let url = new URL('https://javascript.info/url');

alert(url.protocol); // https:
alert(url.host);     // javascript.info
alert(url.pathname); // /url
```

チートシートです:

![](url-object.png)

- `href` は完全な url, `url.toString()` と同じです。
- `protocol` はコロン `:` で終わります。
- `search` ははてな `?` から始まります。
- `hash` はハッシュ `#` から始まります。
- HTTP 認証がある場合、`user` と `password` プロパティもあります。

また、相対 url を作成するときにも利用できます。この場合は2つ目の引数を使います:

```js run
let url = new URL('profile/admin', 'https://javascript.info');

alert(url); // https://javascript.info/profile/admin

url = new URL('tester', url); // 現在の url パスに基準にして `tester` に移動します 

alert(url); // https://javascript.info/profile/tester
```

```smart header="文字列の代わりにどこでも `URL` を使うことができます"
`URL` オブジェクト は `fetch` や `XMLHttpRequest`, 文字列の url が期待されているところならどこでも使うことができます。

ほぼ大多数のメソッドは、自動的に文字列に変換します。
```

## SearchParams

与えられた検索パラメータで url を作成したいとしましょう、例えば `https://google.com/search?query=value` です。

これらは正しくエンコードされている必要があります。

非常に古いブラウザでは、`URL` が登場する以前、組み込み関数 `encodeURIComponent/decodeURIComponent` を使っていました。

いま、それらは必要ありません: `url.searchParams` は[URLSearchParams](https://url.spec.whatwg.org/#urlsearchparams)型のオブジェクトです。

これは検索パラメータに対して便利なメソッドを提供しています。:

- **`append(name, value)`** -- パラメータを追加します,
- **`delete(name)`** -- パラメータを削除します,
- **`get(name)`** -- パラメータを取得します,
- **`getAll(name)`** -- 指定した name のすべてのパラメータを取得します(e.g. `?user=John&user=Pete` など複数ある場合),
- **`has(name)`** -- パラメータが存在するかをチェックします,
- **`set(name, value)`** -- パラメータを設定/置き換えます,
- **`sort()`** -- name でパラメータをソートします。めったに必要とされません,
- ...また、`Map` のように反復可能です.

そのため、`URL` オブジェクトは url パラメータを操作するための簡単な方法も提供します。

例:

```js run
let url = new URL('https://google.com/search');
url.searchParams.set('query', 'test me!');
=======
- **`url`** -- the URL string or path (if base is set, see below).
- **`base`** -- an optional base, if set and `url` has only path, then the URL is generated relative to `base`.

For example, these two URLs are same:

```js run
let url1 = new URL('https://javascript.info/profile/admin');
let url2 = new URL('/profile/admin', 'https://javascript.info');

alert(url1); // https://javascript.info/profile/admin
alert(url2); // https://javascript.info/profile/admin
```

Переход к пути относительно текущего URL:

```js run
let url = new URL('https://javascript.info/profile/admin');
let testerUrl = new URL('tester', url);

alert(testerUrl); // https://javascript.info/profile/tester
```


The `URL` object immediately allows us to access its components, so it's a nice way to parse the url, e.g.:

```js run
let url = new URL('https://javascript.info/url');

alert(url.protocol); // https:
alert(url.host);     // javascript.info
alert(url.pathname); // /url
```

Here's the cheatsheet:

![](url-object.png)

- `href` is the full url, same as `url.toString()`
- `protocol` ends with the colon character `:`
- `search` - a string of parameters, starts with the question mark `?`
- `hash` starts with the hash character `#`
- there are also `user` and `password` properties if HTTP authentication is present.


```smart header="We can use `URL` everywhere instead of a string"
We can use an `URL` object in `fetch` or `XMLHttpRequest`, almost everywhere where a string url is expected.

In the vast majority of methods it's automatically converted to a string.
```

## SearchParams "?..."

Let's say we want to create an url with given search params, for instance, `https://google.com/search?query=JavaScript`.

They must be correctly encoded to include non-latin charcters, spaces etc.

Some time ago, before `URL` objects appeared, we'd use built-in functions `encodeURIComponent/decodeURIComponent`. They have some problems, but now that doesn't matter.

There's URL property for that: `url.searchParams` is an object of type [URLSearchParams](https://url.spec.whatwg.org/#urlsearchparams).

It provides convenient methods for search parameters:

- **`append(name, value)`** -- add the parameter,
- **`delete(name)`** -- remove the parameter,
- **`get(name)`** -- get the parameter,
- **`getAll(name)`** -- get all parameters with that name (if many, e.g. `?user=John&user=Pete`),
- **`has(name)`** -- check for the existance of the parameter,
- **`set(name, value)`** -- set/replace the parameter,
- **`sort()`** -- sort parameters by name, rarely needed,
- ...and also iterable, similar to `Map`.

So, `URL` object also provides an easy way to operate on url parameters.

For example:

```js run
let url = new URL('https://google.com/search');
url.searchParams.set('query', 'test me!'); // added parameter with a space and !
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09

alert(url); // https://google.com/search?query=test+me%21

url.searchParams.set('tbs', 'qdr:y'); // add param for date range: past year

alert(url); // https://google.com/search?query=test+me%21&tbs=qdr%3Ay

// iterate over search parameters (decoded)
for(let [name, value] of url.searchParams) {
  alert(`${name}=${value}`); // query=test me!, then tbs=qdr:y
}
```