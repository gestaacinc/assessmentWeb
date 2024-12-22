# JavaScript Implementation for Modal Order Form

This document outlines the JavaScript implementation for handling the **Order Modal** in the Flask application. The JavaScript is structured to initialize the modal with product data, dynamically update the total price, and reset the modal to its default state.

---

## JavaScript Code

```javascript
let currentProductPrice = 0;

function updateTotalPrice() {
  let quantity = parseFloat(document.getElementById("quantity").value) || 0;
  let totalPrice = currentProductPrice * quantity;
  document.getElementById("displayTotalPrice").textContent = totalPrice.toFixed(2);
  document.getElementById("totalPrice").value = totalPrice.toFixed(2);
}

function resetModal() {
  // Reset all fields to default values
  document.getElementById("productId").value = "";
  document.getElementById("productName").value = "";
  document.getElementById("productImage").src = "";
  document.getElementById("quantity").value = 1; // Default quantity
  document.getElementById("displayTotalPrice").textContent = "0.00";
  document.getElementById("totalPrice").value = "0.00";
  currentProductPrice = 0;
}

let orderModal = document.getElementById("orderModal");
orderModal.addEventListener("show.bs.modal", function (event) {
  resetModal(); // Reset modal fields

  let button = event.relatedTarget; // Button that triggered the modal

  // Extract info from data-* attributes
  let productId = button.getAttribute("data-product-id");
  let productName = button.getAttribute("data-product-name");
  let productPrice = button.getAttribute("data-product-price");
  let productImage = button.getAttribute("data-product-image");

  // Update the modal's content
  document.getElementById("productId").value = productId;
  document.getElementById("productName").value = productName;
  document.getElementById("productImage").src = productImage;

  currentProductPrice = parseFloat(productPrice);
  updateTotalPrice(); // Update total price based on the default quantity
});
```

---

## Explanation of JavaScript Functions

### 1. **Updating Total Price**

The `updateTotalPrice` function dynamically calculates the total price based on the quantity entered by the user.

- **Purpose**:
  - Recalculate the total price whenever the quantity changes.
- **Code**:
  ```javascript
  function updateTotalPrice() {
    let quantity = parseFloat(document.getElementById("quantity").value) || 0;
    let totalPrice = currentProductPrice * quantity;
    document.getElementById("displayTotalPrice").textContent = totalPrice.toFixed(2);
    document.getElementById("totalPrice").value = totalPrice.toFixed(2);
  }
  ```

---

### 2. **Resetting the Modal**

The `resetModal` function ensures that the modal resets to its default state every time it is opened.

- **Purpose**:
  - Clear any residual data from the previous use of the modal.
- **Code**:
  ```javascript
  function resetModal() {
    document.getElementById("productId").value = "";
    document.getElementById("productName").value = "";
    document.getElementById("productImage").src = "";
    document.getElementById("quantity").value = 1; // Default quantity
    document.getElementById("displayTotalPrice").textContent = "0.00";
    document.getElementById("totalPrice").value = "0.00";
    currentProductPrice = 0;
  }
  ```

---

### 3. **Initializing the Modal with Product Data**

The `show.bs.modal` event listener initializes the modal with the product details from the clicked button's `data-*` attributes.

- **Purpose**:
  - Dynamically populate the modal fields with product-specific data.
- **Code**:
  ```javascript
  let orderModal = document.getElementById("orderModal");
  orderModal.addEventListener("show.bs.modal", function (event) {
    resetModal(); // Reset modal fields

    let button = event.relatedTarget; // Button that triggered the modal

    // Extract info from data-* attributes
    let productId = button.getAttribute("data-product-id");
    let productName = button.getAttribute("data-product-name");
    let productPrice = button.getAttribute("data-product-price");
    let productImage = button.getAttribute("data-product-image");

    // Update the modal's content
    document.getElementById("productId").value = productId;
    document.getElementById("productName").value = productName;
    document.getElementById("productImage").src = productImage;

    currentProductPrice = parseFloat(productPrice);
    updateTotalPrice(); // Update total price based on the default quantity
  });
  ```

---

## Implementation Steps

1. **Include JavaScript in the Application**:
   - Add the script directly in your `products.html` file inside a `<script>` tag, or
   - Place the code in a separate file, e.g., `app/static/js/orderModal.js`, and include it in `products.html` using:
     ```html
     <script src="{{ url_for('static', filename='js/orderModal.js') }}"></script>
     ```

2. **Test the Modal**:
   - Run your Flask application:
     ```bash
     python run.py
     ```
   - Open the products page and click on any "Order Now" button to verify:
     - The modal displays the correct product details.
     - The total price updates when you change the quantity.

---

