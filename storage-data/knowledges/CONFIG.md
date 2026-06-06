# CONFIG

#meta

```space-lua
config.set("graphview", {
  ignoredPrefixes = {
    "Library",
    "Repositories",
    "CONFIG",
  },
  position = "rhs",
  enableDecorations = true,
})

config.set("treeview", {
  exclusions = {
    {
      type = "regex",
      rule = "^(?:Library|Repositories|CONFIG).*$",
      negate = false,
    },
  },
})

config.set("std.widgets.linkedMentions.enabled", false)
```

```space-style
html {
  --editor-font: "JetBrains Mono", "Fira Code", "SFMono-Regular", Consolas, monospace !important;
}

#sb-main .cm-editor {
  font-size: 14px;
}
```
