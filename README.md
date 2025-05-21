**ProjectLogger** – a universal logger for Python projects.
This project provides a simple logging system built on Python’s built-in `logging` module, extending its functionality.
### Features
- **Central Logger**: 
  A base logger named `root` is created, from which all others inherit—making log collection and filtering easier.
- **Color Formatting**:
  Customizable colors for different loggers to highlight various project components.
- **External Configuration File**:
  Store color schemes in a JSON file for easy management without touching the code.
- **File Output**:
  Automatically save logs from each run into a separate file.
### Quick Start
1. Copy the `project_logger` folder into the root of your project.
2. In `project_logger/__init__.py`, you can adjust the time zone, log level, and enable debug mode.
3. In your main script (e.g. `main.py`), initialize ProjectLogger:
   ```python
   from project_logger import PL
   
   logger = PL.start_logging()
   logging.info("IT WORKS!")
```
### Creating Additional Loggers
To organize logging by module or directory, add an `__init__.py` to the desired folder:
```python
# project/app/__init__.py
from project_logger import PL  

# create a logger for the 'app' directory with yellow color
logger = PL.new_logger("app", "yellow")
```
Then, anywhere inside `project/app`, you can write:
```python
from project.app import logger

logger.info("Message")
```
#### Child Loggers
To give each module its own child logger:
```python
from project_logger import PL  
  
logger = PL.new_logger("folder_name", "color")

def get_child(name, color=None) -> PL.logger:  
	"""Returns a child logger within the app"""
    return PL.new_child(logger, name, color)
```
Use `get_child('module_name', 'blue')` to obtain a logger for a subpackage.
***Recommendation:** keep all PL-related logic in each package’s `__init__.py` so that your code stays clean and consistent.*
### Loading Colors from a JSON File
Create a JSON file with this structure:
```json
{
  "root": "green",
  "project.app": "yellow",
  "project.app.module": "blue"
}
```
Then load it before initialization:
```python
from project_logger import PL
PL.set_colors_from_json('path/to/colors.json')
logger = PL.start_logging(
```
