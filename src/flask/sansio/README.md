# Sansio

This directory contains the core, I/O-agnostic logic of Flask, designed for maximum reusability across different web frameworks or implementations (like Quart). These components are decoupled from direct request/response handling and global context, allowing them to be integrated into various asynchronous or synchronous environments.

## Key Components

*   `app.py`: Provides the core application class (`App`) that manages configuration, routing, and extensions in an I/O-free manner.
*   `scaffold.py`: Defines the `Scaffold` class, a base for applications and blueprints, handling features like URL routing and static file serving configuration.
*   `blueprints.py`: Contains the `Blueprint` class for organizing application components, similar to Flask's blueprints but without I/O dependencies.

## Examples

### 1. Using `sansio.app.App`

Instantiate and configure the sans-io application.

```python
from flask.sansio.app import App

# Create a sans-io app instance
app = App(__name__)

# Configure the app
app.config.update({
    'SECRET_KEY': 'a-secret-key',
    'TESTING': True,
})

@app.route('/')
def index():
    return 'Hello from Sansio App!'

# Note: This app instance itself does not handle requests.
# It needs to be integrated with an I/O layer (e.g., Werkzeug, Quart).
```

### 2. Using `sansio.scaffold.Scaffold`

Create a basic scaffold that can be extended.

```python
from flask.sansio.scaffold import Scaffold

class MyScaffold(Scaffold):
    def __init__(self, name=None, static_folder=None, static_url_path=None, template_folder=None, **kwargs):
        super().__init__(name, static_folder, static_url_path, template_folder, **kwargs)
        # Custom initialization for MyScaffold

# Instantiate the scaffold
my_scaffold = MyScaffold(__name__)
my_scaffold.add_url_rule('/', endpoint='index', view_func=lambda: 'Hello from Sansio Scaffold!')

# This scaffold can be used as a base for an App or Blueprint.
```

### 3. Using `sansio.blueprints.Blueprint`

Define a blueprint in a sans-io manner.

```python
from flask.sansio.blueprints import Blueprint

# Create a sans-io blueprint
my_blueprint = Blueprint('my_module', __name__, url_prefix='/my')

@my_blueprint.route('/')
def my_route():
    return 'Hello from Sansio Blueprint!'

# This blueprint can be registered with a sans-io App or a compatible framework.
```