# Visual Studio Code: Qt Creator Default Classic

A theme based on Qt Creator's classic default color theme with KDE default
syntax highlighting colors mixed in.

This theme is mostly based on how code is highlighted in Qt Creator, there
are some minor differences. On top of that I added support for other languages
and also added highlighting from KDE's highlighting rules.

The theme is still incomplete. I will add more scopes as I encounter ugliness
or rainbows in the highlighted code.

**Why?**

Because lesser colors are better. I don't want my code to be a frickin rainbow
where every single word is in another color.
Also I like light themes :)

This fixes some problems I had with the default VS Code light theme (personal preference).

**I found a bug. What should I do?**

First check if textmate scopes are present, some languages have shitty tokenizers.
Installing extensions in VS Code for the language may fix it.

Start the **"Inspect Editor Tokens and Scopes"** tool from the command runner and
click on the word and see if the language parser got it right. Highlighting is only
based on those scopes and nothing else.

## Choice of Colors

 - The default font color is `#000000`.
 - Most keywords, types and language constants and built-in's are `#808000`
   (yellow with good readability on `#ffffff` background).
 - Function names, method names, declarations and calls (and maybe others) are just `#000000` because there
   is no reason to highlight them explicitly and it just transforms the code into a rainbow.
 - Complex data types, classes, modules, aliased types and similar constructs are `#8d268f`.
 - Comments are `#737373`.
 - Code Documentations (Doc Strings) are `#000080`.
 - Operators are just `#000000` again, no reason to highlight those explicitly.
 - Strings are `#008000`.
 - All kinds of punctuation which doesn't serve a special purpose in the language are just `#000000`,
   no reason to highlight those, this includes things like pointers in C++, no extra highlighting.
 - Numbers are `#000080`.
 - Number prefixes and suffixes like `0x` (hex) and others when the language supports it are `#000080` too.
 - Escape sequences are `#aaaaaada`.
 - Placeholders in format strings (like `%s` or `{}`) are `#000080`. If the tokenizer gets those right of course.
 - Debugging specific keywords or functions like `debug` in D or `console` in JavaScript are in `#ce3102`
   to make them recognizable in the otherwise barely colored code.
 - HEREDOC start/end markers if the language supports them (like Perl or Ruby) are in `#c82479`.

 - There are use of some other colors too, but they are language specific, without transforming the code
   into a rainbow. All chosen colors are readable on white background in a well-lit room.

Total amount of highlighting colors (without the language-specific additions): **7**

## Extra Highlighting Rules

This is optional. Adds some extra highlighting rules.

Install `fabiospampinato.vscode-highlight` and add this to your `settings.json`:

```jsonc
{
    "highlight.regexes": {
        // D: make D class destructor italic, syntax highlighting doesn't match it as destructor
        "(\\~this)\\(\\)": {
            "filterLanguageRegex": "d",
            "decorations": [ { "fontStyle": "italic", "fontWeight": "bold", "color": "#0057ae" } ]
        },
        // Ruby: highlight "self.", syntax highlighting doesn't match it as separate token
        "(self)\\.": {
            "filterLanguageRegex": "ruby",
            "decorations": [ { "color": "#808000" } ]
        },
        // Ruby: rubocop special comments
        "(\\#\\s)(rubocop\\:enable)(.*)": {
            "filterLanguageRegex": "ruby",
            "decorations": [ {}, { "fontWeight": "bold", "color": "#ce3102" }, { "color": "#8d268f" } ]
        },
        "(\\#\\s)(rubocop\\:disable)(.*)": {
            "filterLanguageRegex": "ruby",
            "decorations": [ {}, { "fontWeight": "bold", "color": "#ce3102" }, { "color": "#8d268f" } ]
        },
        // HTTP: highlight placeholders, syntax highlighting doesn't implement a token for this
        "(\\{\\{.*?\\}\\})": {
            "filterLanguageRegex": "http",
            "decorations": [ { "color": "#777777" } ]
        }
    }
}
```
