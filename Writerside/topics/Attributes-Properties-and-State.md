# Attributes, Properties, and State

## Attributes

*Tags* in PuePy (eg, `t.h1`) only have attributes. Anything you pass to a tag becomes a DOM attribute. This is true
for both standard HTML attributes and non-standard ones. For example:

```Python
class MyPage(Page):
    def populate(self):
        t.h1("My H1", style="color: blue", data_spam="eggs")
```

The `h1`'s `style` will automatically be passed to the DOM, along with `data-spam`:

```html
<h1 style="color: blue" data-spam="eggs">My H1</h1>
```

The same is true of components. Any parameters you pass to the component become part of the component's main enclosing
tag.

## Props

Props (short for properties) are variables passed to a component which are for use by the Python code in the component
and *not* rendered directly into the DOM. For example:

```Python
@t.component()
class ShowUser(Component):
    props = ["user"]

    def populate(self):
        t.h1(f"User: {self.user.name}")
        t.p(f"Email: {self.user.email}")
```

In this example, the component can be passed a *user* argument which is accessible as `self.user`, but *not* made an
HTML attribute. Props are useful for passing information from parent components to child components.

You can define Props either as a list of strings, or as a list of `Prop` instances, which add more metadata.

```Python
from puepy import Prop


@t.component
class ShowUser(Component):
    props = [
        Prop("user",
             description="Currently logged in user",
             type=User,
             default_value=None)
    ] 
```

## State

State is the core of reactivity in PuePy. Each Page and Component has a `self.state` *reactive* dictionary. Each time a
key in the state is set, the relevant page/component is notified and a refresh of the UI is triggered, if necessary.
See [Reactivity](Reactivity.md) for more detail on how to manage state.
