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
    - By default, escapes all entries in form submissions if displayed as jinja variables.
    
Templates are text files that contain placeholders for varaibles, using double curly braces {{ }}.
    - Logic can be added, including `if`, `for` loops etc.
    
### Jinja Template Inheritance
Jinja can inherit from another template using 'extends' to specify a parent template. Blocks can then be used to fill in parts of the parent with content specific to this page

```
{% extends "base.html" %}

{% block title %}Page Title!{% endblock %}
```
    
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

## Error Handling - e.g. Page Not Found
To return a 404, page not found error, use the `flask.abort()` method, e.g.

```
from flask import Flask, abort

from model import db


app = Flask(__name__)

@app.route("/card/<int:index>")
def card_view(index):
    try:
        card = db[index]
        return render_template("card.html", card=card)
    except IndexError:
        abort(404)
```


## Reverse URL building (url_for)
Inside a Jinja template, you can use the `url_for` method to return the url for a given view name. The first argument is the name of the view function, and any other arguments are arguments the view function accepts.

```
{{ url_for('view_function_name', arg1='Hello') }}
```

**'static'** is a view tha exists by default for the 'static' folder, and can be used for e.g. stylesheets, pictures. Using this means that if at some point the location for the items in the static folder changes, such as to a dedicated server, all the urls don't need to be changed - just the flask configuration for static needs changed.

```
<link rel="stylesheet" href="{{ url_for('static', filename='style.css' }}">
```


## Expressions in Templates
Expressions are written using {% %}

If statements require and {% endif %} statement, e.g.:

<button>
    {% if index < max_index %}
        <a href="{{ url_for('card_view', index=index + 1) }}">
            Next Card
        </a>
    {% else %}
        <a href="{{ url_for('card_view', index=0) }}">
            First Card
        </a>
    {% endif %}
</button>

### For Loops
Jinja for loops expose a variable `loop`. This variable can be used to find out which iteration you are in for the loop, 

for a 0-index count, use `loop.index0` e.g.:

```
<ul>
    {% for card in cards %}
        <li>
            <a href="{{ url_for('card_view', index=loop.index0) }}">
                {{ card.question }}
            </a>
        </li>
    {% endfor %}
</ul>
``` 

For a 1-indexed count, use `loop.index`


## REST API
Flask makes it very easy to create a rest api - simply return a json-serializable object, such as a dict. Best practice is to start the URL with "/api/"

Lists cannot be returned as they are, and instead you must use the `jsonify()` method. 

```
from flask import jsonify, Flask

app = Flask(__name__)

posts = [
    {
        "id": "0123",
        "message": "Web Dev is cool"
    }
]

@app.route("/api/all_posts/")
def api_all_posts():
    return jsonify(posts)

```

## Handling POST requests
View functions only handle get requests by default. To accept other types of requests, update the `@app.route` method and perform logic checks using `request.method`

```
from flask import request


@app.route("/add_card", methods=["GET", "POST"])
def add_card():
    if request.method == "POST":
        ...
```

request is a global object, however inside a view function it represents the current request from the browser - this includes headers, cookies form data etc.

The attribute `request.form` behaves like a dictionary, and can be accesseed like: `request.form['key']`

You need to return a template after handling a POST request, e.g.:

```
from flask import request, redirect, url_for


@app.route("/add_card", methods=["GET", "POST"])
def add_card():
    if request.method == "POST":
        ....
        return redirect(url_for('welcome'))
```

## Security
While jinja escape the html characters before displaying them, the user input will be saved in the database as is so it should be cleaned up before being saved. From protection from these kinds of 'attacks', use Flask-WTF
