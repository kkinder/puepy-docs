# PyScript Config

PyScript's configuration is fully [documented](https://docs.pyscript.net/2024.6.2/user-guide/configuration/) in the
PyScript documentation. Configuration for PuePy simply requires adding the PuePy runtime files (
see [Installation](Installation.md)) and
Morphdom:

```JSON
{
  "name": "PuePy Tutorial",
  "debug": true,
  "files": {
    "./puepy-bundle.zip": "./*"
  },
  "js_modules": {
    "main": {
      "https://cdn.jsdelivr.net/npm/morphdom@2.7.2/+esm": "morphdom"
    }
  }
}
```