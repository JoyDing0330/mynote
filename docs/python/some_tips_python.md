## Setup Formatter
* `pip install black`
* `View` > `Command Palette` (or `Shift`+`cmd`+`p`)
* Preferences: Open User Settings (JSON)
* Add the following lines to the settings file:
```json
{
    "python.formatting.provider": "black",
    "[python]": {
        "editor.defaultFormatter": "ms-python.black-formatter",
        "editor.formatOnSave": true # ensures that Black runs automatically every time you save a file.
    }
}
```
* Manually Format a File: Press `Shift` + `Alt` + `F` (or `Shift` + `Option` + `F` on macOS)

* Format from the terminal
```sh
black your_script.py
```

## Setup Snippets
