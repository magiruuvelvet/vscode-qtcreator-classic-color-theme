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
    "highlight.decorations": {
        // prevent follow up text from being highlighted incorrectly on typing, when the regex doesn't match anymore
        // bug: https://github.com/fabiospampinato/vscode-highlight/issues/31
        // workaround may cause performance issues, if VSCode becomes unresponsive, switch back to the default "3"
        "rangeBehavior": 1
    },
    "highlight.regexes": {
        // highlight trailing whitespaces, alternative to possan.nbsp-vscode which is not configurable at all
        // (don't use '\s' here, because it causes highlighting bugs on enter key press)
        "([ 　\\t  ]+$)": { // 0x20, U+3000, tab, U+00A0, U+202F
            "regexFlags": "gm",
            "decorations": [ {  "backgroundColor": "#b4000070" } ]
        },
        // highlight special whitespaces and unicode whitespaces
        "( | )": { // U+00A0, U+202F
            "decorations": [ { "backgroundColor": "#eeeeee20", "border": "1px solid #44444470" } ]
        },

        // D: make D class destructor italic, syntax highlighting doesn't match it as destructor
        "(?<!static )(\\~this)\\(\\)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "fontStyle": "italic", "fontWeight": "bold", "color": "#0057ae" } ]
        },
        // D: make [unknown/custom] identifier within version() bold
        "(\\bversion\\b)([\\s]?\\()(.*?)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, {}, { "fontWeight": "bold" }, {} ]
        },
        // D: version(none|all)
        "(\\bversion\\b)([\\s]?\\()(none|all)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#777777" }, {} ]
        },
        // D: version(endianness|architecture) - architecture list incomplete
        "(\\bversion\\b)([\\s]?\\()(LittleEndian|BigEndian|ARM|AArch64|X86|X86\\_64|WebAssembly|WASI)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#075668" }, {} ]
        },
        // D: version(compiler vendor)
        "(\\bversion\\b)([\\s]?\\()(DigitalMars|LDC|GNU|SDC|D\\_NET)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#ab7826" }, {} ]
        },
        // D: version(C and C++ runtime)
        "(\\bversion\\b)([\\s]?\\()(CRuntime\\_Musl|CRuntime\\_Bionic|CRuntime\\_UClibc|CRuntime\\_DigitalMars|CRuntime\\_Microsoft|CRuntime\\_Glibc|CRuntime\\_Newlib|CRuntime\\_WASI|CppRuntime\\_Clang|CppRuntime\\_Gcc|CppRuntime\\_Microsoft|CppRuntime\\_DigitalMars|CppRuntime\\_Sun)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#739e7b" }, {} ]
        },
        // D: version(platform)
        "(\\bversion\\b)([\\s]?\\()(linux|OSX|iOS|TVOS|tvOS|WatchOS|watchOS|Darwin|FreeBSD|NetBSD|OpenBSD|DragonFlyBSD|BSD|Solaris|Windows|Win32|Win64|Posix|AIX|Haiku|SkyOS|SysV3|SysV4|Hurd|Android|Emscripten|PlayStation|PlayStation4|Cygwin|MinGW|FreeStanding)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, {}, { "fontWeight": "bold", "color": "#c72169" }, {} ]
        },
        // D: make else before version() same color as version()
        "(else)\\s(\\bversion\\b)[\\s]?\\(": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102" }, {} ]
        },
        // D: pragmas
        "(\\bpragma\\b)(\\()(.*?)([\\,\\)])": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, {}, { "color": "#000000" }, {} ]
        },
        // D: known pragmas
        "(\\bpragma\\b)(\\()(inline|mangle|lib|linkerDirective|crt\\_constructor|crt\\_destructor|msg|printf|scanf|startaddress)([\\,\\)])": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, {}, { "fontWeight": "bold" }, {} ]
        },
        // D: __gshared global variable across all threads
        "(\\b\\_\\_gshared\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#808000", "fontWeight": "bold" } ]
        },
        // D: operator overloads, visualize some of them with C++ style operator overloading for better recognization
        "(\\bopUnary\\b|\\bopIndexUnary\\b|\\bopCast\\b|\\bopBinaryRight\\b|\\bopBinary\\b|\\bopEquals\\b|\\bopCmp\\b|\\bopCall\\b|\\bopAssign\\b|\\bopIndexAssign\\b|\\bopSlice\\b|\\bopSliceAssign\\b|\\bopOpAssign\\b|\\bopIndexOpAssign\\b|\\bopDollar\\b|\\bopIndex\\b|\\bopDispatch\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102" } ]
        },
        "(\\bopEquals\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator== ", "color": "#bbbbbb" } } ]
        },
        "(\\bopCall\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator() ", "color": "#bbbbbb" } } ]
        },
        "(\\bopDispatch\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator.function() ", "color": "#bbbbbb" } } ]
        },
        "(\\bopCmp\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator<> ", "color": "#bbbbbb" } } ]
        },
        "(\\bopAssign\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator= ", "color": "#bbbbbb" } } ]
        },
        "(\\bopIndexAssign\\b|\\bopIndex\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator[] ", "color": "#bbbbbb" } } ]
        },
        "(\\bopCast\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator T ", "color": "#bbbbbb" } } ]
        },
        "(\\bopSlice\\b|\\bopSliceAssign\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator[i..j] ", "color": "#bbbbbb" } } ]
        },
        "(\\bopDollar\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "before": { "contentText": "operator[i..$] ", "color": "#bbbbbb" } } ]
        },
        // D: cast() highlighting fix, don't colorize parenthesis
        "(\\bcast)([\\s]?\\()(.*?)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, { "color": "#000000" }, {}, { "color": "#000000" } ]
        },
        // D: scope() don't colorize parenthesis, highlight built-in scopes for visibility
        "(\\bscope)([\\s]?\\()(exit|success|failure)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, { "color": "#000000" }, { "color": "#000000", "fontWeight": "bold" }, { "color": "#000000" } ]
        },
        // D: scoped!
        "(\\bscoped\\!)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#777777" } ]
        },
        // D: missing keywords in highlighter
        "(\\bforeach\\_reverse\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#808000" } ]
        },
        // D: special object functions available almost everywhere
        "(\\.)(idup|dup|length|ptr|funcptr|stringof|alignof|mangleof|init|sizeof|tupleof|classinfo)([\\s\\(\\)\\[\\]\\{\\},;\\.\\+\\-\\|\\&]|$)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, { "color": "#0057ae" }, {} ]
        },
        // D: module constructor/destructor
        "(static\\s[\\s]?this\\(\\)|static\\s[\\s]?\\~this\\(\\)|shared\\s[\\s]?static\\s[\\s]?this\\(\\)|shared\\s[\\s]?static\\s[\\s]?\\~this\\(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#ce3102", "fontWeight": "normal" } ]
        },
        // D: catch-all exceptions block, C++ has `catch(...)` for this, but D needs to know the base exception type
        "(\\bcatch[\\s]?\\()(Exception)(\\))": {
            "filterLanguageRegex": "^d$",
            "decorations": [ {}, { "color": "#aaaaaa" }, {} ]
        },
        // Ruby: highlight "self.", syntax highlighting doesn't match it as separate token
        "(\\bself)\\.": {
            "filterLanguageRegex": "^ruby$",
            "decorations": [ { "color": "#808000" } ]
        },
        // Ruby: rubocop special comments
        "(#[ ]*?)(rubocop\\:enable)(\\s)(.*)": {
            "filterLanguageRegex": "^ruby$",
            "decorations": [ {}, { "fontWeight": "bold", "color": "#ce3102" }, {}, { "color": "#8d268f" } ]
        },
        "(#[ ]*?)(rubocop\\:disable)(\\s)(.*)": {
            "filterLanguageRegex": "^ruby$",
            "decorations": [ {}, { "fontWeight": "bold", "color": "#ce3102" }, {}, { "color": "#8d268f" } ]
        },
        // Ruby: Solargraph special comments
        "(#[ ]*?)(\\@\\!override\\b|\\@yieldself\\b|\\@type\\b)(.*)": {
            "filterLanguageRegex": "^ruby$",
            "decorations": [ {}, { "color": "#ce3102" }, {} ]
        },
        "(#[ ]*?)(\\@type\\b)(.*?)(\\[)(.*?)(\\])": {
            "filterLanguageRegex": "^ruby$",
            "decorations": [ {}, { "color": "#ce3102" }, {}, { "color": "#000000" }, { "color": "#808000" }, { "color": "#000000" } ]
        },
        // Ruby: frozen string literal comment
        "(#[ ]*?)(\\bfrozen\\_string\\_literal\\b)(\\:)([ ]*?)(\\btrue\\b|\\bfalse\\b)$": {
            "filterLanguageRegex": "^ruby$",
            "regexFlags": "gm",
            "decorations": [ {}, { "color": "#0057ae", "fontWeight": "bold" }, {}, {}, { "color": "#808000" } ]
        },
        // Ruby: class constructor
        "(\\bdef\\s)(initialize)([\\s\\(]|$)": {
            "filterLanguageRegex": "^ruby$",
            "regexFlags": "gm",
            "decorations": [ {}, { "color": "#0057ae" }, {} ]
        },
        // Ruby: special keywords
        "(\\bprivate\\_class\\_method\\b)": {
            "filterLanguageRegex": "^ruby$",
            "decorations": [ { "color": "#0057ae", "fontWeight": "bold" } ]
        },
        // Ruby: special object functions available almost everywhere
        "(\\.)(dup|nil\\?|empty\\?|negative\\?|positive\\?|freeze|frozen\\?|to\\_i|to\\_f|to\\_c|to\\_s|to\\_r|to\\_a|to\\_h|to\\_sym|to\\_json|as\\_json|is\\_a\\?)([\\s\\(\\)\\[\\]\\{\\},;\\.\\+\\-\\|\\&]|$)": {
            "filterLanguageRegex": "^ruby$|^slim\\-lang$|^slim$",
            "decorations": [ {}, { "color": "#0057ae" }, {} ]
        },
        // Ruby: ActiveSupport core extensions
        "(\\.)(deep\\_dup|blank\\?|present\\?|presence|in\\?|duplicable\\?)([\\s\\(\\)\\[\\]\\{\\},;\\.\\+\\-\\|\\&]|$)": {
            "filterLanguageRegex": "^ruby$|^slim\\-lang$|^slim$",
            "decorations": [ {}, { "color": "#0057ae" }, {} ]
        },
        // HTTP: highlight placeholders, syntax highlighting doesn't implement a token for this
        "(\\{\\{.*?\\}\\})": {
            "filterLanguageRegex": "^http$",
            "decorations": [ { "color": "#777777" } ]
        },
        // C/C++: __attribute__(())
        "(\\_\\_attribute\\_\\_)[\\s]?\\(\\(": {
            "filterLanguageRegex": "^c$|^cpp$",
            "decorations": [ { "color": "#ce3102" } ]
        },
        // C/C++: __attribute__(( known attributes ))
        "(\\_\\_attribute\\_\\_)([\\s]?\\(\\()(visibility|\\_\\_visibility\\_\\_)": {
            "filterLanguageRegex": "^c$|^cpp$",
            "decorations": [ { "color": "#ce3102" }, {}, { "fontStyle": "italic" } ]
        },
        // C/C++: __attribute__((visibility("known values")))
        "(\\_\\_attribute\\_\\_)([\\s]?\\(\\()(visibility|\\_\\_visibility\\_\\_)([\\s]?\\()(\\\"hidden\\\"|\\\"default\\\")": {
            "filterLanguageRegex": "^c$|^cpp$",
            "decorations": [ { "color": "#ce3102" }, {}, { "fontStyle": "italic" }, {}, { "color": "red", "fontStyle": "italic" } ]
        },
        // C++: thread_local storage
        "(\\bthread\\_local\\b)": {
            "filterLanguageRegex": "^cpp$",
            "decorations": [ { "color": "#808000", "fontWeight": "bold" } ]
        },
        // YAML: placeholder in strings
        "(\\%\\{.*?\\})": {
            "filterLanguageRegex": "^yaml$",
            "decorations": [ { "color": "#777777" } ]
        },
        // C/C++/D: __PRETTY_FUNCTION__, __FILE__, __LINE__
        "(\\_\\_PRETTY\\_FUNCTION\\_\\_|\\_\\_FUNCTION\\_\\_|\\_\\_FILE\\_\\_|\\_\\_LINE\\_\\_)": {
            "filterLanguageRegex": "^c$|^cpp$|^d$",
            "decorations": [ { "color": "#20208c", "fontWeight": "bold" } ]
        },
        // C++: semantic highlighting fix, ensure "auto" is yellow instead of purple (always use keyword color)
        "(\\bauto\\b)[\\s]?": {
            "filterLanguageRegex": "^cpp$",
            "decorations": [ { "color": "#808000" } ]
        },
        // C++: semantic highlighting fix, ensure "nullptr" is ALWAYS yellow and never purple (it is purple at some places)
        "(\\bnullptr\\b)": {
            "filterLanguageRegex": "^cpp$",
            "decorations": [ { "color": "#808000" } ]
        },
        // C++: abstract class method
        "(\\bvirtual\\b)(.*?)(\\=[\\s]?)(0)([\\s]?;$)()": {
            "filterLanguageRegex": "^cpp$",
            "regexFlags": "gm",
            "decorations": [ { "fontStyle": "italic" }, { "fontStyle": "italic" }, {}, {}, { "after": { "contentText": " abstract method", "color": "#aaaaaa" } } ]
        },
        // C/C++: #if 0 ... #endif block
        "(^#if 0)(.*?)(^#endif|^#else)": {
            "filterLanguageRegex": "^c$|^cpp$",
            "regexFlags": "gms",
            "decorations": [ {}, { "backgroundColor": "#aaaaaa30", "isWholeLine": true }, {} ]
        },
        // D: highlight some common classes and aliases as types (purple)
        "(\\bsize_t\\b|\\bptrdiff_t\\b)": {
            "filterLanguageRegex": "^d$",
            "decorations": [ { "color": "#8d268f" } ]
        },
        // CMake: target types, visibility and other identifiers
        "(\\bPUBLIC\\b|\\bPRIVATE\\b|\\bSTATIC\\b|\\bSHARED\\b|\\bEXECUTABLE\\b|\\bINTERFACE\\b|\\bIMPORTED\\b|\\bGLOBAL\\b|\\bALIAS\\b|\\bTARGET\\b|\\bPROPERTY\\b|\\bPROPERTIES\\b)": {
            "filterLanguageRegex": "^cmake$",
            "decorations": [ { "color": "#008000", "fontWeight": "normal" } ]
        },
        // CMake: some keywords which should be differently colored
        "(\\bNOT\\b|\\STREQUAL\\b|\\bEQUAL\\b|\\bCOMPARE\\b|\\bDEFINED\\b|\\bQUIET\\b|\\bREQUIRED\\b|\\bCOMMAND\\b|\\bCOMMENT\\b|\\bALL\\b|\\bDEPENDS\\b)": {
            "filterLanguageRegex": "^cmake$",
            "decorations": [ { "color": "#008000", "fontWeight": "normal" } ]
        },
        // CMake: variables
        "(\\$\\{.*?\\})": {
            "filterLanguageRegex": "^cmake$",
            "decorations": [ { "color": "#808000" } ]
        },
        // CMake: custom macros (shared-cmake-modules)
        "(\\bCreateTarget\\b|\\bCreateTargetFromPath\\b|\\bProjectConfigureFile\\b|\\bd\\_define\\_version\\b|\\bd\\_install\\_injectors\\b)": {
            "filterLanguageRegex": "^cmake$",
            "decorations": [ { "color": "#644a9b" } ]
        },
        // CMake: boolean (ON/OFF)
        "(\\bON\\b|\\bY\\b|\\bTRUE\\b)": {
            "filterLanguageRegex": "^cmake$",
            "decorations": [ { "color": "#30a030", "fontWeight": "normal" } ]
        },
        "(\\bOFF\\b|\\bN\\b|\\bFALSE\\b)": {
            "filterLanguageRegex": "^cmake$",
            "decorations": [ { "color": "#e05050", "fontWeight": "normal" } ]
        },
        // CMake: cache and data types
        "(\\bCACHE\\b|\\bFORCE\\b|\\bSTRING\\b|\\bPATH\\b|\\bBOOL\\b)": {
            "filterLanguageRegex": "^cmake$",
            "decorations": [ { "color": "#006e28", "fontWeight": "normal" } ]
        },
        // CMake: aliased target names
        "(\\b[\\w\\-]+::[\\w\\-]+\\b)": {
            "filterLanguageRegex": "^cmake$",
            "decorations": [ { "color": "#b08000", "fontWeight": "normal" } ]
        },
        // Markdown: strike through
        "(\\~\\~.*?\\~\\~)": {
            "filterLanguageRegex": "^markdown$",
            "decorations": [ { "textDecoration": "line-through" } ]
        },
        // ShellScript: /dev/null highlighting fix
        "(\\/dev\\/null)": {
            "filterLanguageRegex": "^shellscript$",
            "decorations": [ { "color": "#5484cb80", "fontStyle": "italic" } ]
        },
        // ShellScript: continued line (\): highlight fix
        // "(\\\n)([ ]*?)(\\b[\\w].*?[ ]|\\b[\\w].*?\\b)": {
        //     "filterLanguageRegex": "^shellscript$",
        //     "regexFlags": "gm",
        //     "decorations": [ {}, {}, { "color": "#000000" } ]
        // },
        // SQL: highlighting rules for MariaDB/MySQL dialect
        "(#.*)": {
            "filterLanguageRegex": "^sql$",
            "decorations": [ { "color": "#737373" } ] // comments
        },
        "(\\b0x[0-9A-Fa-f]+\\b)": {
            "filterLanguageRegex": "^sql$",
            "decorations": [ { "color": "#000080" } ] // hexadecimal numbers
        },
        // systemd: highlight services and targets
        "(\\b[\\w\\-]+\\.)(service|target)\\b": {
            "filterLanguageRegex": "^systemd\\-unit\\-file$",
            "decorations": [ { "color": "#006e28" }, { "color": "#006e28", "fontStyle": "italic" } ]
        },
        // diff: inserted and deleted lines
        "(^\\+[^+])(.*)": {
            "filterLanguageRegex": "^diff$",
            "regexFlags": "gm",
            "decorations": [ {}, { "backgroundColor": "#006e2808", "isWholeLine": true } ]
        },
        "(^\\-[^-])(.*)": {
            "filterLanguageRegex": "^diff$",
            "regexFlags": "gm",
            "decorations": [ {}, { "backgroundColor": "#bf030308", "isWholeLine": true } ]
        },
        // Slim: code line
        "(^[ ]*?)([-=])([^-=])(.*?$)": {
            "filterLanguageRegex": "^slim\\-lang$|^slim$",
            "regexFlags": "gm",
            "decorations": [ {}, { "color": "#ce3102", "fontWeight": "bold" }, {}, { "backgroundColor": "#eeeeee60", "isWholeLine": true } ]
        },
        "(^[ ]*?)(==)([^-=])(.*?$)": {
            "filterLanguageRegex": "^slim\\-lang$|^slim$",
            "regexFlags": "gm",
            "decorations": [ {}, { "color": "#ce3102", "fontWeight": "bold" }, {}, { "backgroundColor": "#eeeeee60", "isWholeLine": true } ]
        }
    }
}
```
