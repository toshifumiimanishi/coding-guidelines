# Coding Guidelines
ガイドラインは開発者が共通のルールのもと、設計や運用をおこなうための指標であり、以下のメリットがある。

- **開発時間の短縮**：運用すべきスタイルが明確になる。
- **効率的な構文エラーのチェック**：厳格なフォーマットを採用すれば、バリデータがエラーを見分けやすくなる。
- **可読性の向上**：人の目は秩序を好むため、一貫性のあるマークアップは開発者のストレスを軽減する。

## HTML
HTML 構文は、WHATWG の [HTML Living Standard](https://html.spec.whatwg.org/multipage/) に準拠する。

---
**【column】 W3G vs WHATWG の終焉**  
W3C と WHATWG の[合意](https://www.w3.org/blog/news/archives/7753)により、W3C は HTML と DOM に関する標準策定をやめ、今後は WHATWG が策定する HTML Living Standard が唯一の標準となる。

---

### 属性の記述順
属性値は class、id、data-*、その他の属性の順序で記述する。

### target="_blank" で開くリンクには rel="noopener" をつける
`target="_blank"` で開いたページは、`window.opener` を使って親のページを操作できるため **フィッシング詐欺攻撃の脆弱性** がある。`rel="noopener"` を指定すれば、リンクをクリックして開いたページから `window.opener` で親のページを参照できなくなる。

### パスはルート相対パスで指定する
外部ファイルのインクルードに最適であり、パスの一括置換が可能であるためディレクトリ構造の変更に対応しやすい。

### 可視ラベルのない要素に title 属性をつける
フォーム・コントロールの要素（e.g. `<input>`, `<select>`）にラベルとなるテキストが見た目に存在しない場合、title 属性を用いてラベルを提供する。詳しくは [WCAG 2.1 の達成基準 1.3.1 情報及び関係性（レベル A）](https://www.w3.org/WAI/WCAG21/Understanding/info-and-relationships.html)を参照ください。

### WAI-ARIA は必要な場合のみ使用する
WAI-ARIAは、ブラウザや支援技術が認識できる意味論を追加する、ユーザーの理解を補助する技術である。真に意味論の提供は、HTML5 ベースのセマンティクスを使用すべき。ベンダーのサポート状況を鑑みて選定する。

## CSS

### CSS 設計

### クラスの命名規則

### 同じ宣言ブロック内に異なるセレクタの宣言を記述しない
Sass の機能でセレクタを入れ子構造にする場合、ひとつの宣言ブロックに異なるセレクタのスタイルを宣言しない。これはセレクタを一元管理できるメリットがある。

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

## JavaScript
