# Hello, World!

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/01_hello_world/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (01_hello_world)</p>
</tldr>

Let's start with the simplest possible.

Describe what the user will learn and accomplish in the first part, then write a step-by-step procedure but on a
real-world example.

```Python
from puepy import Application, Page, t

app = Application()


@app.page()
class HelloWorldPage(Page):
    def populate(self):
        t.h1("Hello, World!")


app.mount("#app")
```

In Hello World, we:

1. Create an Application instance
2. Create a Page class
3. Mount the application instance to an HTML selector (`#app`). A `<div id="app"></div>` exists in `index.html`.

Don't worry too much about the `populate()` method right now; just understand that `populate()` is where pages and
components define their contents. In this example, we create an `h1` header tag with "Hello, World!" in its contents.

# Applications and pages

The `HelloWorld` page class is registered to the application with `app.page()`. In future examples, we will include
details on how to route different URLs to different pages, but for now, we only have one default page.
