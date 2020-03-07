## microtask-8

Set up Prosoul to be executed from PyCharm.

1. Download and install [PyCharm IDE](https://www.jetbrains.com/pycharm/).
2. Clone the [prosoul](https://github.com/Bitergia/prosoul) repository and open the project using PyCharm.
3. Setup a virtualenvironment for the project. `File >> Settings >> Project:django-prosoul >> Project Interpreter`
4. Set up a new interpreter with the following configuration
    - Location: `prosoul/django-prosoul/VENV_DIR`
    - Base interpreter: `/usr/bin/python3.6`
5. Save the configuration. Now, you can see the required modules in the Project Interpreter. Apply the changes.
6. Open the terminal in PyCharm and make sure the virtual environment `VENV_DIR` is running. Install the project requirements.
    ```
    $ pip3 install -r requirements.txt
    ```
    Once it is done, you can check the updated modules list in the Project Interpreter.
7. To start the django application
    ```
    $ python3 manage.py makemigrations`
    $ python3 manage.py migrate
    ```
8. Edit the `Run/Debug configuration` with the following configuration
    - Script path: `prosoul/django-prosoul/manage.py`
    - Parameters: `runserver`
9. Apply the changes and run the script. By default, the application will be accessible in: [http://127.0.0.1:8000/](http://127.0.0.1:8000/).
---

Here is a small demo video on YouTube

Quick Link >> [Setting up Prosoul with PyCharm](https://www.youtube.com/watch?v=-wU1ck4ZrUw&t=20s)

[![Setting up Prosoul](images/setting_up_prosoul.png "Setting up Prosoul on Youtube | Click to watch it")](http://www.youtube.com/watch?v=-wU1ck4ZrUw)
