# CONFIG

#meta

```space-lua
config.set {
  explorer = {
    negativeFilter = {
      "Library",
      "Repositories",
      "CONFIG",
      "graphview.plug.js",
    },
  },

  graphview = {
    ignoredPrefixes = {
      "Library",
      "Repositories",
      "CONFIG",
      "graphview.plug.js",
    },
    enableDecorations = true,
    position = "rhs",
  },
}
```

```space-style
html {
  --editor-font: "JetBrains Mono", "Fira Code", "SFMono-Regular", Consolas, monospace !important;
}

#sb-main .cm-editor {
  font-size: 14px;
}
```
