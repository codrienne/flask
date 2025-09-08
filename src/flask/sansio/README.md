# Sansio (I/O-Agnostic Components)

This directory contains core components of Flask that are designed to be independent of I/O operations and Flask's global context. This allows them to be used by alternative WSGI/ASGI frameworks, such as Quart, enabling code reuse and a consistent API across different implementations.

The primary goal is to provide a clean, testable interface for request/response handling, routing, and other fundamental web application logic without direct reliance on the underlying server or framework specifics.

## Key Features

*   **I/O Independence**: Components do not perform direct file, network, or other I/O operations.
*   **No Global Context**: Avoids using Flask's `request`, `session`, `g`, etc., directly.
*   **Testability**: Enables easier unit testing of core logic without needing a running server.
*   **Framework Agnosticism**: Designed for use in various Python web frameworks.

## Example Usage

Here's a conceptual example demonstrating how a `sansio` component might be used. This example assumes you have a `sansio` object representing a request and you are using it to construct a response.

```python
# Assume these are hypothetical sansio components
from flask.sansio.scaffold import Scaffold
from flask.sansio.app import App

# --- Conceptual Example ---

# In a real scenario, these would be provided by the framework (e.g., Quart)
# or constructed for testing.

# Represents an incoming request in a framework-agnostic way
class MockSansioRequest:
    def __init__(self, method, path, headers=None, data=None):
        self.method = method
        self.path = path
        self.headers = headers or {}
        self.data = data

    def get_json(self):
        # Simplified: In reality, this would parse self.data
        return {"message": "hello"}

# Represents an outgoing response in a framework-agnostic way
class MockSansioResponse:
    def __init__(self, status_code, headers=None, response=None):
        self.status_code = status_code
        self.headers = headers or {}
        self.response = response

    def get_data(self):
        return self.response

# A simplified sansio-like application structure
class MySansioApp(App):
    def __init__(self):
        super().__init__()
        self.add_url_rule('/', endpoint='index', view_func=self.index)
        self.add_url_rule('/greet', endpoint='greet', view_func=self.greet)

    def index(self):
        # This function would typically return a sansio response object
        return MockSansioResponse(200, response="Welcome!")

    def greet(self, request):
        # Accessing request data in a sansio-compatible way
        data = request.get_json()
        name = data.get("name", "World")
        return MockSansioResponse(200, response=f"Hello, {name}!")

# --- Simulation ---

# Instantiate the sansio-like app
app = MySansioApp()

# Simulate a request to the index endpoint
request_index = MockSansioRequest("GET", "/")
response_index = app.wsgi_app(request_index) # Assuming wsgi_app is adapted for sansio

print(f"Index Response Status: {response_index.status_code}")
print(f"Index Response Body: {response_index.get_data()}")

# Simulate a request to the greet endpoint with JSON data
request_greet = MockSansioRequest("POST", "/greet", data={"name": "Gemini"})
response_greet = app.wsgi_app(request_greet)

print(f"Greet Response Status: {response_greet.status_code}")
print(f"Greet Response Body: {response_greet.get_data()}")

```

**Note**: The example above is illustrative. The actual `flask.sansio` components are more complex and integrated into Flask's internal workings. This example aims to show the *principle* of I/O and global independence.
