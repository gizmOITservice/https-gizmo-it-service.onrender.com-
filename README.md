# Create a full website package as a ZIP file with index.html and placeholder images

import os 
from zipfile import ZipFile

# Directory structure
base_path = "/mnt/data/the_gizmo_website"
os.makedirs(base_path, exist_ok=True)
gallery_path = os.path.join(base_path, "images")
os.makedirs(gallery_path, exist_ok=True)

# Create index.html
index_html = """<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>The GizmO IT Service | Bangalore & Mysore</title>
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css"
    rel="stylesheet"
  />
  <style>
    body { scroll-behavior: smooth; }
    header {
      background: #004080;
      color: white;
      padding: 2rem 1rem;
      text-align: center;
    }
    .nav-link { color: white !important; }
    .cta-btn {
      background: #ff8000;
      color: white;
    }
    .service-card, .gallery-img {
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }
    footer {
      background: #222;
      color: #ddd;
      padding: 1rem;
      text-align: center;
    }
    .gallery-img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-radius: 5px;
    }
    .whatsapp-button {
      position: fixed;
      bottom: 20px;
      right: 20px;
      z-index: 999;
      background-color: #25D366;
      color: white;
      border-radius: 50%;
      padding: 15px;
      font-size: 24px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.3);
    }
    .whatsapp-button:hover {
      background-color: #1ebc59;
      text-decoration: none;
    }
  </style>
</head>
<body>

<header>
  <h1>The GizmO IT Service</h1>
  <p>Doorstep Computer & Laptop Repair – Bangalore & Mysore</p>
  <nav class="nav justify-content-center">
    <a class="nav-link" href="#home">Home</a>
    <a class="nav-link" href="#services">Services</a>
    <a class="nav-link" href="#gallery">Gallery</a>
    <a class="nav-link" href="#contact">Contact</a>
  </nav>
</header>

<section id="home" class="container py-5">
  <h2 class="text-center">Welcome to The GizmO IT Service</h2>
  <p class="lead text-center">
    Reliable doorstep support for all your computer and laptop needs — from virus removal to RAM and motherboard upgrades — across Bangalore and Mysore.
  </p>
  <div class="text-center">
    <a href="#contact" class="btn cta-btn btn-lg">Book a Service Now</a>
  </div>
</section>

<section id="services" class="container py-5 bg-light">
  <h2 class="mb-4 text-center">Our Services</h2>
  <div class="row">
    <div class="col-md-4 mb-4">
      <div class="card service-card p-3">
        <h5>Doorstep Repair</h5>
        <p>Desktop, Laptop & MacBook repair at your home or office.</p>
      </div>
    </div>
    <div class="col-md-4 mb-4">
      <div class="card service-card p-3">
        <h5>Motherboard & RAM Upgrades</h5>
        <p>Boost your device with new motherboards, RAM, and SSD upgrades.</p>
      </div>
    </div>
    <div class="col-md-4 mb-4">
      <div class="card service-card p-3">
        <h5>Virus Removal</h5>
        <p>Full cleanup, antivirus installation, and speed optimization.</p>
      </div>
    </div>
  </div>
</section>

<section id="gallery" class="container py-5">
  <h2 class="text-center mb-4">Our Work Gallery</h2>
  <div class="row">
    <div class="col-md-4 mb-4">
      <img src="images/repair1.jpg" alt="Repair 1" class="gallery-img" />
    </div>
    <div class="col-md-4 mb-4">
      <img src="images/repair2.jpg" alt="Repair 2" class="gallery-img" />
    </div>
    <div class="col-md-4 mb-4">
      <img src="images/repair3.jpg" alt="Repair 3" class="gallery-img" />
    </div>
  </div>
</section>

<section id="contact" class="container py-5 bg-light">
  <h2 class="text-center mb-4">Contact Us</h2>
  <div class="row justify-content-center">
    <div class="col-md-8">
      <p><strong>Phone / WhatsApp:</strong> <a href="https://wa.me/918431944493">+91 84319 44493</a></p>
      <p><strong>Email:</strong> <a href="mailto:gizmorepair6@gmail.com">gizmorepair6@gmail.com</a></p>
      <form>
        <div class="form-row">
          <div class="form-group col-md-6">
            <input type="text" class="form-control" placeholder="Your Name" required />
          </div>
          <div class="form-group col-md-6">
            <input type="tel" class="form-control" placeholder="Phone Number" required />
          </div>
        </div>
        <div class="form-group">
          <textarea class="form-control" rows="4" placeholder="How can we help you?" required></textarea>
        </div>
        <button type="submit" class="btn cta-btn btn-block">Submit Request</button>
      </form>
    </div>
  </div>
</section>

<footer>
  <p>© 2025 The GizmO IT Service – Bangalore & Mysore</p>
</footer>

<a href="https://wa.me/918431944493" class="whatsapp-button" target="_blank" title="Chat with us on WhatsApp">🟢</a>
</body>
</html>
"""

# Save HTML
with open(os.path.join(base_path, "index.html"), "w") as f:
    f.write(index_html)

# Add placeholder images
for i in range(1, 4):
    with open(os.path.join(gallery_path, f"repair{i}.jpg"), "wb") as img:
        img.write(b"\xFF\xD8\xFF\xE0" + b"\x00" * 1024 + b"\xFF\xD9")  # dummy JPEG file

# Create ZIP
zip_path = "/mnt/data/the_gizmo_it_service_website.zip"
with ZipFile(zip_path, 'w') as zipf:
    for root, dirs, files in os.walk(base_path):
        for file in files:
            full_path = os.path.join(root, file)
            arcname = os.path.relpath(full_path, base_path)
            zipf.write(full_path, arcname)

zip_path
