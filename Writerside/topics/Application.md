f# Application

You must always create a PuePy `Application` instance, even if only one page exists on your site. Additionally, each
running code base should only create one application instance. To make use of the application, you will need to define
at least one page and also mount the application (and thus the) page onto an existing HTML element.

```Python
from puepy import Application

app = Application()


@app.page()
class HelloWorldPage(Page):
    def populate(self):
        t.h1("Hello, World!")


app.mount("#app")
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>PuePy Hello, World</title>
    <meta charset="UTF-8">
    <meta name="viewport" 
          content="width=device-width,initial-scale=1.0">
    <link rel="stylesheet"
          href="https://pyscript.net/releases/2024.6.2/core.css">
    <script type="module"
            src="https://pyscript.net/releases/2024.6.2/core.js"
    ></script>
</head>
<body>
    <div id="app"></div>
    <script type="mpy" 
            src="./hello_world.py" 
            config="../pyscript.json"></script>
</body>
</html>
```

## Application state

Like components, applications can have state.

```Python
class AwesomeApp(Application):
    def initial(self):
        return {"authenticated_user": ""}
```

Application state can be access and updated in components using `self.application.state`

## Instance methods

- `app.install_router(router_class, **kwargs)`: Instantiates a Router class with any keyword arguments and configures it on the application. See also the [Router Developer Guide](Router.md) or [Routing Tutorial chapter](Routing.md).
- `@app.page(route=None, name=None)`: Decorator to add route for a given page.
- `app.mount(selector_or_element, path=None, page_kwargs=None)`: Should only be called once. Mounts the application to the given selector_or_element (either a string [query selector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) or a DOM element). If `path` is specified, it will override the window location in determining the route. If specified, `page_kwargs` can be a dict passed to the Page displayed.

## Instance attributes

- `app.router`: router object, if installed
- `app.default_page`: default page class to render if no matching routes are found

