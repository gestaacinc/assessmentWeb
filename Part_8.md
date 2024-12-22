# Modal Order Implementation

This document provides a step-by-step guide for implementing the **Order Modal** in a Flask application. The modal allows users to view product details and place orders directly from the products page.

---

## Modal Structure: `modal_order.html`

The `modal_order.html` file contains the HTML structure for the order modal. It dynamically displays the selected product details and captures the user's order information.

### **Structure**

```html
<!-- Order Modal -->
<div class="modal fade" id="orderModal" tabindex="-1" aria-labelledby="orderModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="orderModalLabel">Place Your Order</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <form id="orderForm" action="/submit_order" method="post">
          <!-- Hidden Product ID -->
          <input type="hidden" id="productId" name="productId" />

          <!-- Product Details -->
          <div class="mb-3">
            <label class="form-label">Product</label>
            <img id="productImage" src="" alt="Product Image" style="width: 100%; max-width: 300px; height: auto" />
            <input type="text" class="form-control mt-2" id="productName" name="productName" readonly />
          </div>

          <!-- Customer Mobile Number -->
          <div class="mb-3">
            <label for="customerMobileNumber" class="form-label">Mobile Number</label>
            <input type="text" class="form-control" id="customerMobileNumber" name="customerMobileNumber" required />
          </div>

          <!-- Quantity Input -->
          <div class="mb-3">
            <label for="quantity" class="form-label">Quantity</label>
            <input type="number" class="form-control" id="quantity" name="quantity" min="1" required />
          </div>

          <!-- Total Price Display -->
          <div class="mb-3">
            <label class="form-label">Total Price</label>
            <p id="displayTotalPrice"></p>
            <input type="hidden" id="totalPrice" name="totalPrice" />
          </div>

          <button type="submit" class="btn btn-primary">Submit Order</button>
        </form>
      </div>
    </div>
  </div>
</div>
```

### **Explanation**

- **Hidden Product ID**: Stores the `productId` for the selected product, essential for processing the order.
- **Product Details**: Displays the product image and name to the user.
- **Customer Mobile Number**: Captures the customer’s mobile number.
- **Quantity Input**: Allows the user to specify the quantity of the product.
- **Total Price**: Displays the total price of the order and includes a hidden input for form submission.

---

## Modal Location

The `modal_order.html` file should be placed in the **`templates/partials/`** directory to enable easy inclusion in other templates.

---

## Integration with `products.html`

1. Update the "Order Now" buttons to pass the product data to the modal.
2. Include the `modal_order.html` partial in `products.html`.

### **Button Example with Data Attributes**

```html
<div class="mx-3 mb-3">
  <button
    class="btn btn-primary w-100"
    data-bs-toggle="modal"
    data-bs-target="#orderModal"
    data-product-id="{{ product[0] }}"
    data-product-name="{{ product[1] }}"
    data-product-price="{{ product[2] }}"
    data-product-image="{{ url_for('static', filename=product[4]) }}"
  >
    Order Now
  </button>
</div>
```

#### **Explanation**

- **Data Attributes**:  
  Each button includes:
  - `data-product-id`: Product ID
  - `data-product-name`: Product Name
  - `data-product-price`: Product Price
  - `data-product-image`: Product Image URL.
- **Modal Trigger**:  
  Clicking the button triggers the modal and passes the product data to it.

### **Include Modal in `products.html`**

```html
{% extends 'layout.html' %}

{% block content %}
<h1>Products</h1>
<div class="row">
    {% for product in products %}
    <div class="col-md-3 col-sm-6 mb-4">
        <div class="card">
            <img src="{{ url_for('static', filename=product[4]) }}" class="card-img-top img-fluid" alt="{{ product[1] }}">
            <div class="card-body">
                <h5 class="card-title">{{ product[1] }}</h5>
                <p class="card-text">Price: ₱{{ product[2] }}</p>
                <p class="card-text">Unit: {{ product[3] }}</p>
                <!-- Order Button -->
                <div class="mx-3 mb-3">
                    <button
                        class="btn btn-primary w-100"
                        data-bs-toggle="modal"
                        data-bs-target="#orderModal"
                        data-product-id="{{ product[0] }}"
                        data-product-name="{{ product[1] }}"
                        data-product-price="{{ product[2] }}"
                        data-product-image="{{ url_for('static', filename=product[4]) }}"
                    >
                        Order Now
                    </button>
                </div>
            </div>
        </div>
    </div>
    {% endfor %}
</div>

<!-- Include the Order Modal -->
{% include 'partials/modal_order.html' %}
{% endblock %}
```

---

## Next Steps

In the next step, we will implement **JavaScript functions** to dynamically populate the modal fields with the selected product’s data. This will ensure the modal displays accurate and relevant information for each product.
