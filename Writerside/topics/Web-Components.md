# Web Components

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/tutorial/09_webcomponents/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (tutorial/09_webcomponents)</p>
</tldr>


Web Components are a collection of technologies, supported by all modern browsers, that let developers reuse custom
components in a framework-agnostic way. Although PuePy is an esoteric framework, and no major component libraries
exist for it (as they do with React or Vue), you can use Web Component widgets easily in PuePy and make use of common
components available on the Internet.

## Using Shoelace

Shoelace is a popular and professionally developed suite of web components for building high quality user experiences.
In this example, we'll see how to use Shoelace Web Components inside a project of ours.

### Adding remote assets

In `index.html`, we'll add the remote assets we need for Shoelace

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@2.15.1/cdn/themes/light.css"/>
<script type="module"
      src="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@2.15.1/cdn/shoelace-autoloader.js"></script>
```

### Using Web Components in Python

Because WebComponents are initialized just like other HTML tags, they can be used directly in Python:

```Python
from puepy import Application, Page, t

app = Application()


@app.page()
class DefaultPage(Page):
    def populate(self):
        with t.sl_dialog(label="Dialog", classes="dialog-overview", tag="sl-dialog", ref="dialog"):
            t("Web Components are just delightful.")
            t.sl_button("Close", slot="footer", variant="primary", on_click=self.on_close_click)
        t.sl_button("Open Dialog", tag="sl-button", on_click=self.on_open_click)

    def on_open_click(self, event):
        self.refs["dialog"].element.show()

    def on_close_click(self, event):
        self.refs["dialog"].element.hide()


app.mount("#app")
```

### Access methods and properties of web components

Web Components are meant to be access directly, like this in JavaScript:

```html
<sl-dialog id="foo"></sl-dialog>

<script>
    document.querySelector("#foo").show()
</script>
```

The actual DOM elements are accessible in PuePy, but require using the `.element` attribute of the higher level Python
instance of your tag:

```Python
self.refs["dialog"].element.show()
```
