# Karabiner-Elements 的 Windows 風格輸入法切換規則

[English](README.md) | 中文 | [Português](README_pt.md) | [日本語](README_ja.md)

這個倉庫提供 `windows_style_IME_switcher`，這是一份 Karabiner-Elements complex modification 規則，適合從 Windows 轉到 macOS、但仍希望保留熟悉輸入法切換習慣的使用者。

它適用於同時使用多個 macOS 輸入來源的情境，例如：

- 英文
- 第三方中文輸入法，例如 Squirrel
- 葡萄牙文或其他額外輸入法

## 功能

主要規則檔案是：

```text
windows_style_IME_switcher.json
```

它提供兩種行為：

1. 按住 `Control` 並點按 `Shift`，切換到下一個輸入來源。
2. 按下 `Control + Space` 在英文與 macOS 輸入來源排序中的第 2 個輸入法之間切換。

更具體地說：

- 當目前輸入來源是英文時，`Control + Space` 會切換到下一個輸入來源。
- 當目前輸入來源不是英文時，`Control + Space` 會切回英文。
- 第 2 個輸入來源不是寫死的，它會跟隨 macOS 裡的輸入來源排序。

例如，如果你的輸入來源順序是：

```text
English -> Squirrel -> Portuguese
```

那麼：

- 按住 `Control` 並點按 `Shift` 會依序循環 `English -> Squirrel -> Portuguese -> English`。
- `Control + Space` 會從英文切到 Squirrel。
- `Control + Space` 會從 Squirrel 或 Portuguese 切回英文。

如果你之後把 Portuguese、日本語或其他輸入來源移到第 2 位，從英文按下 `Control + Space` 時，就會切換到新的第 2 個輸入來源。

## 系統需求

- macOS

- [Karabiner-Elements](https://karabiner-elements.pqrs.org/)

- macOS 的「選擇上一個輸入方式」快捷鍵設定為 `Control + 空格鍵`

- macOS 的「選擇輸入選單中的下一個輸入方式」快捷鍵設定為 `Control + Shift + /`

你可以在這裡檢查或修改這些快捷鍵：

```text
系統設定 -> 鍵盤 -> 鍵盤快速鍵 -> 輸入方式
```

## 安裝方式

1. 打開 Karabiner-Elements 的 complex modifications 目錄：

```text
~/.config/karabiner/assets/complex_modifications/
```

2. 將這個檔案放入該目錄：

```text
windows_style_IME_switcher.json
```

3. 打開 Karabiner-Elements。
4. 前往：

```text
Complex Modifications -> Add your own rule
```

5. 啟用：

```text
windows_style_IME_switcher
```

## 工作原理

這條規則會把按住 `Control` 並點按 `Shift` 對應到 macOS 既有的快捷鍵：

```text
Control + Shift + /
```

因此輸入來源的循環順序仍由 macOS 控制，而不是由 Karabiner-Elements 寫死。

`Control + Space` 的行為使用 Karabiner-Elements 的輸入來源條件：

- `input_source_if` 用來判斷目前是否為英文。
- `input_source_unless` 用來判斷目前是否不是英文。
- `select_input_source` 用來切回英文。

英文輸入來源目前使用以下條件匹配：

```json
{
  "language": "^en$"
}
```

## 注意事項

這條規則假設你的英文輸入來源在 Karabiner-Elements 中會被識別為 `language = en`。

如果 `Control + Space` 不能正確切回英文，可以使用 Karabiner-EventViewer 查看英文輸入來源的實際識別值，然後把 `language` 條件改成更精準的 `input_source_id`。

## 測試

啟用規則後，建議測試以下情境：

1. 從英文開始，按住 `Control` 並連續點按 `Shift`，確認它會依序循環所有輸入來源。
2. 從英文開始，按 `Control + Space`，確認它會切換到第 2 個輸入來源。
3. 從任何非英文輸入來源開始，按 `Control + Space`，確認它會切回英文。
4. 在 macOS 設定中更改第 2 個輸入來源，再從英文按 `Control + Space`，確認它會切到新的第 2 個輸入來源。

## 維護者

[Elias-Lee-SC](https://github.com/Elias-Lee-SC/)
