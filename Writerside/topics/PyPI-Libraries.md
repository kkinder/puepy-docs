# PyPI Libraries

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/tutorial/08_libraries/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (tutorial/08_libraries)</p>
</tldr>

## PyPi Libraries (when using CPython/Pyodide)

In our example, we use `<script type="py"...>` in index.html to specify that the runtime is CPython, not MicroPython.
Doing so gives you access to a great many packages available on PyPi just by adding them to the `packages` section of
your [PyScript Config](PyScript-Config.md).

```json
{
  "name": "PuePy Tutorial",
  "debug": true,
  "files": {
    "../puepy-bundle.zip": "./*"
  },
  "packages": [
    "beautifulsoup4"
  ]
}
```

From there, we import BeautifulSoup like we normally would in our source file:

```Python
from bs4 import BeautifulSoup, Comment
```

For more information, including packages available to
MicroPython, [refer to the PyScript docs](https://docs.pyscript.net/2024.5.2/user-guide/configuration/#packages).