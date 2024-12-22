# Saving Orders and Implementing Flash Messages in Flask

This document outlines the steps to save user orders to a MySQL database, provide feedback using **flash messages**, and configure a secret key for Flask applications.

---

## 1. Saving the Order

When a user submits an order, the `/submit_order` route processes and saves it to the database. 

### **Flask Route for Order Submission**

```python
@app.route('/submit_order', methods=['POST'])
def submit_order():
    try:
        # Retrieve form data
        product_id = request.form['productId']
        customer_mobile_number = request.form['customerMobileNumber']
        quantity = request.form['quantity']
        total_cost = request.form['totalPrice']

        # Insert order into the database
        order_id = insert_order_into_database(customer_mobile_number, total_cost, product_id, quantity)

        # Flash success message and redirect
        flash('Order submitted successfully!', 'success')
        return redirect(url_for('index'))
    except Exception as e:
        # Flash error message and redirect
        flash(f'Error processing your order: {str(e)}', 'error')
        return redirect(url_for('index'))
```

### **Process**

1. **Form Data Retrieval**:
   - Retrieves form data (`productId`, `customerMobileNumber`, `quantity`, `totalPrice`) submitted via POST.

2. **Database Insertion**:
   - Calls `insert_order_into_database` to save the order and order details.

3. **Success Feedback**:
   - Displays a success flash message if the order is saved successfully.

4. **Error Handling**:
   - Displays an error flash message if something goes wrong.

---

## 2. Inserting Orders into the Database

### **`insert_order_into_database` Function**

```python
def insert_order_into_database(customer_mobile_number, total_cost, product_id, quantity):
    cursor = mysql.connection.cursor()

    # Insert order into `order` table
    cursor.execute("INSERT INTO `order` (CustomerMobileNumber, TotalCost) VALUES (%s, %s)",
                   (customer_mobile_number, total_cost))
    order_id = cursor.lastrowid

    # Insert order details into `orderdetails` table
    cursor.execute("INSERT INTO orderdetails (OrderID, ProductID, Quantity) VALUES (%s, %s, %s)",
                   (order_id, product_id, quantity))

    # Commit transaction and close cursor
    mysql.connection.commit()
    cursor.close()
    return order_id
```

### **Process**

1. **Insert into `order` Table**:
   - Saves the customer's mobile number and total cost.
   - Retrieves the `order_id` for the new record using `lastrowid`.

2. **Insert into `orderdetails` Table**:
   - Uses the `order_id` to save product-specific details (product ID and quantity).

3. **Transaction Handling**:
   - Uses `commit` to ensure data integrity.

---

## 3. Implementing Flash Messages

Flash messages provide immediate feedback to users about the result of their actions.

### **Displaying Flash Messages**

Add the following snippet to `products.html` (or any other relevant template) to display flash messages:

```html
{% with messages = get_flashed_messages(with_categories=true) %}
  {% if messages %}
    {% for category, message in messages %}
      <div class="alert alert-{{ category }}">{{ message }}</div>
    {% endfor %}
  {% endif %}
{% endwith %}
```

### **Explanation**

- **`get_flashed_messages`**:
  - Retrieves all flashed messages with their categories.
- **Bootstrap Alert Classes**:
  - The `category` (e.g., `'success'` or `'error'`) is used to apply the corresponding Bootstrap alert class.

---

## 4. Setting a Secret Key in `__init__.py`

A **secret key** is essential for securely signing session data and enabling flash messages.

### **Setup in `__init__.py`**

```python
from flask import Flask
from flask_mysqldb import MySQL

app = Flask(__name__)

# Set a secure and unique secret key
app.secret_key = 'yoursecretkey'

# Database connection
app.config['MYSQL_HOST'] = 'localhost'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = ''
app.config['MYSQL_DB'] = 'kiosk_app'

# Initialize MySQL
mysql = MySQL(app)

from application import routes
```

### **Why Set a Secret Key?**

- Secures session data, including flash messages.
- Prevents tampering with session cookies.
- Should be kept secret and unique, especially in production environments.

---

## 5. Testing the Implementation

### **Steps to Test**

1. **Submit an Order**:
   - Open the products page, fill out the order form, and submit.

2. **Check Feedback**:
   - Verify that success or error flash messages appear at the top of the page.

3. **Verify Database Records**:
   - Use **phpMyAdmin** or another MySQL client to confirm that the order and order details are correctly saved in the database.

---

## Conclusion

This implementation ensures the following:

1. **Database Integration**:
   - Orders and order details are saved with transactional integrity.

2. **User Feedback**:
   - Flash messages provide users with immediate success or error notifications.

3. **Secure Configuration**:
   - A secret key ensures session security and enables flash messages.

By combining these elements, your Flask application provides a secure and user-friendly experience for handling orders.
