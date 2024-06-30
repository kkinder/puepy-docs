# Component

The `puepy.Component` class is the most important class in PuePy. It defines components, which are reusable
encapsulations of data, display, and logic, reused throughout applications. The Page class is a subclass
of `Component`, so anything applying to components also applies to pages.

`Component` is a subclass of `puepy.Tag`, which all composed tags (`h1`, `p`, `strong`, etc) are instantiated from.

## Parent/Child relationships

Each tag (and thus each component) in PuePy has a parent unless it is the root page. Consider the following example:

```Python
from puepy import Application, Page, Component, t

app = Application()


@t.component()
class CustomInput(Component):
    enclosing_tag = "input"

    def on_event_handle(self, event):
        print(self.parent)


class MyPage(Page):
    def populate(self):
        with t.div():
            t.custom_input()
```

In this example, the *parent* of the `CustomInput` instance *is not* the `MyPage` instance, it is the `div`,
a `puepy.Tag` instance. In many cases, you will want to interact another relevant object, not necessarily the one
immediately parental of your current instance. In those instances, from your components, you may reference:

- `self.page` (Page instance): The page ultimately responsible for rendering this component
- `self.origin` (Component or Page instance): The component that created yours in its `populate()` method 
- `self.parent` (Tag instance): The direct parent of the current instance 

Additionally, parent instances have the following available:

- `self.children` (list): Direct child nodes
- `self.refs` (dict): Instances created during this instance's most recent `populate()` method

<warning>
In general, none of the attributes regarding parent/child/origin relationships should be modified by application code.
Doing so could result in unexpected behavior.
</warning>

## Refs

In addition to parent/child relationships, most components and pages define an entire hierarchy of tags and components
in the `populate()` method. If you want to reference components later, or tell PuePy which component is which (in case
the ordering changes in sebsequent redraws), using a `ref=` argument when building tags:

```Python
class MyPage(Page):
    def populate(self):
        t.button("My Button", ref="my_button")
    
    def auto_click_button(self, ...):
        self.refs["my_button"].element.click()
```

For more information on why this is useful, see the [Refs Tutorial Topic](Refs.md).

## Enclosing tags

All components by design must map directly to one *enclosing* tag, in addition to any additional elements rendered in
`populate()`. The enclosing tag can be defined as a class attribute. For example, in
[the full application example](A-Full-App-Template.md) from the tutorial, we define a custom component for charts:

```Python
@t.component()
class Chart(Component):
    ...
    
    enclosing_tag = "canvas"

    ...
```

That bit about `enclosing_tag = "canvas"` tells PuePy that the Chart component should be rendered in HTML as a 
`<canvas>` tag. If enclosing tag is not specified, `div` is the default.
