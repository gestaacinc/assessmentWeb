# Rendering Products on the Products Page in Flask

This guide outlines the steps to display products from a MySQL database on the products page in a Flask application.

---

## Prerequisites

1. A Flask application set up with **Flask-MySQLdb** for database access.
2. A MySQL database (`db_kiosk`) with a `product` table populated with data.

---

## Step 1: Create a Route for the Products Page

1. Open the `routes.py` file in your project.
2. Add a route `/products` to fetch product data from the database and render it using a template.

Example:

```python
# routes.py

from app import app, mysql
from flask import render_template

@app.route('/products')
def products():
    cursor = mysql.connection.cursor()
    cursor.execute("SELECT * FROM product")  # Replace 'product' with your actual table name
    product_data = cursor.fetchall()
    cursor.close()
    return render_template('product.html', products=product_data)
```

---

## Step 2: Design the Products Template

1. Create a `product.html` file inside the `templates` folder.
2. Use the following example to display the products with Bootstrap’s grid system:

```html
<!-- product.html -->

{% extends 'layout.html' %}

{% block title %}Product Page{% endblock %}

{% block content %}
<h1>Products</h1>
<div class="row">
    {% for product in products %}
    <div class="col-4">
        <div class="card mb-4">
            <img src="{{ url_for('static', filename=product[4]) }}" class="card-img-top img-fluid" alt="Product Image">
            <div class="card-body">
                <h5 class="card-title">{{ product[1] }}</h5>
                <p class="card-text">Price: ₱{{ product[2] }}</p>
                <p class="card-text">Unit: {{ product[3] }}</p>
            </div>
        </div>
    </div>
    {% endfor %}
</div>
{% endblock %}
```

---

## Step 3: Ensure Product Images Are Correctly Aligned

To correctly display product images:

1. Verify that all product image files are in the `app/static/images` folder.
2. The file names must exactly match the values in the `PhotoURL` column of the `product` table in `db_kiosk`.

### If Images Are Missing:
- **Option 1**: Add the correct image files to the `app/static/images` folder.
- **Option 2**: Update the `PhotoURL` column values in `phpMyAdmin` to match the existing file names.

---

## Step 4: Run and Test the Application

1. Start your Flask application:

   ```bash
   python run.py
   ```

2. Open a web browser and navigate to:  
   [http://127.0.0.1:5000/products](http://127.0.0.1:5000/products).

3. Confirm that:
   - Product data is displayed.
   - Images load correctly for each product.

---

## Example Database Configuration

Ensure the `PhotoURL` column in your `product` table matches the image file names in `app/static/images`. Here’s an example:

| Product Name         | PhotoURL              |
|----------------------|-----------------------|
| Fresh Apples         | images/apple.jpg      |
| Organic Tomatoes     | images/tomatoes.jpg  |
| Green Lettuce        | images/lettuce.jpg    |

If a mismatch occurs, either:

- Rename the files in `app/static/images`, or  
- Edit the `PhotoURL` values directly in **phpMyAdmin**.

---

