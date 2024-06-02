# Refs

<tldr>
    <p>The Problem: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/04_refs_problem/index.html">Broken Demo</a></p>
    <p>The Solution: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/04_refs_problem/solution.html">Fixed Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (04_refs_problem)</p>
</tldr>

## Problems with continuity

When PuePy redraws the DOM, new elements may be created and elements that were created in the last refresh, but not the
current one, will be discarded and eventually garbage collected. This lack of continuity between refreshes can cause
unintended problems.

```Python
from puepy import Application, Page, t

app = Application()


@app.page()
class RefsProblemPage(Page):
    def initial(self):
        return {"word": ""}

    def populate(self):
        t.h1("Problem: DOM elements are re-created")
        if self.state["word"]:
            for char in self.state["word"]:
                t.span(char, classes="char-box")
        with t.div(style="margin-top: 1em"):
            t.input(bind="word", placeholder="Type a word")
        t.hr()
        t.p("Notice that as you type, each time the page redraws, your input loses focus.")
        t.a("See Solution Here", href="./solution.html")


app.mount("#app")
```

As you can tell in a demo of
the [the problem](https://kkinder.pyscriptapps.com/puepy-tutorial/latest/04_refs_problem/index.html), as you type
elements above the `<input>` element redraw and the text field loses focus, which causes a jarring user experience.

Because PuePy doesn't know which elements are supposed to "match" the ones from the previous refresh, and the ordering
is now different, the original `<input>` is being discarded each refresh and replaced with a new one. That causes a
poor user experience.

### Using refs to preserve elements between refreshes

To tell PuePy not to garbage collect an element, but to reuse it between redraws, just give it a `ref=` parameter. The
ref should be unique to the component you're coding: that is, each ref should be unique among all elements created in 
the `populate()` method you're writing.

When PuePy finds an element with a ref, it will reuse that ref if it existed in the last refresh, modifying it with any
updated parameters passed to it. [Try out this version](https://kkinder.pyscriptapps.com/puepy-tutorial/latest/04_refs_problem/solution.html):

```Python
from puepy import Application, Page, t

app = Application()


@app.page()
class RefsSolutionPage(Page):
    def initial(self):
        return {"word": ""}

    def populate(self):
        t.h1("Solution: Use ref=")
        if self.state["word"]:
            for char in self.state["word"]:
                t.span(char, classes="char-box")
        with t.div(style="margin-top: 1em"):
            t.input(bind="word", placeholder="Type a word", ref="enter_word")
        t.hr()
        t.p(
            'Solution: by setting ref="enter_word" on the input box, we tell PuePy to identify that component by its ref.'
        )
        t.a("See Problem", href="./index.html")


app.mount("#app")
```

The operative change simply tells PuePy to keep the t.input object between refreshes based on its ref:

```Python
t.input(... ref="enter_word")
```

