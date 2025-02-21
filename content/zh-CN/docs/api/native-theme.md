# nativeTheme

> Read and respond to changes in Chromium's native color theme.

进程：[主进程](../glossary.md#main-process)

## 事件

The `nativeTheme` module emits the following events:

### 事件名: 'updated'

Emitted when something in the underlying NativeTheme has changed. This normally means that either the value of `shouldUseDarkColors`, `shouldUseHighContrastColors` or `shouldUseInvertedColorScheme` has changed. You will have to check them to determine which one has changed.

## 属性

`nativeTheme` 模块具备以下属性：

### `nativeTheme.shouldUseDarkColors` _只读_

此属性的值为一个 `Boolean` 类型的值，代表着当前OS / Chromium是否正处于dark模式。或者应用程序是否正被建议使用dark模式的皮肤  如果你希望修改此属性的值，你应该使用下面的 `themeSource`属性

### `nativeTheme.themeSource`

A `String` property that can be `system`, `light` or `dark`.  It is used to override and supercede the value that Chromium has chosen to use internally.

Setting this property to `system` will remove the override and everything will be reset to the OS default.  By default `themeSource` is `system`.

Settings this property to `dark` will have the following effects:
* `nativeTheme.shouldUseDarkColors` will be `true` when accessed
* Any UI Electron renders on Linux and Windows including context menus, devtools, etc. will use the dark UI.
* Any UI the OS renders on macOS including menus, window frames, etc. will use the dark UI.
* The [`prefers-color-scheme`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme) CSS query will match `dark` mode.
* The `updated` event will be emitted

Settings this property to `light` will have the following effects:
* `nativeTheme.shouldUseDarkColors` will be `false` when accessed
* Any UI Electron renders on Linux and Windows including context menus, devtools, etc. will use the light UI.
* Any UI the OS renders on macOS including menus, window frames, etc. will use the light UI.
* The [`prefers-color-scheme`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme) CSS query will match `light` mode.
* The `updated` event will be emitted

The usage of this property should align with a classic "dark mode" state machine in your application where the user has three options.
* `Follow OS` --> `themeSource = 'system'`
* `Dark Mode` --> `themeSource = 'dark'`
* `Light Mode` --> `themeSource = 'light'`

Your application should then always use `shouldUseDarkColors` to determine what CSS to apply.

### `nativeTheme.shouldUseHighContrastColors` _macOS_ _Windows_ _Readonly_

A `Boolean` for if the OS / Chromium currently has high-contrast mode enabled or is being instructed to show a high-constrast UI.

### `nativeTheme.shouldUseInvertedColorScheme` _macOS_ _Windows_ _Readonly_

A `Boolean` for if the OS / Chromium currently has an inverted color scheme or is being instructed to use an inverted color scheme.
