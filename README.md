<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Admin Login</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <header>
        <h1>Admin Login</h1>
    </header>
    <form class="booking-form" method="POST" style="max-width: 400px; margin: auto;">
        {% if error %}
            <p style="color: red;">{{ error }}</p>
        {% endif %}
        <label>Username: <input type="text" name="username" required></label>
        <label>Password: <input type="password" name="password" required></label>
        <button type="submit">Login</button>
    </form>
</body>
</html>
from flask import Flask, render_template, request, redirect, session, url_for
import csv
import os

app = Flask(__name__)
app.secret_key = 'supersecretkey'  # Change this to any strong string

ADMIN_USERNAME = 'admin'
ADMIN_PASSWORD = 'gizmo123'  # Change this to your own password

# ... (keep your home, about, and book routes here)

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        if username == ADMIN_USERNAME and password == ADMIN_PASSWORD:
            session['logged_in'] = True
            return redirect('/admin')
        else:
            return render_template('login.html', error="Invalid credentials")
    return render_template('login.html')

@app.route('/logout')
def logout():
    session.pop('logged_in', None)
    return redirect('/login')

@app.route('/admin')
def admin():
    if not session.get('logged_in'):
        return redirect('/login')

    bookings = []
    file_path = 'data/bookings.csv'
    if os.path.exists(file_path):
        with open(file_path, newline='') as f:
            reader = csv.DictReader(f)
            bookings = list(reader)
    return render_template('admin.html', bookings=bookings)