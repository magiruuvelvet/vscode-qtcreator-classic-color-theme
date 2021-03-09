# Semantic highlighting

Semantic highlighting is disabled in the theme, because semantic highlighting still
overwrites textmate scopes and there is no way to configure it, which breaks the
themes look and feel.

You can enable semantic highlighting on a per-language basis in your `settings.json`:

```json
{
    "[languageId]": {
        "editor.semanticHighlighting.enabled": true
    }
}
```

This will overwrite the semantic highlighting setting in the theme and enable
it for only the given language. Multiple language ids can be added.

Once Microsoft decided to fix semantic highlighting overwriting textmate scopes
I will enable it in the theme by default.
