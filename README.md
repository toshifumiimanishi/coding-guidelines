# Coding Guidelines
ガイドラインは開発者が共通のルールのもと、設計や運用をおこなうための指標であり、以下のメリットがある。

- **開発時間の短縮**：運用すべきスタイルが明確になる。
- **効率的な構文エラーのチェック**：厳格なフォーマットを採用すれば、バリデータがエラーを見分けやすくなる。
- **可読性の向上**：人の目は秩序を好むため、一貫性のあるマークアップは開発者のストレスを軽減する。

## HTML
HTML 構文は、WHATWG の [HTML Living Standard](https://html.spec.whatwg.org/multipage/) に準拠する。W3C と WHATWG の[合意](https://www.w3.org/blog/news/archives/7753)により、WHATWG が策定する HTML Living Standard が唯一の標準となる。


### 属性の記述順
属性値は class、id、data-*、その他の属性の順序で記述する。

### target="_blank" で開くリンクには rel="noopener" をつける
`target="_blank"` で開いたページは、`window.opener` を使って親のページを操作できるため **フィッシング詐欺攻撃の脆弱性** がある。`rel="noopener"` を指定すれば、リンクをクリックして開いたページから `window.opener` で親のページを参照できなくなる。

### img 要素には width, heihgt を明示しない
レスポンシブデザインやリキッドデザインが主流の昨今において、CSS に `width`, `height` を指定することが多く、HTML の属性値に明示しない。画像サイズを明示することでレンダリングコストを抑制するメリットは、CSS で指定してもかまわない。

### パスはルート相対パスで指定する
外部ファイルのインクルードに最適であり、パスの一括置換が可能であるためディレクトリ構造の変更に対応しやすい。

### Boolean 属性値は省略する
Boolean 属性値は一貫して省略する。Boolean 属性は、属性が存在すれば true（真）、存在しなければ false（偽）になります。つまり、属性値はあってもなくても true（真）になります。値が空（e.g. `required=""`）でも true（真）になります。Boolean 属性値の一貫した省略は、その属性が Boolean 属性であることが明確になり、可読性が向上します。

下記の記述はいずれも true（真）になります。

```html
<input required>
<input required="">
<input required="required">
```

### WAI-ARIA より HTML5 ベースのセマンティクスが望ましい
WAI-ARIA は、ブラウザや支援技術が認識できる意味論を追加する、ユーザーの理解を補助する技術である。真に意味論の提供は、HTML5 ベースのセマンティクスを使用すべき。ベンダーのサポート状況を鑑みて選定する。

### 要処置事項は TODO 接頭辞でコメントする
あとで手をつけることをソースコードにコメントする場合、`TODO: action item` の形式で記述する。
```html
<!-- TODO: remove optional tags -->
<ul>
  <li>Apples</li>
  <li>Oranges</li>
</ul>
```

## CSS

### CSS 設計
[FLOCSS](https://github.com/hiloki/flocss) をベースに各プロジェクトに合った設計を考える。詳しくは[堅牢で保守的な最低限度の CSS 設計](https://qiita.com/toshifumiimanishi/items/289ff7a44054bcc39b09)を参照ください。

### クラスの命名規則
基本的なクラスの命名には [BEM](https://en.bem.info/methodology/) を採用する。ただし、Element はアンダースコアひとつ、Modifier はハイフンひとつを接頭辞として運用する。また、BEM の命名規則の混在を防ぐため、単語間の区切りにダッシュやアンダースコアを使わない。

```css
.Block_Element { ... }

.Block_Element.-Modifier { ... }
```

### 擬似要素の構文は CSS3 に準拠する
擬似要素の構文はコロンふたつを推奨する。これは CSS Level 3 から導入された記法で擬似クラスと区別するためである。CSS Level 1 や CSS Level 2 では、擬似要素と擬似クラスの構文はコロンがひとつでした。

### 同じ宣言ブロック内に異なるセレクタの宣言を記述しない
Sass の機能でセレクタを入れ子構造にする場合、ひとつの宣言ブロックに異なるセレクタのスタイルを宣言しない。これはセレクタの煩雑化を防ぎ、一元管理に寄与する。

```scss
// Bad pattern
.parent {
  background-color: #000;
  
  .child {
    font-weight: bold;
  }
}

.child {
  color: #fff;
}
```
```scss
// Good pattern
.parent {
  background-color: #000;
}

.child {
  color: #fff;
  
  .parent & {
    font-weight: bold;
  }
}
```

### CSS ハックは原則禁止
CSS ハックは最終手段として、**最大限に別のアプローチを模索すべき**である。そもそも CSSハックとは、ブラウザ間で異なる CSS の実装状況の違いやバグなどを吸収し、各ブラウザの表示を同一にするためのテクニックである。しかし、CSS ハックはブラウザのバグを利用するため、将来的にバグが修正されればプロジェクトに不具合が生じるかもしれない。


## JavaScript

### 引用符はシングルクォーテーション
原則、引用符は「'（シングルクォーテーション）」を使用しましょう。DOM の取得にセレクタをエスケープする手間が省けます。ただし、テンプレート文字列にプレースホルダーを含む場合は除きます。

```js
console.log('hello world')    // ✓ ok
console.log("hello world")    // ✗ avoid
console.log(`hello world`)    // ✗ avoid
 
console.log(`hello ${name}`)  // ✓ ok
```

## Accessibility

### 可視ラベルのない要素に title 属性をつける（レベル A）
フォーム・コントロールの要素（e.g. `<input>`, `<select>`）にラベルとなるテキストが見た目に存在しない場合、title 属性を用いてラベルを提供する。詳しくは [WCAG 2.1 の達成基準 1.3.1 情報及び関係性](https://www.w3.org/WAI/WCAG21/Understanding/info-and-relationships.html)を参照ください。

### インタラクティブな要素にラベルづけ（レベル A）
インタラクティブな要素にはアクセシブルな名前をつけて、支援技術で伝えれるようにすべきです。これは [WCAG 2.1 の達成基準 4.1.2「名前（name）・役割（role）及び値（value）」](https://www.w3.org/WAI/WCAG21/Understanding/name-role-value.html)に関連します。
