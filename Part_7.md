# Interactive Image Display with Modals in Flask

This guide demonstrates how to implement an interactive feature in a Flask application where clicking on product images displays them in a Bootstrap modal. Using a separate partial template for the modal helps maintain clean and organized code.

---

## Prerequisites

1. A Flask application with a configured **products page**.
2. **Bootstrap** integrated into the application for modal functionality.

---

## Step 1: Create the Modal Partial Template

1. Create a folder named `partials` inside the `templates` directory if it doesn't already exist.
2. Inside the `partials` folder, create a file named `image_modal.html`.

**Example: `templates/partials/image_modal.html`**

```html
<!-- Bootstrap Modal -->
<div class="modal fade" id="productModal" tabindex="-1" aria-labelledby="productModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-lg">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="productModalLabel">Product Image</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <img id="modalImg" src="" class="img-fluid" alt="Product">
      </div>
    </div>
  </div>
</div>
```

---

## Step 2: Include the Modal Partial in `product.html`

1. Open the `product.html` file.
2. Use Jinja2’s `{% include %}` directive to include the modal partial in your products page.

**Example: Including the Modal Partial**

```html
<!-- product.html -->

{% extends 'layout.html' %}

{% block content %}
<h1>Products</h1>
<div class="row">
    {% for product in products %}
    <div class="col-md-3 col-sm-6 mb-4">
        <div class="card">
            <img src="{{ url_for('static', filename='images/' + product.PhotoURL) }}" 
                 class="card-img-top img-fluid" 
                 alt="{{ product.ProductName }}" 
                 data-bs-toggle="modal" 
                 data-bs-target="#productModal" 
                 onclick="updateModalImage(this.src)">
        </div>
    </div>
    {% endfor %}
</div>

<!-- Include the image modal from the partials folder -->
{% include 'partials/image_modal.html' %}
{% endblock %}
```

---

## Step 3: Set Up Product Images to Trigger the Modal

1. Modify the product image display loop in `product.html` to ensure each image triggers the modal on click.
2. Add the `data-bs-toggle` and `data-bs-target` attributes to the `<img>` tag and include an `onclick` event to handle the modal image update.

---

## Step 4: Add JavaScript for Updating Modal Image

1. Inside `product.html`, include a JavaScript function to update the modal’s image source when an image is clicked.

**Example: JavaScript in `product.html`**

```html
<script>
function updateModalImage(imageSrc) {
    document.getElementById('modalImg').src = imageSrc;
}
</script>
```

---

## Step 5: Test the Functionality

1. Start your Flask application:

   ```bash
   python run.py
   ```

2. Navigate to [http://127.0.0.1:5000/products](http://127.0.0.1:5000/products).
3. Click on any product image:
   - A modal should open displaying a larger version of the clicked image.

---

## Additional Notes

1. **Ensure Images Are Properly Linked**:
   - The product images must be stored in the `app/static/images` directory.
   - The file names in the `PhotoURL` column of your `product` table must match the actual file names in the `images` folder.

2. **To Fix Missing Images**:
   - Add missing images to the `app/static/images` directory, or
   - Update the `PhotoURL` values in **phpMyAdmin** to match the available image files.

---

