# Project 1 Flask Kiosk Setup Guide

This guide will walk you through setting up the basic structure for the Project 1 Flask Kiosk application, focusing on the initial setup excluding Flask-WTF and SQLAlchemy.

## Project Structure

Organize your project directory as follows:

```
FarmersMarketApp/
├── app/
│   ├── static/
│   │   ├── css/
│   │   │   └── styles.css
│   │   ├── js/
│   │   │   └── script.js
│   │   └── images/
│   │       └── [product images]
│   ├── templates/
│   │   ├── layout.html
│   │   ├── index.html
│   │   ├── product.html
│   │   └── order_confirmation.html
│   ├── __init__.py
│   ├── routes.py
├── venv/
├── requirements.txt
└── run.py
```

## Setting Up Virtual Environment and Flask

### 1. Create Project Directory and Virtual Environment

Open your command prompt and create your project directory:

```bash
mkdir FarmersMarketApp
cd FarmersMarketApp
```

Create a virtual environment:

```bash
python -m venv venv
```

Activate the virtual environment:

```bash
venv\Scripts\activate
```

### 2. Install Flask and Flask-MySQLdb

Install Flask:

```bash
pip install flask
```

Install Flask-MySQLdb for database interaction:

```bash
pip install flask-mysqldb
```

### 3. Create `requirements.txt`

Generate a `requirements.txt` file:

```bash
pip freeze > requirements.txt
```

## Next Steps

With your environment set up, you're ready to start developing the homepage for your application using Flask. Structure your Flask application with the given directory layout for efficient project management.
