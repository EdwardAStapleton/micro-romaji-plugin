# micro-romaji-plugin — SKK-style Japanese Input for the micro editor

[![Releases - Download](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/EdwardAStapleton/micro-romaji-plugin/releases)

A micro text editor plugin that provides a compact SKK-inspired Japanese input method. Type romaji and convert to hiragana or katakana. Use it inside micro in terminal or GUI mode. This README explains features, install steps, usage, configuration, internals, tips, and development notes.

Badges
- Topics: hiragana · input · input-method · japanese · kana · katakana · micro · micro-editor · micro-plugin · plugin · romaji · skk · terminal · text-editor
- Platform: Linux · macOS · Windows (WSL)
- License: MIT

Preview image
![Hiragana table](https://upload.wikimedia.org/wikipedia/commons/1/1f/Japanese_Hiragana_Katakana.svg)

Contents
- What it does
- Key features
- Quick start
- Releases and download (required file run)
- Installation methods
  - Automatic (micro plugin manager)
  - Manual (download and run release file)
  - Install from source
- Configuration
  - Basic settings
  - Keybindings
  - Modes: hiragana, katakana, ascii
  - Custom romaji map
- Usage
  - Typing flow
  - Inline conversion
  - Lookups and suggestions
  - Converting existing text
- SKK-inspired behavior
  - Candidate flow
  - Dictionary model
  - History and learning
- Examples and workflows
  - Writing in kanji-heavy text
  - Editing markdown with Japanese
  - Terminal-only workflow
- Troubleshooting and common fixes
- Developer guide
  - Code layout
  - Tests
  - Debugging
- Contributing
- Changelog and releases
- License
- Credits and resources

What it does
The plugin converts romaji input to Japanese kana in micro. It follows a flow inspired by the SKK method. The plugin supports hiragana and katakana. It supports ASCII passthrough and punctuation mapping. It exposes commands and keybindings. It offers a small built-in dictionary and hooks for custom user dictionaries.

Key features
- Live romaji to kana conversion.
- Toggle between hiragana and katakana modes.
- SKK-style candidate selection for kanji conversion.
- Config file support for custom romaji maps.
- Integrates with micro keybindings and menu.
- Small, fast, and terminal-friendly.
- Works with micro command palette and scripts.

Quick start
1. Install the plugin using the method you prefer.
2. Enable the plugin.
3. Switch to hiragana or katakana mode and type romaji.
4. Use the convert key to accept a candidate.
5. Use the candidate keys to choose kanji when available.

Releases and download (required file run)
The plugin releases are available on GitHub Releases:
https://github.com/EdwardAStapleton/micro-romaji-plugin/releases

This link points to the Releases page and contains packaged assets. Download the release asset that matches your platform. The release package includes an install script or a plugin file. After you download the release file, extract it and execute the install file that comes inside. Example steps you can follow after you download the archive:

- If you download a tarball named micro-romaji-<version>.tar.gz:
  - tar -xzf micro-romaji-<version>.tar.gz
  - cd micro-romaji-<version>
  - chmod +x install.sh
  - ./install.sh

- If you download a zip file named micro-romaji-<version>.zip:
  - unzip micro-romaji-<version>.zip
  - cd micro-romaji-<version>
  - ./install.sh

The install script installs the plugin into micro's plugin path and sets default config files. The Releases page contains platform-specific builds and example configs. Visit the Releases page and pick the asset that matches your system.

Installation methods

Automated install (micro plugin manager)
If you use micro's plugin manager, you can install directly from the repository. The plugin supports the micro plugin manager. Run:

micro -plugin install EdwardAStapleton/micro-romaji-plugin

If micro's plugin manager cannot fetch the repo for any reason, fall back to the release archive and run the included install script as shown in the Releases section.

Manual install (download and run release file)
Follow these steps when you prefer direct control or when you are offline.

1. Visit the Releases page:
https://github.com/EdwardAStapleton/micro-romaji-plugin/releases

2. Download the archive that matches your OS. The archive contains:
- plugin file(s)
- config sample (romaji_map.json)
- install script (install.sh)
- README and examples

3. Extract and run the install script:
- tar -xzf micro-romaji-<version>.tar.gz
- cd micro-romaji-<version>
- chmod +x install.sh
- ./install.sh

The script installs files to the correct micro config directory:
- Linux/macOS: $XDG_CONFIG_HOME/micro/plug or ~/.config/micro/plug
- Windows (WSL): ~/.config/micro/plug or /home/<user>/.config/micro/plug

After install, restart micro to pick up the plugin. The install script also installs a sample romaji map and sample user dictionary.

Install from source
Clone the repository and copy files into micro's plugin dir.

git clone https://github.com/EdwardAStapleton/micro-romaji-plugin.git
cd micro-romaji-plugin
cp -r plugin ~/.config/micro/plug/micro-romaji-plugin
cp romaji_map.json ~/.config/micro/romaji_map.json

Restart micro.

Configuration

Config file locations
- Global: $XDG_CONFIG_HOME/micro/plugins/micro-romaji-plugin/
- User: ~/.config/micro/plugins/micro-romaji-plugin/
- Romaji map: ~/.config/micro/micro-romaji/romaji_map.json

Default config
The plugin ships with a minimal romaji map and a small base dictionary. The default config sets:
- default_mode: "hiragana"
- convert_key: "Ctrl-k"
- toggle_key: "Ctrl-j"
- candidate_keys: "1..9"
- ascii_toggle: "Ctrl-a"
- use_user_dict: true
- user_dict_path: "~/.config/micro/micro-romaji/user_dict.json"

Sample romaji_map.json
The romaji map maps input sequences to kana. A small excerpt:

{
  "a": "あ",
  "i": "い",
  "u": "う",
  "e": "え",
  "o": "お",
  "ka": "か",
  "ki": "き",
  "ku": "く",
  "ke": "け",
  "ko": "こ",
  "kya": "きゃ",
  "kyo": "きょ"
}

Mode settings
- default_mode: choose "hiragana" or "katakana".
- ascii_mode: when true, pass through ASCII without conversion.
- punctuation_map: map ascii punctuation to Japanese punctuation when set.

Keybindings
The plugin binds a small set of keys. You can override these in micro's keybindings config.

Default keys:
- Ctrl-j: toggle mode (hiragana/katakana)
- Ctrl-k: convert current candidate and commit
- Ctrl-l: show candidate list (if any)
- Ctrl-a: toggle ascii passthrough
- ESC: cancel input mode

You can add custom bindings in your micro settings. Example:

"Bindings": {
  "Ctrl-j": "plugin:micro-romaji-toggle-mode",
  "Ctrl-k": "plugin:micro-romaji-convert"
}

Modes: hiragana, katakana, ascii
- Hiragana: default mode. Romaji maps to hiragana glyphs.
- Katakana: maps same romaji sequences to katakana.
- ASCII: pass through ASCII input with no conversion. Use this when you type code or commands.

Switch modes with the toggle key. The plugin shows a small status indicator in micro's status bar when active.

Custom romaji map
You can override or extend the romaji map. Create or edit romaji_map.json in your config path. The plugin merges the user map with the default map. Provide mappings for sequences and multi-letter constructs such as "shi", "cha", "tsu".

Custom mapping example:
{
  "shi": "し",
  "tsu": "つ",
  "cha": "ちゃ",
  "fa": "ふぁ",
  "va": "ゔぁ"
}

Punctuation mapping
The plugin can map ASCII punctuation to Japanese punctuation. Example mapping:

{
  ",": "、",
  ".": "。",
  "?": "？",
  "!": "！",
  ":": "：",
  ";": "；",
  "\"": "「",
  "'": "」"
}

You can turn mapping on or off.

Grammar and composition rules
The plugin implements common romaji conventions:
- Double consonant: "tt" -> small tsu (っ).
- Syllabic n: "n" before consonant -> ん.
- Long vowel: "ou" or "oo" -> おう or おお depending on map. You can set a preference for "おう" or "おー".

These rules follow common SKK and romaji input method behavior. You can tune edge cases in romaji_map.json.

Usage

Typing flow
1. Activate the plugin if not active.
2. Type romaji as in any IME.
3. The plugin buffers input and shows an inline preedit string.
4. When the sequence matches a mapping, the plugin shows the kana candidate inline.
5. Press the convert key (Ctrl-k by default) to commit the candidate.
6. If kanji candidates exist, the plugin shows a candidate list.
7. Pick a candidate via the number keys or cycle through them.

Inline conversion example
- Type: "konnichiha"
- The plugin buffers input and converts to "こんにちは" on convert.
- If you have a kanji candidate for part of the phrase, the plugin shows selection for that segment.

Candidate selection and kanji conversion
When the plugin shows candidates, it uses its small dictionary and the user dictionary. The plugin shows candidates in a popup. You can pick one by pressing a number. The plugin accepts either the kana result or a chosen kanji candidate.

Dictionary integration
The plugin reads a built-in base dictionary. You can add a user dictionary. The user dictionary can contain entries in the form:

{
  "こんにちは": ["今日は", "今日は"],
  "わたし": ["私", "渡し"]
}

The plugin ranks candidates by usage count and recency. It learns from conversions and updates the user dictionary when you pick candidates.

Converting existing text
You can select existing romaji text and run the convert command. The plugin converts the selection inline and offers the same candidate flow as typed input.

SKK-inspired behavior

Candidate flow
The plugin follows a small SKK-like flow:
- Buffer romaji until a delimiter or convert key.
- Show kana candidate.
- If a kanji conversion exists, show candidate list.
- Accept candidate to replace the buffered text.

The plugin uses a segmentation strategy similar to SKK to split input into morphemes. This helps when you type long runs without spaces.

Dictionary model
- Built-in dictionary: small core with common words and kanji conversions.
- User dictionary: editable JSON file. The plugin writes usage data and new entries here.
- System dictionary: optional hook to call external conversion tools. See advanced integration.

History and learning
The plugin tracks conversions. It increments counters when you choose a candidate. It stores counters in the user dictionary to bias future candidate order. The plugin supports a simple decay algorithm to age old entries.

Examples and workflows

Writing a Japanese note
- Enable the plugin.
- Set mode to hiragana.
- Type: "ashita ame ga furu."
- Convert to: "あした雨が降る。"
- Toggle to katakana for loan words: "コンピュータ".

Editing Markdown
- Use ASCII mode for code blocks and inline code.
- The plugin recognizes fenced code blocks and auto-disables conversion inside them.
- The plugin auto-enables Japanese mappings in normal text.

Terminal-only workflow
- Run micro from a terminal.
- Use the same keys.
- The plugin uses micro's status bar to show mode and preedit.
- Use candidate numbers by pressing number keys.

Troubleshooting and common fixes

Plugin does not load
- Ensure the plugin files live in micro's plugin folder.
- Restart micro after install.
- Check that the plugin filename matches the plugin.json entry if present.

Keybinding conflicts
- Some key combos may conflict with your terminal or OS shortcuts.
- Rebind the plugin keys in your micro settings file if needed.

Conversion wrong for some romaji
- Edit romaji_map.json to add or override mappings.
- Use the user dictionary to add kanji conversions.

Candidate list does not appear
- Ensure you use the convert key after buffering input.
- Check the user dictionary for available conversions.

Debug logs
- Enable debug mode in the plugin config to print logs to micro's message area or a log file.
- Debug mode shows the buffered input, the candidate list, and dictionary lookups.

Developer guide

Code layout
- plugin/ — core plugin entry points and event handlers
- data/ — built-in romaji map and base dictionary
- tests/ — unit tests for conversion routines
- scripts/ — build and packaging scripts
- docs/ — extended documentation and examples

Core modules
- preedit.lua (or preedit.go depending on implementation): handles buffered input.
- converter.lua: converts romaji to kana using romaji map and rules.
- dict.lua: dictionary lookup and candidate generation.
- ui.lua: candidate popup and status bar updates.
- config.lua: load and merge config files.

Tests
The repository includes unit tests for conversion rules. The tests assert:
- mapping for common syllables
- double consonant behavior
- syllabic n handling
- long vowel preferences
- candidate ranking

Run tests locally with the provided test runner:
sh scripts/run-tests.sh

Debugging
- Use micro's debug console to print plugin logs.
- Set log_level = "debug" in config.
- The plugin logs romaji buffer state, matched mappings, and candidate arrays.

Contributing

How to contribute
- Fork the repository.
- Create a feature branch.
- Run tests and ensure conversions pass.
- Open a pull request with your changes.
- Add tests for any new conversion rule or dictionary change.

Style guide
- Keep functions small and focused.
- Add clear comments for conversion rules.
- Maintain a high test coverage for conversion logic.

Localization and internationalization
- The plugin stores messages in an i18n folder.
- English is default. You can add translations and submit them.

Changelog and releases
The Releases page contains full release notes and packaged assets. Always review the release notes before upgrading. To install a release, download the package and run the included install script.

Releases:
https://github.com/EdwardAStapleton/micro-romaji-plugin/releases

When you use the Releases page, pick the asset for your platform. The install script sets up config files and places plugin files into micro's plugin folder. The release notes list breaking changes and migration steps.

Advanced topics

Hooking an external backend
The plugin supports an optional backend for kanji conversion. You can configure the plugin to call:
- an external skk-server
- a local mecab-based converter
- a cloud API (user provided key)

Configure the backend URL in config:
{
  "backend": {
    "type": "http",
    "url": "http://localhost:1178/convert",
    "timeout_ms": 500
  }
}

When configured, the plugin sends buffered kana to the backend and shows returned candidates. The plugin uses the user dictionary as a fallback.

Performance
The plugin uses a small in-memory map for fast lookups. It keeps the user dictionary on disk and loads it at startup. For very large dictionaries, use the backend hook or split the dictionary into smaller files.

Security
The plugin reads user config files. Keep your system secure and review scripts before running them. Use the Releases page to get signed assets when available.

Examples of romaji rules and edge cases

Double consonant (促音 sokuon)
- Input: "kitte"
- Output: "きって" (small tsu inserted before t)
- Rules: double consonant maps to small tsu for consonants other than n.

Syllabic n (ん)
- Input: "kanpai"
- Output: "かんぱい"
- Rules: The plugin checks following character to decide if n forms ん or part of next syllable.

Long vowels
- Input: "toukyou"
- Output: "とうきょう" or "とーきょう" depending on long vowel mode.
- Default: expand to kana sequences (おう or う) as per romaji_map preference.

Palatalized sounds
- Input: "kyo", "sha", "chu"
- Output: "きょ", "しゃ", "ちゅ"

Small vowels and extended combos
- Support for "fa", "fi", "fe", "fo" via mapped kana or katakana sequences.

Katakana mode and loan words
- Toggle to katakana and type "konpyu-ta" to get "コンピューター".
- The plugin maps vowel length to "ー" when in katakana mode if configured.

Punctuation and spacing
- When punctuation mapping is enabled, ASCII punctuation converts to Japanese punctuation in natural text.
- The plugin can auto-insert a space or a Japanese full-width space (　) after conversions if configured.

Sample configuration file (full)
Place this file in ~/.config/micro/plugins/micro-romaji-plugin/config.json

{
  "default_mode": "hiragana",
  "convert_key": "Ctrl-k",
  "toggle_key": "Ctrl-j",
  "ascii_toggle": "Ctrl-a",
  "candidate_keys": "1..9",
  "romaji_map_path": "~/.config/micro/micro-romaji/romaji_map.json",
  "user_dict_path": "~/.config/micro/micro-romaji/user_dict.json",
  "punctuation_map": true,
  "backend": null,
  "log_level": "info",
  "long_vowel_preference": "kana",
  "auto_space_after_commit": false,
  "disable_in_code_blocks": true
}

FAQs

Q: Can I use this in Windows?
A: Yes. Use micro on Windows or WSL. Put the plugin files into the correct config path. The install script detects WSL and uses the Linux path.

Q: How do I add a kanji conversion?
A: Edit the user_dict.json and add the kana to kanji mapping. The plugin updates frequencies when you select candidates.

Q: The plugin converts inside code blocks. How do I disable that?
A: Set disable_in_code_blocks to true in config. The plugin detects fenced code blocks in common markup formats.

Q: How do I disable punctuation mapping?
A: Set punctuation_map to false in config.

Q: I want to use a different convert key. Can I change it?
A: Yes. Adjust convert_key in config or set a micro keybinding.

Q: Does it support macOS system IME?
A: The plugin works inside micro. If you use a system IME, you can still run this plugin to convert romaji typed in micro. The plugin operates independently of system IMEs.

Testing and CI
The project includes a simple test harness. The CI runs unit tests on supported platforms. The tests cover:
- romaji mapping
- sokuon behavior
- syllabic n rules
- candidate ranking

To run tests locally:
sh scripts/run-tests.sh

If tests fail, check the romaji map and debug logs.

Packaging and releases
Releases include:
- Zip and tarball assets
- Install script
- Checksums

When publishing a release:
- Bump the version in plugin.json and scripts
- Update changelog
- Build assets
- Upload to the Releases page

License
The project uses the MIT license. Check the LICENSE file in the repo for full terms.

Credits and resources
- SKK method: inspiration for candidate flow and dictionary model.
- micro editor: plugin host. See https://micro-editor.github.io/
- Wikimedia hiragana and katakana charts for reference and images.
- mecab and other tokenizers for backend integration ideas.

Useful links
- Official releases: https://github.com/EdwardAStapleton/micro-romaji-plugin/releases
- micro editor: https://micro-editor.github.io/
- Romaji conventions: various public resources and IME guides
- Hiragana chart: https://upload.wikimedia.org/wikipedia/commons/1/1f/Japanese_Hiragana_Katakana.svg

Contributing guide (detailed)
- Report issues on GitHub. Provide steps to reproduce, platform, micro version, and plugin version.
- For bug fixes: include a test that reproduces the bug and the fix.
- For new features: add documentation and tests.
- For dictionary updates: keep changes minimal. Prefer to add new user dictionary entries in examples.

Developer notes
- Keep conversion code deterministic.
- Avoid global state where possible.
- Keep user dictionary writes atomic to avoid corruption on crash.
- Use small, readable maps for romaji rules. Large maps slow startup.
- Consider lazy-loading large dictionaries.

Packaging tips
- Use tar.gz for Unix users and zip for cross-platform.
- Include an install.sh that validates micro path and copies files in place.
- Provide an uninstall.sh to remove installed files.

Final pointers
- Use the Releases page to grab packaged assets and the install script. After you download the release file, extract it and run the install script inside the package as shown earlier. The releases page contains platform-specific assets and step-by-step install instructions.

- If the Releases link does not work, check the Releases section in the repository on GitHub. The repo contains manual install instructions and example config files.

Explore examples and tweak the romaji map to match your typing style. The plugin gives a compact SKK-like experience inside micro.