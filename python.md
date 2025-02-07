
### start a python project
```bash
# -m = run a python module as a script
# venv = the module for creating virtual environments
# venv (at end) = the name of the virtual environment directory
python -m venv venv

# activate the environment
source venv/bin/activate

# create requirements.txt, list packages in it like
gradio==5.15.0

# install packages
pip install -r requirements.txt

# write your code in app.py and execute it
python app.py

# deactivate currently active environment
deactivate
```

### date
```bash
import datetime
datetime.datetime.now()  # current time
```
