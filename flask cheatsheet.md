# Flask

## Model-Template-View (Model-View-Controller) Pattern
This is a set of best practices for how to organise your code.

### Views
Defines the behaviour of the page, and determines how to send the appropriate content to the client.

*View Function* - Takes a Web request and returns a Web response. This is just a normal python function.

*URL Mapping* - This maps a function to a URL endpoint, using the `@app.route()` method.

```
from flask import Flask

app = Flask(__name__)

@app.route("/")
def homepage():
    return "Welcome to my website!"
```

### Templates
Components that display data to the user. Create a user-friendly presentation.

Jinja templates generate text files
    - We use it to create HTML files
    
Templates are text files that contain placeholders for varaibles, using double curly braces {{ }}.
    - Logic can be added, including `if`, `for` loops etc.
    
### A Jinja template example
<html>
    <head>
        <title>{{ name }}'s Page</title>
    </head>
    
    <body>
        <h1>Hi, I'm {{ name }}!</h1>
        I'm {{ age }} years old.
    </body>
</html>

### Calling a template from a view
To call a template from within a view, use the `flask.render_template()` function.

```
from flask import Flask, render_template

app = Flask(__app__)

@app.route("/")
def welcome():
    return render_template(
        "welcome.html",  # name of the file in the "templates" folder
        name="Liam",  # Other arguments are passed to the template context
        age=35
    )
```

## Model
Represent the data that lives in the application.


## Process
Browser send HTTP GET request

Flask will look up the URL in the URL map, and find the mapped view function.

The view function determines the response, and interacts with the model and the templates, e.g.
1. Call the database to get the appropriate data
2. Send that data to the template
3. Return the rendered template
4. The browser shows the page to the user.
