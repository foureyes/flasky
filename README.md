# Tech @ NYU - Basic Flask

This talk will essentially cover material sourced from the Flask Quickstart Documentation:

[http://flask.pocoo.org/docs/0.12/quickstart/#quickstart](http://flask.pocoo.org/docs/0.12/quickstart/#quickstart)

## Required

* [glitch.com](glitch.com) or [repl.it][repli.it]

OR

* [Python](https://www.python.org/) (3 preferred)
* [Flask](http://flask.pocoo.org/) 0.12.x
* and, optionally...
	* [virtualenv](https://virtualenv.pypa.io/en/stable/) - for maintaining different Python environments
	* [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) - tools to simplify virtualenv usage
* installation
	* use brew / download binary
	* `pip install flask`

## glitch.com

Bootstrap your application with these files

### `requirements.txt`

```
Flask
```

### `server.py`

```
from flask import Flask
import os
app = Flask(__name__)

@app.route('/')
def index():
  return 'hello'

app.run()
```

### `glitch.json`

```
{
  "install": "pip3 install --user -r requirements.txt",
  "start": "python3 server.py",
  "watch": {
    "restart": {
      "include": [
        "\\.py$",
        "^start\\.sh"
      ]
    }
  }
}
```

## Starting Out

Bringing in flask to a project

```
from flask import Flask
```

Create a new flask application:

```
app = Flask(__name__)
```

Have our application (place in a py file, like `app.py`)

```
@app.route('/hello')
def hello():
    return 'hello' 
```

Running your application

```
app.run()
```

And then...

```
python3 app.py
```

Alternatively...

* don't add app.run
* and use this to run instead
	```
	export FLASK_APP=filename
	flask run
	```
* and for debug mode
	```
	FLASK_DEBUG=1 FLASK_APP=filename flask run
	```

## Features

### Routing

* `@app.route('path')`
* `return "response text"`

Challenge

* create two pages that link to each other

### Templating and Templating With Variables

1. configuration
2. `from flask import render_template`
3. create directory called templates
4. `return render_template('template_name.html')`
5. template variables:
    * `return render_template('template_name.html', var_name=val)`
    * in templates:
        * expressions - `{{ expression }}` 
        * comments - `{{# comment #}}`
        * statement - `{% statement %}`
        * `{{ var_name }}`

Challenge

* create a page that rolls two six sided dice and displays both values!

### More Templating - Control Structures

for loop

```
{% for loop_var in list_of_objects %}
    <div>{{ loop_var.prop1 }} - {{ loop_var.prop2 }}</div>
{% endfor %}
```

conditional

```
{% if var_name %}
    <em>{{ var_name }}</em>
{% else %}
    <div>NOOOPE</div>
{% endif %}
```
nesting is possible

```
{% if people %}

    {% for p in people %}
        <p>{{p.first}} - {{p.last}}</p>
    {% endfor %}

{% else %}
    nope, not here
{% endif %}
```
* in layout.html: `{% block body %}{% endblock %}`
* in index.html: 

```
{% extends "layout.html" %}
{% block body %}
    stuff...
{% endblock %}
```

## Forms

`request` object that's imported represents the current http request

* `from flask import request`
* `@app.route('/login', methods=['GET', 'POST'])`
* `   if request.method == 'POST':`
* `request.form["name of form element"]`

```
<form method="POST" action="">
    <input type="text">	
    <input type="submit">	
</form>
```

Challenge

* create a number guessing game

## Storing Data

```
@app.route('/cats/')
def cats():
    return render_template('cats.html', names=names)

@app.route('/cat/create', methods=['POST'])
def cat_create():
    names.append(request.form['name'])
    return redirect(url_for('cats'))
```

## Routing Revisited

Trailing Slashes?

* `@app.route('/path/')`
    * `/path` will redirect to `/path/`
* `@app.route('/path')`
    * `/path/` will give error


Parameters / Variables in Paths

* add parameter / variable to path with angle brackets:
    * `@app.route('/foo/<variable_name>')`
* add same parameters to route handler
    * `def foo(variable_name):`
* can convert to type
    * `@app.route('/foo/<type_to_convert_to:variable_name>')`
    * string, int, float, path

```
@app.route('/number/<int:n>')
def number(n):
    return str(n + 2)
```

Challenge

* hacker news clone

URL Building

* `from flask import Flask, url_for`
* `url_for('function_name')`
* `url_for('function_name', path_variable="value")`

## Flash Messages

[Documentation](http://flask.pocoo.org/docs/0.12/patterns/flashing/#message-flashing-pattern)

* enable sessions: `app.secret_key = 'some_secret'` 
* add a message: `flash()`
* in expression in template: `get_flashed_messages()`

## JSON

* `from flask import jsonify`
* return jsonify(var_name)
