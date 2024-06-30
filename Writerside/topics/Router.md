# Router

The PuePy router enables you to do client-side routing to multiple "pages", each with its own URL and optionally, URL
parameters.

## Use without the router

Use of routing is optional in PuePy. If you do not install a router, you can only define one page, and that page will
be mounted on the target element.

```Python
@app.page()
class DefaultPage(Page):
    ...

app.mount("#app")
```

Using PuePy without the router is ideal when you only have one page, or when you plan to use server-side routing.

## Enabling the router

Routing is an entirely optional feature of PuePy. Many projects may prefer to rely on backend-side routing, like a
traditional web project. For that reason, if you wish to use client-side routing, you must enable it and specify a
"mode" for it to operate.

```Python
from puepy import Application
from puepy.router import Router


app = Application()
app.install_router(Router)
app.link_mode = "hash"
```

| link_mode                  | description                                                                          | pro/con                                                     |
|----------------------------|--------------------------------------------------------------------------------------|-------------------------------------------------------------|
| `Router.LINK_MODE_HASH`    | Uses window location "anchor" (the part of the URL after a #) to determine route     | Simple implementation, works with any backend               |
| `Router.LINK_MODE_HTML5`   | Uses browser's history API to manipulate page URLs without causing a reload          | Cleaner URLs (but requires backend configuration)           |
| `Router.LINK_MODE_DIRECT`  | Keeps routing information on the client, but links directly to pages, causing reload | Might be ideal for niche use cases or server-side rendering |

<tip>
If you want to use client-side routing but aren't sure what <code>link_mode</code> to enable, "hash" is probably your
best bet.
</tip>

### Accessing the Router object

Once enabled, the Router object may be accessed on any component using `self.page.router`.

## Defining routes

The preferred way of adding pages to the router is by calling the `@app.page` decorator on your Application object 
(see [Hello World](Hello-World.md) in the tutorial or the [Application Developer Guide](Application.md)).

```Python
from puepy import Page


@app.page("/my-page")
class MyPage(Page):
    ...
```

Or, define a default route by not passing a path.

```Python
@app.page()
class MyPage(Page):
    ...
```

You can also add routes directly, though this isn't the preferred method.

```Python
app.router.add_route(path_match="/foobar", page_class=Foobar)
```

### Route names

By default, routes are named by converting the class name to lower case, and replacing `MixedCase` with `under_scores`.
For instance, `MyPage` would be converted to `my_page` as a route name. You may, however, give your routes custom names
by passing a `name` parameter to either `add_route` or `@app.page`:

```Python
@app.page("/my-page", name="another_name")
class MyPage(Page):
    ...


class AnotherPage(Page):
    ...
    
    
app.add_route("/foobar", AnotherPage, name="foobar")
```

### Passing parameters to pages

Pages can accept parameters fropm the router with props matching placeholders defined in the route path.

```Python
@app.page("/post/<author_id>/<post_id>/view")
class PostPage(Page):
    props = ["author_id", "post_id"]
    
    def populate(self):
        t.p(f"This is a post from {self.author_id}->{self.user_id}")
```

## Reversing routes

Call `router.reverse` with the page you want to find the route for, along with any relevant arguments.

```Python
path = self.page.router.reverse(PostPage, author_id="5", post_id="7")
```

The page can either be the Page object itself, or the route name.

```Python
path = self.page.router.reverse("post_page", author_id="5", post_id="7")
```
