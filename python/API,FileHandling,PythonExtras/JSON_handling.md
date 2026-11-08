# Reading and Writing JSON

# What is JSON?

**JSON** (**J**ava**S**cript **O**bject **N**otation) is a lightweight text format for storing and exchanging data. It’s:

- Human-readable
- Language-independent
- Widely used in APIs, configuration files, and data interchange

JSON maps naturally to Python types:

|**JSON**|**Python**|
| :---: | :---: |
|`object`| `dict` |
| `array` | `list` |
| `string` | `str`|
| `number` | `int` / `float`|
| `true`/`false`|`True` / `False`|
| `null` | `None`|

Python’s built-in `json` module handles converting between JSON and Python objects.

# Why this matters

JSON shows up everywhere:

- Talking to web APIs
- Saving settings and configuration
- Exchanging data between services
- Storing lightweight structured data on disk

Knowing how to read and write JSON lets your Python programs integrate with the rest of the world.

We are focusomg on file-based JSON: loading JSON from files and saving Python objects back to disk.

# The `json` module at a glance

Import the module:

```python
import json
```
Core functions:

- `json.load(file)` — read JSON from a **file object** → Python object
- `json.dump(obj, file, ...)` — write Python object **to a file object** as JSON
- `json.loads(string)` — read JSON **from a string** → Python object (useful with APIs)
- `json.dumps(obj, ...)` — convert Python object **to a JSON string**

For file handling, you’ll mostly use `load` and `dump` together with `with open(...)`.

# Reading JSON from a file

## Basic example

Suppose `config.json` contains:

```json
{
  "debug": true,
  "host": "localhost",
  "port": 8000
}
```
Read it into Python:

```python
import json

with open("config.json", "r", encoding="utf-8") as f:
    config = json.load(f)

print(config["debug"])  # True
print(config["host"])   # 'localhost'
print(config["port"])   # 8000
```
Behind the scenes:

- `open("config.json", "r", encoding="utf-8")` opens the file in text mode
- `json.load(f)` parses the JSON text into Python objects (here, a dict)

## Reading lists of objects
```json
// example JSON
[
  {"id": 1, "name": "Alice"},
  {"id": 2, "name": "Bob"}
]
```

```python
import json

with open("users.json", "r", encoding="utf-8") as f:
    users = json.load(f)

for user in users:
    print(user["id"], user["name"])
```
# Writing JSON to a file

## Basic example

```python
import json

data = {
    "debug": False,
    "host": "0.0.0.0",
    "port": 8080,
}

with open("config.json", "w", encoding="utf-8") as f:
    json.dump(data, f)
```
This will produce compact JSON like:

```json
{"debug": false, "host": "0.0.0.0", "port": 8080}
```

## Pretty-printing JSON

Use `indent` to make JSON easier to read:

```python
with open("config_pretty.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=2)
```
Output:

```json
{
  "debug": false,
  "host": "0.0.0.0",
  "port": 8080
}
```
You can also control key order with `sort_keys=True`:

```python
with open("config_sorted.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=2, sort_keys=True)
```

# Controlling encoding and newlines

JSON is text, typically encoded as **UTF-8**.

```python
with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False)
```
- `encoding="utf-8"` ensures proper Unicode handling.
- `ensure_ascii=False` lets non-ASCII characters (like accents or emojis) be written directly instead of as escape sequences.

Example:

```python
data = {"name": "José", "emoji": "🐍"}

with open("user.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)
```
# Using `json.loads` and `json.dumps`
Sometimes your JSON isn’t in a file (e.g., an HTTP response body). You can still use the same module:
```python
import json

json_text = '{"active": true, "roles": ["admin", "editor"]}'

data = json.loads(json_text)
print(data["active"])  # True

back_to_text = json.dumps(data)
print(back_to_text)    # '{"active": true, "roles": ["admin", "editor"]}'
```
You can combine this with file reading/writing yourself if you want more control:

```python
with open("data.json", "r", encoding="utf-8") as f:
    text = f.read()
data = json.loads(text)
```
…but in most cases `json.load` / `json.dump` are simpler.

# Handling errors when reading JSON

## Invalid JSON

If the file is not valid JSON, `json.load` will raise `json.JSONDecodeError`:

```python
import json

try:
    with open("broken.json", "r", encoding="utf-8") as f:
        data = json.load(f)
except json.JSONDecodeError as e:
    print("Invalid JSON:", e)
```
## Missing file
Combine with `FileNotFoundError` handling from the basics guide:

```python
import json

try:
    with open("settings.json", "r", encoding="utf-8") as f:
        settings = json.load(f)
except FileNotFoundError:
    print("settings.json not found, using defaults")
    settings = {}
```
# Custom types and `default`

JSON only supports basic types. If you try to dump a custom object, you’ll get a `TypeError`:

```python
import json
from datetime import datetime

data = {"time": datetime.now()} #datetime.now() returns a datetime obj

json.dumps(data)  # TypeError: Object of type datetime is not JSON serializable
```
Use the `default` parameter to convert unsupported objects:

```python
import json
from datetime import datetime

data = {"time": datetime.now()}

def default_converter(obj):
    if isinstance(obj, datetime):
        return obj.isoformat()
    raise TypeError(f"Type {type(obj)} not serializable")

json_text = json.dumps(data, default=default_converter, indent=2)
print(json_text)
# Output:
# {
#   "time": "2024-01-15T14:30:45.123456"
# }
```
The `default` function is called for any object that `json.dumps()` can't serialize. Here, it converts `datetime` objects to ISO format strings, which are JSON-compatible. (you can also use the date tiem `strftime()` method to convert the dateTime OBJ to a string)

To read the data back and convert the ISO string back to a `datetime` object:

```python
# Load the JSON
loaded_data = json.loads(json_text)
print(loaded_data)  # {'time': '2024-01-15T14:30:45.123456'}

# Convert the ISO string back to datetime
loaded_data['time'] = datetime.fromisoformat(loaded_data['time'])
print(loaded_data['time'])  # 2024-01-15 14:30:45.123456
print(type(loaded_data['time']))  # <class 'datetime.datetime'>
```
You can also create a helper function to automatically convert datetime strings when loading:

```python
def load_with_datetime(json_str):
    """Load JSON and convert ISO datetime strings back to datetime objects."""
    data = json.loads(json_str)
    for key, value in data.items():
        if isinstance(value, str):
            try:
                data[key] = datetime.fromisoformat(value)
            except (ValueError, AttributeError):
                pass  # Not a datetime string, leave as-is
    return data

loaded_data = load_with_datetime(json_text)
print(loaded_data['time'])  # 2024-01-15 14:30:45.123456
```
# Putting it all together: simple JSON config helper

```python
import json
from pathlib import Path

def load_json(path, default=None):
    """Load JSON from a file, or return default if it doesn't exist."""
    path = Path(path)
    if not path.exists():
        return {} if default is None else default
    with path.open("r", encoding="utf-8") as f:
        return json.load(f)

def save_json(path, data):
    """Save JSON to a file with pretty formatting."""
    path = Path(path)
    with path.open("w", encoding="utf-8") as f:
        json.dump(data, f, indent=2, ensure_ascii=False)

settings = load_json("settings.json", default={"theme": "dark"})
settings["theme"] = "light"
save_json("settings.json", settings)
```
# summery

- Use the `json` module (`load`, `dump`, `loads`, `dumps`)
- Read JSON from files into Python dictionaries and lists
- Write Python objects back to disk as JSON
- Pretty-print and control encoding with `indent`, `sort_keys`, and `ensure_ascii`
- Handle common errors like invalid JSON and missing files
- Start dealing with custom types using the default argument


















