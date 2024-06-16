# Installation

## Client-side installation

Although PuePy is [available on pypi](https://pypi.org), because PuePy is intended primarily as a client-side framework,
"installation" is best achieved by downloading the wheel file and including it in your pyscript `packages` configuration.

A simple first project (with no web server) would be:

- `index.html` (index.html file)
- `pyscript.json` (pyscript config file)
- `hello.py` (Hello World code)
- `puepy-0.3.0-py3-none-any.whl` (PuePy wheel file)

The runtime file would contain only the files needed to actually execute PuePy code; no tests or other files. Runtime
zips are available in each release's notes on GitHub.

### Downloading client runtime

```Bash
curl -O https://files.pythonhosted.org/packages/b0/d2/2dcd29df8eea125e2b4ba31a3f837886a96c664e61f728e1375d54f9f929/puepy-0.3.0-py3-none-any.whl
```

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>PuePy Hello, World</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <!-- Load PyScript from CDN -->
    <link rel="stylesheet" href="https://pyscript.net/releases/2024.6.1/core.css">
    <script type="module" src="https://pyscript.net/releases/2024.6.1/core.js"></script>
</head>
<body>
    <!-- Where application is mounted -->
    <div id="app"></div>

    <!-- PyScript Config -->
    <script type="mpy" src="./hello.py" config="./pyscript.json"></script>
</body>
</html>
```

### pyscript.json

`pyscript.json` is your basic PyScript config file (see [PyScript Config](PyScript-Config.md)). At a minimum, you
need to include the puepy-bundle.zip:

```json
{
  "name": "PuePy Tutorial",
  "debug": true,
  "packages": [
    "./puepy-0.3.0-py3-none-any.whl"
  ],
  "js_modules": {
    "main": {
      "https://cdn.jsdelivr.net/npm/morphdom@2.7.2/+esm": "morphdom"
    }
  }
}
```

This configuration tells PyScript to include our PuePy runtime environment `./puepy-0.3.0-py3-none-any.whl` and
[Morphdom](https://github.com/patrick-steele-idem/morphdom), which PuePy uses to modify the DOM at runtime.

### hello.py

```Python
from puepy import Application, Page, t

app = Application()


@app.page()
class HelloWorldPage(Page):
    def populate(self):
        t.h1("Hello, World!")


app.mount("#app")
```

## Other installation strategies

As you can see, current installation practice is little more than regular PyScript installation, plus a zip file and
a JavaScript module.

You may install from PyPi into your web project's backend code, but you'd be responsible for serving code from the
PuePy to the browser.
