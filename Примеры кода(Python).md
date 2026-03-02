Код - [[Python]]


# Генератор

```python
def test():
   a = 0
   while True:
       yield a
       a += 1
```

---

# Пример ТУИ (нейронка)

```python
from textual.app import App, ComposeResult
from textual.widgets import Header, Footer, Button, Static

class TextualApp(App):
    CSS = """
    Screen {
        layout: vertical;
        align: center middle;
    }
    Button {
        margin: 1;
    }
    """

    def compose(self) -> ComposeResult:
        yield Header("TUI для Ратника")
        yield Static("Царь, ты лох, но я тебя люблю.", id="title")
        yield Button("Нажми меня, если ты герой", id="button1")
        yield Button("Закрыть приложение", id="button2")
        yield Footer()

    def on_button_pressed(self, event: Button.Pressed) -> None:
        if event.button.id == "button1":
            self.query_one("#title").update("Ты нажал на кнопку — ты легенда!")

        if event.button.id == "button2":
            self.exit("Закрыто по приказу царя.")  

if __name__ == "__main__":
    app = TextualApp()
    app.run()
```

--- 

# 