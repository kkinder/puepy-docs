# Watchers

<tldr>
    <p>Try It: <a href="https://kkinder.pyscriptapps.com/puepy-tutorial/latest/tutorial/05_watchers/index.html">Working Demo</a></p>
    <p>Experiment: <a href="https://pyscript.com/@kkinder/puepy-tutorial/latest">PyScript</a> (tutorial/05_watchers)</p>
</tldr>

When state changes and you want to monitor specific variables, use `on_<variable>_change` methods in your component.

```Python
from puepy import Application, Page, t

app = Application()


@app.page()
class WatcherPage(Page):
    def initial(self):
        self.winner = 4

        return {"number": "", "message": ""}

    def populate(self):
        t.h1("Can you guess a number between 1 and 10?")

        with t.div(style="margin: 1em"):
            t.input(bind="number", placeholder="Enter a guess", autocomplete="off", type="number", maxlength=1)

        if self.state["message"]:
            t.p(self.state["message"])

    def on_number_change(self, event):
        try:
            if int(self.state["number"]) == self.winner:
                self.state["message"] = "You guessed the number!"
            else:
                self.state["message"] = "Keep trying..."
        except (ValueError, TypeError):
            self.state["message"] = ""


app.mount("#app")
```

Notice that there is no need to explicitly bind the method to the state change; its name alone causes it to execute.
You can also watch for all state changes by implementing on `on_state_change(self, key, value)`.

