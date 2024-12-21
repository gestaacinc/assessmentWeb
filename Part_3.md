
# Setting Up Flask

This guide will walk you through initializing your Flask app, creating routes, and running the Flask development server.

## Step 1: Initialize the Flask App

1. **Locate the `__init__.py` File**  
   Inside the `app` directory, find the `__init__.py` file. This is where we will initialize the Flask application.

2. **Open `__init__.py` in Your Code Editor**  
   Add the following code to initialize the Flask app:

   ```python
   from flask import Flask

   app = Flask(__name__)

   # If you have additional configurations, you can set them here
   # For example, to enable debug mode:
   # app.config['DEBUG'] = True

   from app import routes
   ```

---

## Step 2: Create Routes

1. **Locate the `routes.py` File**  
   The `routes.py` file is where you define the URLs that your application will respond to.

2. **Open `routes.py` in Your Code Editor**  
   Add a simple route for the home page:

   ```python
   from app import app
   from flask import render_template

   @app.route('/')
   def index():
       return render_template('index.html')
   ```

   This code defines a route for the root URL (`/`). When this URL is visited, the `index()` function will render and return the `index.html` template.

---

## Step 3: Start the Flask Development Server

1. **Locate the `run.py` File**  
   Open the `run.py` file at the root of your project.

2. **Add the Following Code to `run.py`**  

   ```python
   from app import app

   if __name__ == '__main__':
       app.run(debug=True)
   ```

3. **Save the File**  

---

## Step 4: Start the Server

1. **Open Command Prompt or Terminal**  
   Navigate to the root of your project directory.

2. . **Run the Flask App**  

   ```bash
   python run.py
   ```

4. **Access the Application in Your Browser**  
   Open your web browser and navigate to:  
   [http://127.0.0.1:5000/](http://127.0.0.1:5000/)

   You should see your `index.html` page rendered successfully.

---

### Notes:
- Ensure the `index.html` file is in the `templates` folder of your project.
- The `debug=True` option allows for live reloading and better error messages during development. Disable this in production.
