from flask import Flask, render_template, request, redirect
import csv
import os

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/book', methods=['GET', 'POST'])
def book():
    if request.method == 'POST':
        booking = {
            'name': request.form['name'],
            'phone': request.form['phone'],
            'city': request.form['city'],
            'description': request.form['description']
        }
        file_exists = os.path.isfile('data/bookings.csv')
        with open('data/bookings.csv', 'a', newline='') as f:
            writer = csv.DictWriter(f, fieldnames=booking.keys())
            if not file_exists:
                writer.writeheader()
            writer.writerow(booking)
        return redirect('/')
    return render_template('book.html')

@app.route('/about')
def about():
    return render_template('about.html')

if __name__ == '__main__':
    app.run(debug=True)
templates/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>The GizmO IT Service</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <header>
        <img src="{{ url_for('static', filename='logo.png') }}" alt="The GizmO Logo" class="logo">
        <h1>The GizmO IT Service</h1>
        <p>Computer Repair & IT Services - Bangalore and Mysore</p>
        <a href="https://wa.me/918431944493" class="cta-button">WhatsApp Us</a>
    </header>

    <section class="services">
        <h2>Our Services</h2>
        <ul>
            <li>Laptop Repair & Speed Upgrade</li>
            <li>Motherboard Upgrade & Repair</li>
            <li>Virus & Malware Removal</li>
            <li>Software Installation & Updates</li>
            <li>MacBook Services</li>
            <li>Doorstep Service</li>
        </ul>
    </section>

    <section class="contact">
        <h2>Contact Us</h2>
        <p>📞 Phone: 8431944493</p>
        <p>📧 Email: gizmorepair6@gmail.com</p>
        <p>📍 Locations: Bangalore & Mysore</p>
        <a href="/book" class="cta-button">Book a Service</a>
    </section>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Book a Service</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <header>
        <h1>Book a Service</h1>
        <a href="/" class="cta-button">Back to Home</a>
    </header>

    <form class="booking-form" method="POST">
        <label>Name: <input type="text" name="name" required></label>
        <label>Phone: <input type="tel" name="phone" required></label>
        <label>City: 
            <select name="city">
                <option>Bangalore</option>
                <option>Mysore</option>
            </select>
        </label>
        <label>Problem Description:<br><textarea name="description" rows="4"></textarea></label>
        <button type="submit">Submit</button>
    </form>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>About Us - The GizmO IT Service</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <header>
        <h1>About The GizmO IT Service</h1>
        <a href="/" class="cta-button">Back to Home</a>
    </header>
    <section>
        <p>We offer doorstep computer repair and IT support in Bangalore and Mysore. From laptop upgrades to virus removal and MacBook service, we handle all your tech needs.</p>
        <p>Our expert technicians are just a call away. Trust, speed, and transparency – that’s our promise.</p>
    </section>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background: #f5f5f5;
}
header {
    background: #002B5B;
    color: white;
    padding: 20px;
    text-align: center;
}
.logo {
    width: 100px;
    margin-bottom: 10px;
}
.cta-button {
    background-color: #0078D7;
    color: white;
    padding: 10px 20px;
    text-decoration: none;
    display: inline-block;
    margin-top: 10px;
}
.services, .contact, form {
    padding: 20px;
    background: white;
    margin: 20px;
}
.booking-form label {
    display: block;
    margin-bottom: 10px;
}
.booking-form input, .booking-form textarea, .booking-form select {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
}
.booking-form button {
    margin-top: 15px;
    padding: 10px 20px;
    background: #0078D7;
    color: white;
    border: none;
}
flask
python-3.10