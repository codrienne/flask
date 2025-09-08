# Flask: A Python Web Framework

Flask is a lightweight WSGI web application framework. It's designed to make getting started with web development quick and easy, with the ability to scale up to complex applications. This directory contains the core components of the Flask framework.

## Key Concepts

*   **`Flask` Application Object**: The central object of the Flask framework, defined in `app.py`. It handles routing, request and response handling, templating, and more.
*   **Routing**: Maps URLs to Python functions (view functions).
*   **Templates**: Uses Jinja2 for rendering dynamic HTML content.
*   **Request Context**: Provides access to request-specific data like form data, URL parameters, and session information.
*   **Blueprints**: A way to organize your Flask application into smaller, reusable components.

## Installation

Flask can be installed using pip:

```bash
pip install Flask
```

## Quick Start

Here's a simple "Hello, World!" example using Flask:

```python
# app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True)
```

To run this application:

1.  Save the code above as `app.py`.
2.  Open your terminal, navigate to the directory where you saved `app.py`.
3.  Run the development server:
    ```bash
    flask run
    ```
4.  Open your web browser and go to `http://127.0.0.1:5000/`. You should see "Hello, World!".

## More Examples

### Rendering a Template

First, create a `templates` directory in the same location as your `app.py`, and add an `index.html` file:

```html
<!-- templates/index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>
    <h1>Hello, {{ name }}!</h1>
</body>
</html>
```

Then, modify your `app.py` to render this template:

```python
# app.py
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/<name>')
def hello(name):
    return render_template('index.html', name=name)

if __name__ == '__main__':
    app.run(debug=True)
```

Now, if you go to `http://127.0.0.1:5000/Alice`, you will see "Hello, Alice!".

### Handling Form Data

```python
# app.py
from flask import Flask, request, render_template_string

app = Flask(__name__)

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        return f'Welcome, {username}!'
    return '''
        <form method="post">
            <p><input type=text name=username>
            <p><input type=submit value=Login>
        </form>
    '''

if __name__ == '__main__':
    app.run(debug=True)
```

Access `http://127.0.0.1:5000/login` to see the form and submit data.

## Further Reading

For more detailed information, refer to the official Flask documentation.
