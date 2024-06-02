# Components

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/06_components/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (06_components)</p>
</tldr>

## Registering and reusing components

Define components and reuse them throughout your project by subclassing `puepy.Component` and registering your
component with the `t.component()`. You can also call `t.add_library` with multiple component classes to register many
all at once.

## Slots

Components can optionally define one or more **slots**. A slot is filled in by the code calling the component.

```Python
from puepy import Application, Page, Component, t

app = Application()


# Using the @t.component decorator registers the component for use elsewhere
@t.component()
class Card(Component):
    props = ["type"]
    default_classes = ["card"]

    def populate(self):
        with t.h2(classes=[self.type]):
            self.insert_slot("card-header")
        with t.p():
            self.insert_slot()  # If you don't pass a name, it defaults to the main slot
        t.button("Understood", on_click=self.on_button_click)

    def on_button_click(self, event):
        self.trigger_event("my_custom_event", detail={"type": self.type})


@app.page()
class ComponentPage(Page):
    def initial(self):
        return {"message": ""}

    def populate(self):
        t.h1("Components are useful")

        with t.card(type="success", on_my_custom_event=self.handle_custom_event) as card:
            with card.slot("card-header"):
                t("Success!")
            with card.slot():
                t("Your operation worked")

        with t.card(type="warning", on_my_custom_event=self.handle_custom_event) as card:
            with card.slot("card-header"):
                t("Warning!")
            with card.slot():
                t("Your operation may not work")

        with t.card(type="error", on_my_custom_event=self.handle_custom_event) as card:
            with card.slot("card-header"):
                t("Failure!")
            with card.slot():
                t("Your operation failed")

        if self.state["message"]:
            t.p(self.state["message"])

    def handle_custom_event(self, event):
        self.state["message"] = f"Custom event from card with type {event.detail.get("type")}"


app.mount("#app")
```

In this example, the `Card` component defines two slots where content will be inserted by the page making use of the
card:

```Python
with t.h2():
    self.insert_slot("card-header")
with t.p():
    self.insert_slot()  # If you don't pass a name, it defaults to the main slot
```

A default slot does not need to be named, but if you define multiple slots, you must name the rest.

```Python
with t.card() as card:
    with card.slot("card-header"):
        t("Success!")
    with card.slot():
        t("Your operation worked")
```

<warning>
When consuming components with slots, to populate a slot, you <emphasis>do not call</emphasis> <code>t.slot</code>. You
call <code>.slot</code> directly on the component instance provided by the context manager.
</warning>