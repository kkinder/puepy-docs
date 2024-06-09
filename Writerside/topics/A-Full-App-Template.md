# A Full App Template

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/tutorial/10_full_app/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (tutorial/10_full_app)</p>
</tldr>

Finally, we'll tie everything we've learned together by building a "full app" in CPython.

## Multiple files

For our larger example, we'll have several files:

- main.py: The Python file started from our `<script>` tag
- common.py: A place to put objects common to other files
- components.py: A place to put reusable components
- pages.py: A place to put individual pages we navigate to

## Adding a JavaScript library

To include chart.js in our project, we add `js_modules` to our PyScript config file with a pointer to the chart.js CDN:

```text
  "js_modules": {
    "main": {
      "https://cdn.jsdelivr.net/npm/chart.js": "chart"
    }
  }
```

<tip>
See <a href="https://docs.pyscript.net/2024.5.2/user-guide/configuration/#javascript-modules">JavaScript Modules</a> in
PyScript's documentation for additional information on loading JavaScript libraries into your project.
</tip>

We use charts.js directly from Python, in `components.py`:

```Python
@t.component()
class Chart(Component):
    props = ["type", "data", "options"]
    enclosing_tag = "canvas"

    def post_render(self, element):
        import js

        js.Chart.new(
            element,
            jsobj(type=self.type, data=self.data, options=self.options),
        )
```

## Niceties 

### A common app layout

In `components.py`, we define a common application layout, then reuse it in multiple pages:

```Python

@dataclass
class SidebarItem:
    label: str
    icon: str
    route: Route


@t.component()
class AppLayout(Component):
    sidebar_items = [
        SidebarItem("Dashboard", "emoji-sunglasses", "welcome_page"),
        SidebarItem("Charts", "graph-up", "charts_page"),
        SidebarItem("Forms", "input-cursor-text", "forms_page"),
    ]

    def populate(self):
        with t.sl_drawer(label="Menu", placement="start", classes="drawer-placement-start", ref="drawer"):
            self.populate_sidebar()

        with t.div(classes="container"):
            with t.div(classes="header"):
                with t.div():
                    with t.sl_button(classes="menu-btn", on_click=self.show_drawer):
                        t.sl_icon(name="list")
                t.div("The Dapper App")
                self.populate_topright()
            with t.div(classes="sidebar", id="sidebar"):
                self.populate_sidebar()
            with t.div(classes="main"):
                self.insert_slot()
            with t.div(classes="footer"):
                t("Business Time!")

    def populate_topright(self):
        with t.div(classes="dropdown-hoist"):
            with t.sl_dropdown(hoist=""):
                t.sl_icon_button(slot="trigger", label="User Settings", name="person-gear")
                with t.sl_menu(on_sl_select=self.on_menu_select):
                    t.sl_menu_item(
                        "Profile",
                        t.sl_icon(slot="suffix", name="person-badge"),
                        value="profile",
                    )
                    t.sl_menu_item("Settings", t.sl_icon(slot="suffix", name="gear"), value="settings")
                    t.sl_divider()
                    t.sl_menu_item("Logout", t.sl_icon(slot="suffix", name="box-arrow-right"), value="logout")

    def on_menu_select(self, event):
        print("You selected", event.detail.item.value)

    def populate_sidebar(self):
        for item in self.sidebar_items:
            with t.div():
                with t.sl_button(
                    item.label,
                    variant="text",
                    classes="sidebar-button",
                    href=self.page.router.reverse(item.route),
                ):
                    if item.icon:
                        t.sl_icon(name=item.icon, slot="prefix")

    def show_drawer(self, event):
        self.refs["drawer"].element.show()
```


### Loading indicator

Since CPython takes a while to load on slower connections, we'll populate the `<div id="app>` element with a loading 
indicator, which will be replaced once the application mounts:

```html
<div id="app">
  <div style="text-align: center; height: 100vh; display: flex; justify-content: center; align-items: center;">
    <sl-spinner style="font-size: 50px; --track-width: 10px;"></sl-spinner>
  </div>
</div>
```

