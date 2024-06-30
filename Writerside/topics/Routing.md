# Routing

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/tutorial/07_routing/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (tutorial/07_routing)</p>
</tldr>

## Pages and Routes

Define your site using multiple pages, each with its own parameters.

```Python
from puepy import Application, Page, Component, t
from puepy.router import Router

app = Application()
app.install_router(Router, link_mode=Router.LINK_MODE_HASH)

pets = {
    "scooby": {"name": "Scooby-Doo", "type": "dog", "character": "fearful"},
    "garfield": {"name": "Garfield", "type": "cat", "character": "lazy"},
    "snoopy": {"name": "Snoopy", "type": "dog", "character": "playful"},
}


@t.component()
class Link(Component):
    props = ["args"]
    enclosing_tag = "a"

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

        self.add_event_listener("click", self.on_click)

    def set_href(self, href):
        if issubclass(href, Page):
            args = self.args or {}
            self._resolved_href = self.page.router.reverse(href, **args)
        else:
            self._resolved_href = href

        self.attrs["href"] = self._resolved_href

    def on_click(self, event):
        if (
            isinstance(self._resolved_href, str)
            and self._resolved_href[0] in "#/"
            and self.page.router.navigate_to_path(self._resolved_href)
        ):
            # A page was found; prevent navigation and navigate to page
            event.preventDefault()


@app.page("/pet/<pet_id>")
class PetPage(Page):
    props = ["pet_id"]

    def populate(self):
        pet = pets.get(self.pet_id)
        t.h1("Pet Information")
        with t.dl():
            for k, v in pet.items():
                t.dt(k)
                t.dd(v)
        t.link("Back to Homepage", href=DefaultPage)


@app.page()
class DefaultPage(Page):
    def populate(self):
        t.h1("PuePy Routing Demo: Pet Listing")
        with t.ul():
            for pet_id, pet_details in pets.items():
                with t.li():
                    t.link(pet_details["name"], href=PetPage, args={"pet_id": pet_id})


app.mount("#app")

```

### Specifying Paths

Each page's path is set by the `app.page` decorator. Parameters can be parsed out of URLs and passed as arguments to
the class, ideally as props.

```Python
@app.page("/pet/<pet_id>")
class PetPage(Page):
    props = ["pet_id"]
```

### Default pages

The root page, which has no path, will render by default:

```Python
@app.page()
class DefaultPage(Page):
    ...
```

### Getting URLs for pages

Paths are resolved by calling `self.router.reverse`

```Python
self.page.router.reverse(href, **args)
```

## Further Reading

For more, see [the Router developer guide](Router.md).
