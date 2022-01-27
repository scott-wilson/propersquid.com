+++
title = "Development Setup"
date = 2018-05-27
draft = false

[taxonomies]
categories = ["Programming"]
tags = ["programming", "environment"]
+++

This is mostly for me, but if you want the same (or similar) development setup I have, then here's the tools I use.

# Editor
- [Visual Studio Code](https://code.visualstudio.com/)

# Languages
- [Python](https://www.python.org/)
- [Go](https://golang.org/)
- [Rust](https://www.rust-lang.org/)

# Python Packages
- [yapf](https://pypi.org/project/yapf/)
- [flake8](https://pypi.org/project/flake8/)
- [flake8-docstrings](https://pypi.org/project/flake8-docstrings/)
- [tox](https://pypi.org/project/tox/)
- [tox-pipenv](https://pypi.org/project/tox-pipenv/)
- [tox-pyenv](https://pypi.org/project/tox-pyenv/)
- [pipenv](https://pypi.org/project/pipenv/)
- [pyenv](https://github.com/pyenv/pyenv)
- [hypothesis](https://pypi.org/project/hypothesis/)
- [pytest](https://pypi.org/project/pytest/)
- [pytest-cov](https://pypi.org/project/pytest-cov/)
- [pytest-xdist](https://pypi.org/project/pytest-xdist/)
- [pytest-mock](https://pypi.org/project/pytest-mock/)

# Font
- [Hack](https://sourcefoundry.org/hack/)

# Visual Studio Code Extensions

- Atom Keymap
- Atom One Dark Theme
- Better TOML
- Bookmarks
- C/C++
- C#
- CMake
- CMake Tools
- Docker
- EditorConfig for VS Code
- EditorConfig for VS Code Snippet
- final-newline
- Git History
- gitignore
- GitLens - Git Supercharged
- Go
- Jinja
- MayaPort
- MEL - Maya Embedded Language
- mssql
- Prettify JSON
- Project Manager
- Python
- REST Client
- Rust (rls)
- Settings Sync
- Sort
- Sort JSON objects
- Sort lines
- sprt-imports
- Todo+
- Trailing Spaces
- Travis CI Status
- TSLint
- vscode-database

# Visual Studio Code Settings
```json
{
    // vscode
    "editor.multiCursorModifier": "ctrlCmd",
    "editor.detectIndentation": true,
    "editor.formatOnPaste": false,
    "editor.fontFamily": "Hack, 'Droid Sans Mono', 'Courier New', monospace, 'Droid Sans Fallback'",
    "editor.renderWhitespace": "boundary",
    "editor.minimap.enabled": true,
    "editor.minimap.renderCharacters": false,
    "files.insertFinalNewline": true,
    "files.trimTrailingWhitespace": true,
    "files.trimFinalNewlines": true,
    "workbench.iconTheme": "vs-seti",
    "files.exclude": {
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/.DS_Store": true,
        "**/*.pyc": true
    },
    "atomKeymap.promptV3Features": true,
    "gitlens.advanced.messages": {
        "suppressCommitHasNoPreviousCommitWarning": false,
        "suppressCommitNotFoundWarning": false,
        "suppressFileNotUnderSourceControlWarning": false,
        "suppressGitVersionWarning": false,
        "suppressLineUncommittedWarning": false,
        "suppressNoRepositoryWarning": false,
        "suppressResultsExplorerNotice": false,
        "suppressShowKeyBindingsNotice": true,
        "suppressUpdateNotice": false,
        "suppressWelcomeNotice": true
    },
    "sync.removeExtensions": true,
    "sync.syncExtensions": true,
    "gitlens.keymap": "chorded",
    "workbench.colorTheme": "Atom One Dark",
    "gitlens.historyExplorer.enabled": true,
    "gitlens.codeLens.enabled": false,
    // Typescript/Javascript
    "[typescript]": {
        "editor.rulers": [
            120
        ]
    },
    "[typescriptreact]": {
        "editor.rulers": [
            120
        ]
    },
    "[javascript]": {
        "editor.rulers": [
            120
        ]
    },
    "[javascriptreact]": {
        "editor.rulers": [
            120
        ]
    },
    // only for python language files
    "[python]": {
        "editor.rulers": [
            72,
            79
        ],
        "editor.tabSize": 4,
        "editor.insertSpaces": true
    },
    // pythonVSCode extension
    "python.linting.pylintEnabled": false,
    "python.linting.flake8Enabled": true,
    "python.linting.pep8Enabled": false,
    "python.linting.lintOnSave": true,
    "python.linting.maxNumberOfProblems": 1000,
    "python.formatting.provider": "yapf",
    "python.formatting.yapfArgs": [
        "--style",
        "{based_on_style: pep8, indent_width: 4}"
    ],
    // only for rust language files
    "[rust]": {
        "editor.formatOnSave": true,
        "editor.tabSize": 4,
        "editor.insertSpaces": true
    },
    // Project manager
    "projectManager.sortList": "Name",
    // CMake
    "cmake.buildDirectory": "${workspaceRoot}/../${workspaceRootFolderName}-build"
}

```
