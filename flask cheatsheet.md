# Flask

## Views
*View Function* - Takes a Web request and returns a Web response. This is just a normal python function.

*URL Mapping* - This maps a function to a URL endpoint, using the `@app.route()` method.

```
from flask import Flask

app = Flask(__name__)

@app.route("/")
def homepage():
    return "Welcome to my website!"
```
