# Using Local Bootstrap Files in Flask

This guide will walk you through using local Bootstrap files for styling and interactivity in your Flask application.

---

## Step 1: Download and Extract Bootstrap Files

1. Download the Bootstrap files from this link:  
   [Bootstrap 5.3 - Getting Started](https://getbootstrap.com/docs/5.3/getting-started/download/).

2. Extract the downloaded `.zip` file.

3. Locate the files:
   - **`bootstrap.min.css`** inside the `css/` folder.
   - **`bootstrap.min.js`** inside the `js/` folder.

4. Place these files in your Flask `static` folder:
   - **`bootstrap.min.css`** → `static/css/`
   - **`bootstrap.min.js`** → `static/js/`

Your directory structure should look like this:

```plaintext
FarmersMarketApp/
├── app/
│   ├── static/
│   │   ├── css/
│   │   │   └── bootstrap.min.css
│   │   ├── js/
│   │   │   └── bootstrap.min.js
│   │   └── images/
│   │       └── [product images]
│   ├── templates/
│   │   ├── layout.html
│   │   ├── index.html
│   │   ├── product.html
│   │   └── order_confirmation.html
│   ├── __init__.py
│   ├── models.py
│   ├── routes.py
│   └── forms.py
├── venv/
├── requirements.txt
└── run.py
```

---

## Step 2: Create the Base Template (`layout.html`)

Edit your `layout.html` to include the local Bootstrap files and a navigation bar.

```html
<!doctype html>
<html lang="en">
<head>
  <!-- Required meta tags -->
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <!-- Local Bootstrap CSS -->
  <link rel="stylesheet" href="{{ url_for('static', filename='css/bootstrap.min.css') }}">

  <title>{% block title %}Palenke Market{% endblock %}</title>
</head>
<body>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Palenke Market</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="/">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/products">Products</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/contact">Contact</a>
        </li>
      </ul>
    </div>
  </div>
</nav>

<div class="container mt-4">
  {% block content %}
  <!-- Content overridden by child templates -->
  {% endblock %}
</div>

<!-- Local Bootstrap JS -->
<script src="{{ url_for('static', filename='js/bootstrap.min.js') }}"></script>

</body>
</html>
```

---

## Step 3: Extend the Base Template in `index.html`

Create an `index.html` file to extend the base layout and define its own content block:

```html
{% extends 'layout.html' %}

{% block title %}Home - Palenke Market{% endblock %}

{% block content %}
<h1>Welcome to Palenke Market!</h1>
<p>Your one-stop shop for fresh produce and quality goods.</p>

<div class="row">
  <div class="col-md-4">
    <div class="card">
      <img src="{{ url_for('static', filename='images/sample_product.jpg') }}" class="card-img-top" alt="Sample Product">
      <div class="card-body">
        <h5 class="card-title">Fresh Apples</h5>
        <p class="card-text">Crisp, delicious, and perfect for your daily snack.</p>
        <a href="/products" class="btn btn-primary">View Products</a>
      </div>
    </div>
  </div>
</div>
{% endblock %}
```

---

## Step 4: Define the Route for the Homepage

Ensure your `routes.py` file contains the route to render the `index.html` template:

```python
from flask import render_template
from app import app

@app.route('/')
def index():
    return render_template('index.html')
```

---

## Step 5: Run the Application

1. Open your terminal or command prompt.
2. Start the Flask server:

   ```bash
   python run.py
   ```

4. Open your browser and navigate to [http://127.0.0.1:5000/](http://127.0.0.1:5000/).  
   You should see the homepage styled with Bootstrap and a functional navigation bar.

---

### Notes:
- Ensure your images are placed in the `static/images` folder and referenced using `url_for`.
- Modify the navbar content or add new pages as needed by defining additional routes and templates.
