# Application

You must always create a PuePy `Application` instance, even if only one page exists on your site. Additionally, each
running code base should only create one application instance. To make use of the application, you will need to define
at least one page and also mount the application (and thus the) page onto an existing HTML element.

<tabs>
    <tab id="html-ver" title="index.html">
        <code-block lang="html">
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;title&gt;PuePy Hello, World&lt;/title&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width,initial-scale=1.0&quot;&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://pyscript.net/releases/2024.5.2/core.css&quot;&gt;
    &lt;script type=&quot;module&quot; src=&quot;https://pyscript.net/releases/2024.5.2/core.js&quot;&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&quot;app&quot;&gt;&lt;/div&gt;
    &lt;script type=&quot;mpy&quot; src=&quot;./hello_world.py&quot; config=&quot;../pyscript.json&quot;&gt;&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
</code-block>
    </tab>
    <tab id="main-ver" title="main.py">
        <code-block lang="python">
        from puepy import Application

        app = Application()
        
        
        @app.page()
        class HelloWorldPage(Page):
        def populate(self):
        t.h1("Hello, World!")
        
        
        app.mount("#app")
</code-block>
    </tab>
</tabs>

## Instance methods

- `app.install_router(router_class, **kwargs)`: Instantiates a Router class with any keyword arguments and configures it on the application. See also the [Router Developer Guide](Router.md) or [Routing Tutorial chapter](Routing.md).
- `@app.page(route=None, name=None)`: Decorator to add route for a given page.
- `app.mount(selector_or_element, path=None, page_kwargs=None)`: Should only be called once. Mounts the application to the given selector_or_element (either a string [query selector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) or a DOM element). If `path` is specified, it will override the window location in determining the route. If specified, `page_kwargs` can be a dict passed to the Page displayed.

## Instance attributes

- `app.router`: router object, if installed
- `app.default_page`: default page class to render if no matching routes are found

