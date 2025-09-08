# Sansio (I/O-Free Core)

This directory contains the core, I/O-free components of Flask. The primary goal of this separation is to enable alternative implementations of Flask, such as Quart, to reuse Flask's fundamental logic without being tied to Flask's specific I/O handling or global context.

## Purpose

-   **Decoupling:** Separates the core application logic (routing, request/response objects, context management) from the underlying web server and I/O operations.
-   **Reusability:** Allows other WSGI/ASGI frameworks to leverage Flask's well-tested core components.
-   **Testability:** Functions within `sansio` are inherently easier to test as they do not depend on external I/O or global state, making them pure functions or classes.
-   **Abstraction:** Provides a clean interface for request and response handling that can be adapted to various asynchronous or synchronous environments.

## Key Characteristics of Sansio Code:

-   **No I/O Operations:** Functions and classes here do not perform any network I/O, file system operations, or other blocking calls.
-   **No Flask Globals:** Code does not rely on Flask's global objects like `current_app`, `request`, or `session`. Instead, these are passed explicitly as arguments or managed through context objects.
-   **Pure Functions/Classes:** Aims for functional programming principles where possible, making behavior predictable and testable.

## Example Usage Patterns:

While direct executable examples are complex without the surrounding framework, here's how you might conceptualize using `sansio` components:

Imagine a `sansio.app.Flask` class that takes a WSGI or ASGI application factory.

```python
# Conceptual example (not runnable code without full sansio context)
from flask.sansio.app import Flask as SansioFlask
from flask.sansio.request import Request as SansioRequest
from flask.sansio.response import Response as SansioResponse

# A simple sansio application factory
def create_sansio_app():
    app = SansioFlask(__name__) # __name__ here is just an identifier, not a module lookup

    @app.route('/')
    def homepage(request: SansioRequest):
        # Logic here operates on the SansioRequest object
        # and returns a SansioResponse object.
        return SansioResponse("Hello from Sansio!", status=200)

    return app

# This SansioFlask instance would then be wrapped by a WSGI/ASGI server
# For example, a hypothetical `run_sansio_app` function:
# run_sansio_app(create_sansio_app())
```

This structure allows a server (like Gunicorn for WSGI or Uvicorn for ASGI) to provide the actual `request` object and handle the `response` object, while the `sansio` code defines the application's routing and logic.
