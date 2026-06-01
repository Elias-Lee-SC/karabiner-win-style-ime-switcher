# Karabiner-Elements 用 Windows 風入力ソース切り替えルール

[English](README.md) | [中文](README_zh.md) | [Português](README_pt.md) | 日本語

このリポジトリには、Windows 風の入力メソッド切り替え操作に慣れている macOS ユーザー向けの Karabiner-Elements complex modification ルール `windows_style_IME_switcher` が含まれています。

複数の macOS 入力ソースを使う環境を想定しています。たとえば：

- 英語
- Squirrel などのサードパーティ製中国語入力メソッド
- ポルトガル語、またはその他の追加入力メソッド

## 機能

メインのルールファイルは次のファイルです：

```text
windows_style_IME_switcher.json
```

このルールは 2 つの動作を提供します：

1. `Control` を押したまま `Shift` をタップすると、次の入力ソースへ切り替えます。
2. `Control + Space` を押すと、英語と macOS の入力ソース順で 2 番目の入力ソースを切り替えます。

具体的には：

- 現在の入力ソースが英語の場合、`Control + Space` は次の入力ソースへ切り替えます。
- 現在の入力ソースが英語ではない場合、`Control + Space` は英語へ戻します。
- 2 番目の入力ソースは固定されていません。macOS の入力ソース順に従います。

たとえば、入力ソースの順番が次のようになっている場合：

```text
English -> Squirrel -> Portuguese
```

このように動作します：

- `Control` を押したまま `Shift` をタップすると、`English -> Squirrel -> Portuguese -> English` の順に循環します。
- `Control + Space` は English から Squirrel へ切り替えます。
- `Control + Space` は Squirrel または Portuguese から English へ戻します。

あとで Portuguese、日本語、または別の入力ソースを 2 番目に移動した場合、English から `Control + Space` を押すと、その新しい 2 番目の入力ソースへ切り替わります。

## 必要条件

- macOS

- [Karabiner-Elements](https://karabiner-elements.pqrs.org/)

- macOS の「前の入力ソースを選択」ショートカットが `Control + スペース` に設定されていること

- macOS の「入力メニューの次のソースを選択」ショートカットが `Control + Shift + /` に設定されていること

これらのショートカットは次の場所で確認または変更できます：

```text
システム設定 -> キーボード -> キーボードショートカット -> 入力ソース
```

## インストール

1. Karabiner-Elements の complex modifications ディレクトリを開きます：

```text
~/.config/karabiner/assets/complex_modifications/
```

2. 次のファイルをそのディレクトリに置きます：

```text
windows_style_IME_switcher.json
```

3. Karabiner-Elements を開きます。
4. 次の画面へ移動します：

```text
Complex Modifications -> Add your own rule
```

5. 次のルールを有効にします：

```text
windows_style_IME_switcher
```

## 仕組み

このルールは `Control` を押したまま `Shift` をタップする操作を macOS 既存のショートカットに割り当てます：

```text
Control + Shift + /
```

そのため、入力ソースの循環順は Karabiner-Elements ではなく macOS 側で管理されます。

`Control + Space` の動作には Karabiner-Elements の入力ソース条件を使っています：

- `input_source_if` は英語を検出します。
- `input_source_unless` は英語以外の入力ソースを検出します。
- `select_input_source` は英語へ戻します。

英語入力ソースは次の条件で判定しています：

```json
{
  "language": "^en$"
}
```

## 注意

このルールは、英語入力ソースが Karabiner-Elements で `language = en` として認識されることを前提にしています。

`Control + Space` で正しく英語へ戻らない場合は、Karabiner-EventViewer で英語入力ソースの実際の識別子を確認し、`language` 条件をより正確な `input_source_id` に置き換えてください。

## テスト

ルールを有効にしたあと、次の動作を確認してください：

1. English から開始し、`Control` を押したまま `Shift` を繰り返しタップします。すべての入力ソースを順に循環するはずです。
2. English から開始し、`Control + Space` を押します。2 番目の入力ソースへ切り替わるはずです。
3. 英語以外の入力ソースから開始し、`Control + Space` を押します。English へ戻るはずです。
4. macOS 設定で 2 番目の入力ソースを変更し、English からもう一度 `Control + Space` を押します。新しい 2 番目の入力ソースへ切り替わるはずです。

## メンテナー

[Elias-Lee-SC](https://github.com/Elias-Lee-SC/)
