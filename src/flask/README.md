# Flask Implementation

This folder contains the implementation of the Flask framework.

## Sub-Packages

* **sansio:** Contains code usable by alternative Flask implementations.
* **json:** Contains the Flask JSON implementation.

## Usage

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True)
```