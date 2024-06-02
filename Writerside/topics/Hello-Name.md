# Hello, Name

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/02_hello_name/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (02_hello_name)</p>
</tldr>

- 

Component (and thus, application) state is reactive in PuePy and initialized using the `initial()` method.

```Python
from puepy import Application, Page, t

app = Application()


@app.page()
class HelloNamePage(Page):
    def initial(self):
        return {"name": ""}

    def populate(self):
        if self.state["name"]:
            t.h1(f"Hello, {self.state['name']}!")
        else:
            t.h1(f"Why don't you tell me your name?")

        with t.div(style="margin: 1em"):
            t.input(bind="name", placeholder="name", autocomplete="off")


app.mount("#app")
```

In this example, we introduce three new things:

- `initial()` defines the pages's **working state**.
- `populate()` does conditional rendering based on that working state.
- `populate()` also defines a new `input` element, with a `bind="name"` parameter.

## Reactivity

A page or component's initial state is defined by the `initial()` method. If implemented, it should return a dictionary,
which is then stored as a special reactive dictionary, `self.state`. As the state is modified, the component redraws,
updating the DOM as needed.

<warning>
Modifying <code>.state</code> values in-place <emphasis>will not work</emphasis>

<code-block lang="python">
# THESE WILL NOT WORK:
self.state["my_list"].append("spam")
self.state["my_dict"]["spam"] = "eggs"
</code-block>

This is because PuePy's ReactiveDict cannot detect "deep" changes to state automatically. If you are  modifying objects
in-place, use this context manager notation:

<code-block lang="python">
# This will work
with self.reactive_dict.mutate("my_list", "my_dict"):
    self.state["my_list"].append("spam")
    self.state["my_dict"]["spam"] = "eggs"
</code-block>

For more information, see the [Reactivity Developer Guide](Reactivity.md).
</warning>
