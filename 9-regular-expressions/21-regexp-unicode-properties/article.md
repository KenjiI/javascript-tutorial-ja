
<<<<<<< HEAD
# Unicode character properies \p

[Unicode](https://en.wikipedia.org/wiki/Unicode), the encoding format used by Javascript strings, has a lot of properties for different characters (or, technically, code points). They describe which "categories" character belongs to, and a variety of technical details.
=======
# Unicode character properties \p

[Unicode](https://en.wikipedia.org/wiki/Unicode), the encoding format used by JavaScript strings, has a lot of properties for different characters (or, technically, code points). They describe which "categories" character belongs to, and a variety of technical details.
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09

In regular expressions these can be set by `\p{…}`. And there must be flag `'u'`.

For instance, `\p{Letter}` denotes a letter in any of language. We can also use `\p{L}`, as `L` is an alias of `Letter`, there are shorter aliases for almost every property.

Here's the main tree of properties:

- Letter `L`:
  - lowercase `Ll`, modifier `Lm`, titlecase `Lt`, uppercase `Lu`, other `Lo`
- Number `N`:
<<<<<<< HEAD
  - decimal digit `Nd`, letter number `Nl`, other `No`:
=======
  - decimal digit `Nd`, letter number `Nl`, other `No`
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09
- Punctuation `P`:
  - connector `Pc`, dash `Pd`, initial quote `Pi`, final quote `Pf`, open `Ps`, close `Pe`, other `Po`
- Mark `M` (accents etc):
  - spacing combining `Mc`, enclosing `Me`, non-spacing `Mn`
- Symbol `S`:
  - currency `Sc`, modifier `Sk`, math `Sm`, other `So`
- Separator `Z`:
  - line `Zl`, paragraph `Zp`, space `Zs`
- Other `C`:
<<<<<<< HEAD
  - control `Cc`, format `Cf`, not assigned `Cn`, private use `Co`, surrogate `Cs`.
=======
  - control `Cc`, format `Cf`, not assigned `Cn`, private use `Co`, surrogate `Cs`
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09

```smart header="More information"
Interested to see which characters belong to a property? There's a tool at <http://cldr.unicode.org/unicode-utilities/list-unicodeset> for that.

You could also explore properties at [Character Property Index](http://unicode.org/cldr/utility/properties.jsp).

For the full Unicode Character Database in text format (along with all properties), see <https://www.unicode.org/Public/UCD/latest/ucd/>.
```

There are also other derived categories, like:
- `Alphabetic` (`Alpha`), includes Letters `L`, plus letter numbers `Nl` (e.g. roman numbers Ⅻ), plus some other symbols `Other_Alphabetic` (`OAltpa`).
- `Hex_Digit` includes hexadimal digits: `0-9`, `a-f`.
- ...Unicode is a big beast, it includes a lot of properties.

For instance, let's look for a 6-digit hex number:

```js run
<<<<<<< HEAD
let reg = /\p{Hex_Digit}{6}/u; // flag 'u' is requireds
=======
let reg = /\p{Hex_Digit}{6}/u; // flag 'u' is required
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09

alert("color: #123ABC".match(reg)); // 123ABC
```

There are also properties with a value. For instance, Unicode "Script" (a writing system) can be Cyrillic, Greek, Arabic, Han (Chinese) etc, the [list is long]("https://en.wikipedia.org/wiki/Script_(Unicode)").

<<<<<<< HEAD
To search for certain scripts, we should supply `Script=<value>`, e.g. to search for cyrillic letters: `\p{sc=Cyrillic}`, for Chinese glyphs: `\p{sc=Han}`, etc:
=======
To search for characters in certain scripts ("alphabets"), we should supply `Script=<value>`, e.g. to search for cyrillic letters: `\p{sc=Cyrillic}`, for Chinese glyphs: `\p{sc=Han}`, etc:
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09

```js run
let regexp = /\p{sc=Han}+/gu; // get chinese words

let str = `Hello Привет 你好 123_456`;

alert( str.match(regexp) ); // 你好
```

## Building multi-language \w

<<<<<<< HEAD
Let's make a "universal" regexp for `pattern:\w`, for any language. That task has a standard solution in many programming languages with unicode-aware regexps, e.g. Perl.
=======
The pattern `pattern:\w` means "wordly characters", but doesn't work for languages that use non-Latin alphabets, such as Cyrillic and others. It's just a shorthand for `[a-zA-Z0-9_]`, so `pattern:\w+` won't find any Chinese words etc.

Let's make a "universal" regexp, that looks for wordly characters in any language. That's easy to do using Unicode properties:
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09

```js
/[\p{Alphabetic}\p{Mark}\p{Decimal_Number}\p{Connector_Punctuation}\p{Join_Control}]/u
```

<<<<<<< HEAD
Let's decipher. Remember, `pattern:\w` is actually the same as `pattern:[a-zA-Z0-9_]`.

So the character set includes:
=======
Let's decipher. Just as `pattern:\w` is the same as `pattern:[a-zA-Z0-9_]`, we're making a set of our own, that includes:
>>>>>>> 9b5c1c95ec8a466150e519b0e94748717c747b09

- `Alphabetic` for letters,
- `Mark` for accents, as in Unicode accents may be represented by separate code points,
- `Decimal_Number` for numbers,
- `Connector_Punctuation` for the `'_'` character and alike,
- `Join_Control` -– two special code points with hex codes `200c` and `200d`, used in ligatures e.g. in arabic.

Or, if we replace long names with aliases (a list of aliases [here](https://www.unicode.org/Public/UCD/latest/ucd/PropertyValueAliases.txt)):

```js run
let regexp = /([\p{Alpha}\p{M}\p{Nd}\p{Pc}\p{Join_C}]+)/gu;

let str = `Hello Привет 你好 123_456`;

alert( str.match(regexp) ); // Hello,Привет,你好,123_456
```