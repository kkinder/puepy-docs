# Counter

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/03_counter/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (03_counter)</p>
</tldr>

Events are triggered when you pass `on_<event_name>` to a tag:

```Python
from puepy import Application, Page, t

app = Application()


@app.page()
class CounterPage(Page):
    def initial(self):
        return {"current_value": 0}

    def populate(self):
        with t.div(classes="button-box"):
            t.button("-", classes=["button", "decrement-button"], on_click=self.on_decrement_click)
            t.span(str(self.state["current_value"]), classes="count")
            t.button("+", classes="button increment-button", on_click=self.on_increment_click)

    def on_decrement_click(self, event):
        self.state["current_value"] -= 1

    def on_increment_click(self, event):
        self.state["current_value"] += 1


app.mount("#app")
```

In this example, we introduce three new things:

- `initial()` defines the pages's **working state**.
- `populate()` does conditional rendering based on that working state and defines a new `input` element, with
  a `bind="name"` parameter.
- `on_decrement_click()` and `on_increment_click()` define event handlers for when the buttons created in `populate()`
  are clicked.

<tip>
    The <code>event</code> parameter sent to event handlers is the same as it is in JavaScript.
</tip>

<note>

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

This is because PuePy's ReactiveDict cannot detect "deep" changes to state automatically. If you are modifying objects
in-place, use this context manager notation:

<code-block lang="python">
# This will work
with self.reactive_dict.mutate("my_list", "my_dict"):
    self.state["my_list"].append("spam")
    self.state["my_dict"]["spam"] = "eggs"
</code-block>
</warning>
For further guidance, see the <a href="Reactivity.md">Reactivity</a> topic.
</note>