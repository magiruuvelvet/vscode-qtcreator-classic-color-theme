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
    "highlight.regexFlags": "g",
    "highlight.regexes": {
        // D: make D class destructor italic, syntax highlighting doesn't match it as destructor
        "(\\~this)\\(\\)": {
            "filterLanguageRegex": "d",
            "decorations": [ { "fontStyle": "italic", "fontWeight": "bold", "color": "#0057ae" } ]
        },
        // D: make [unknown/custom] identifier within version() bold
        "(version)([\\s]?\\()(.*?)(\\))": {
            "filterLanguageRegex": "d",
            "decorations": [ {}, {}, { "fontWeight": "bold" }, {} ]
        },
        // D: version(none|all)
        "(version)([\\s]?\\()(none|all)(\\))": {
            "filterLanguageRegex": "d",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#777777" }, {} ]
        },
        // D: version(endianness|architecture) - architecture list incomplete
        "(version)([\\s]?\\()(LittleEndian|BigEndian|ARM|AArch64|X86|X86\\_64|WebAssembly|WASI)(\\))": {
            "filterLanguageRegex": "d",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#075668" }, {} ]
        },
        // D: version(compiler vendor)
        "(version)([\\s]?\\()(DigitalMars|LDC|GNU|SDC|D\\_NET)(\\))": {
            "filterLanguageRegex": "d",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#ab7826" }, {} ]
        },
        // D: version(C and C++ runtime)
        "(version)([\\s]?\\()(CRuntime\\_Musl|CRuntime\\_Bionic|CRuntime\\_UClibc|CRuntime\\_DigitalMars|CRuntime\\_Microsoft|CRuntime\\_Glibc|CRuntime\\_Newlib|CRuntime\\_WASI|CppRuntime\\_Clang|CppRuntime\\_Gcc|CppRuntime\\_Microsoft|CppRuntime\\_DigitalMars|CppRuntime\\_Sun)(\\))": {
            "filterLanguageRegex": "d",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#739e7b" }, {} ]
        },
        // D: version(platform)
        "(version)([\\s]?\\()(linux|OSX|iOS|TVOS|WatchOS|Darwin|FreeBSD|NetBSD|OpenBSD|DragonFlyBSD|BSD|Solaris|Windows|Win32|Win64|Posix|AIX|Haiku|SkyOS|SysV3|SysV4|Hurd|Android|Emscripten|PlayStation|PlayStation4|Cygwin|MinGW|FreeStanding)(\\))": {
            "filterLanguageRegex": "d",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#c72169" }, {} ]
        },
        // D: make else before version() same color as version()
        "(else)\\s(version)[\\s]?\\(": {
            "filterLanguageRegex": "d",
            "decorations": [ { "color": "#ce3102" }, {} ]
        },
        // D: __gshared global variable across all threads
        "(\\_\\_gshared)": {
            "filterLanguageRegex": "d",
            "decorations": [ { "color": "#808000", "fontWeight": "bold" } ]
        },
        // D: operator overloads
        "(opUnary|opIndexUnary|opCast|opBinary|opBinaryRight|opEquals|opCmp|opCall|opAssign|opIndexAssign|opSlice|opSliceAssign|opOpAssign|opIndexOpAssign|opDollar|opIndex|opDispatch)": {
            "filterLanguageRegex": "d",
            "decorations": [ { "color": "#808000" } ]
        },
        // D: cast() highlighting fix, don't colorize parenthesis
        "(cast)([\\s]?\\()(.*?)(\\))": {
            "filterLanguageRegex": "d",
            "decorations": [ {}, { "color": "#000000" }, {}, { "color": "#000000" } ]
        },
        // D: scope() don't colorize parenthesis, highlight built-in scopes for visibility
        "(scope)([\\s]?\\()(exit|success|failure)(\\))": {
            "filterLanguageRegex": "d",
            "decorations": [ {}, { "color": "#000000" }, { "color": "#000000", "fontWeight": "bold" }, { "color": "#000000" } ]
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
        // Ruby: class constructor
        "(def\\s)(initialize)([\\(]|$)": {
            "filterLanguageRegex": "ruby",
            "regexFlags": "gm",
            "decorations": [ {}, { "color": "#0057ae" }, {} ]
        },
        // HTTP: highlight placeholders, syntax highlighting doesn't implement a token for this
        "(\\{\\{.*?\\}\\})": {
            "filterLanguageRegex": "http",
            "decorations": [ { "color": "#777777" } ]
        },
        // C/C++: __attribute__(())
        "(\\_\\_attribute\\_\\_)[\\s]?\\(\\(": {
            "filterLanguageRegex": "^c$|^cpp$",
            "decorations": [ { "color": "#ce3102" } ]
        },
        // C++: thread_local storage
        "(thread\\_local)": {
            "filterLanguageRegex": "cpp",
            "decorations": [ { "color": "#808000", "fontWeight": "bold" } ]
        },
        // YAML: placeholder in strings
        "(\\%\\{.*?\\})": {
            "filterLanguageRegex": "yaml",
            "decorations": [ { "color": "#777777" } ]
        }
    }
}
```
