# my-delivery-service<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>JKS Group E-Commerce with Camera & Maps</title>
<style>
  body { font-family: Arial, sans-serif; margin:0; padding:0; background:#f0f8ff; color:#003366; }
  nav { background:#00aaff; padding:1rem; display:flex; justify-content:center; gap:20px; flex-wrap:wrap; }
  nav button { background:#fff; border:none; padding:0.5rem 1rem; border-radius:5px; font-weight:bold; cursor:pointer; white-space:nowrap; }
  nav button:hover { background:#0077cc; color:#fff; }
  section { display:none; padding:1rem; }
  section.active, section.visible { display:block; }
  .items-grid { display:flex; flex-wrap:wrap; gap:10px; margin-top:10px; }
  .items-grid img { width:100px; height:100px; object-fit:cover; border-radius:5px; }
  form { margin-top:1rem; max-width:400px; }
  form label { display:block; margin-bottom:0.5rem; font-weight:bold; }
  form input, form textarea, form select { width:100%; padding:0.5rem; margin-bottom:1rem; border:1px solid #ccc; border-radius:5px; }
  form button { background:#00aaff; color:#fff; border:none; padding:0.7rem 1.5rem; border-radius:5px; font-weight:bold; cursor:pointer; }
  form button:hover { background:#0077cc; }
  video, canvas { display:block; margin: 10px 0; border-radius:5px; }
  .footer-btns { margin-top: 1rem; display: flex; justify-content: space-between; }
  .footer-btns button { background:#0077cc; color:#fff; border:none; padding:0.5rem 1rem; border-radius:5px; cursor:pointer; }
  .footer-btns button:hover { background:#005fa3; }
</style>
</head>
<body>

<nav>
  <button onclick="showSection('home')">Home</button>
  <button onclick="showSection('food')">Food</button>
  <button onclick="showSection('medicine')">Medicine</button>
  <button onclick="showSection('ecommerce')">E-Commerce</button>
  <button onclick="showSection('orderForm')">Order Form</button>
  <button onclick="showSection('camera')">Camera</button>
  <button onclick="showSection('maps')">Google Maps</button>
</nav>

<section id="home" class="active">
  <h2>Welcome to JKS E-Commerce</h2>
  <p>Select a category to browse items or use the order form.</p>
</section>

<section id="food">
  <h2>Food Category Items</h2>
  <div class="items-grid" id="foodItems"></div>
</section>

<section id="medicine">
  <h2>Medicine Category Items</h2>
  <div class="items-grid" id="medicineItems"></div>
</section>

<section id="ecommerce">
  <h2>E-Commerce Category Items</h2>
  <div class="items-grid" id="ecommerceItems"></div>
</section>

<section id="orderForm">
  <h2>Place Your Order</h2>
  <form id="orderFormElement" onsubmit="submitOrder(event)">
    <label for="categorySelect">Select Category</label>
    <select id="categorySelect" required>
      <option value="">--Select Category--</option>
      <option value="Food">Food</option>
      <option value="Medicine">Medicine</option>
      <option value="E-Commerce">E-Commerce</option>
    </select>

    <label for="itemSelect">Select Item</label>
    <select id="itemSelect" required>
      <option value="">--Select Item--</option>
    </select>

    <label for="name">Name</label>
    <input type="text" id="name" placeholder="Your Name" required />

    <label for="mobile">Mobile Number</label>
    <input type="tel" id="mobile" pattern="[0-9]{10}" placeholder="10-digit Mobile Number" required />

    <label for="orderDetails">Order Details</label>
    <textarea id="orderDetails" placeholder="Quantity, preferences, etc." rows="3" required></textarea>

    <label for="address">Delivery Address</label>
    <textarea id="address" placeholder="Your address details" rows="3" required></textarea>

    <button type="submit">Send Order via WhatsApp</button>
  </form>
</section>

<section id="camera" style="display:none;">
  <h2>Camera</h2>
  <video id="video" autoplay width="320" height="240"></video><br/>
  <button onclick="takePhoto()">Capture Photo</button>
  <canvas id="canvas" width="320" height="240" style="display:none;"></canvas><br/>
  <button onclick="closeCamera()">Close Camera</button>
</section>

<section id="maps" style="display:none;">
  <h2>Select Delivery Location</h2>
  <div id="map" style="width:100%; height:300px;"></div>
  <button onclick="confirmLocation()">Confirm Location</button>
  <button onclick="showSection('home')">Back</button>
</section>

<script>
// Categories and images for each category
const categories = {
  Food: [
    "https://via.placeholder.com/100?text=Food1",
    "https://via.placeholder.com/100?text=Food2",
    "https://via.placeholder.com/100?text=Food3",
    "https://via.placeholder.com/100?text=Food4",
    "https://via.placeholder.com/100?text=Food5",
    "https://via.placeholder.com/100?text=Food6",
    "https://via.placeholder.com/100?text=Food7",
    "https://via.placeholder.com/100?text=Food8",
    "https://via.placeholder.com/100?text=Food9",
    "https://via.placeholder.com/100?text=Food10"
  ],
  Medicine: [
    "https://via.placeholder.com/100?text=Med1",
    "https://via.placeholder.com/100?text=Med2",
    "https://via.placeholder.com/100?text=Med3",
    "https://via.placeholder.com/100?text=Med4",
    "https://via.placeholder.com/100?text=Med5",
    "https://via.placeholder.com/100?text=Med6",
    "https://via.placeholder.com/100?text=Med7",
    "https://via.placeholder.com/100?text=Med8",
    "https://via.placeholder.com/100?text=Med9",
    "https://via.placeholder.com/100?text=Med10"
  ],
  "E-Commerce": [
    "https://via.placeholder.com/100?text=Elec1",
    "https://via.placeholder.com/100?text=Elec2",
    "https://via.placeholder.com/100?text=Elec3",
    "https://via.placeholder.com/100?text=Elec4",
    "https://via.placeholder.com/100?text=Elec5",
    "https://via.placeholder.com/100?text=Elec6",
    "https://via.placeholder.com/100?text=Elec7",
    "https://via.placeholder.com/100?text=Elec8",
    "https://via.placeholder.com/100?text=Elec9",
    "https://via.placeholder.com/100?text=Elec10"
  ]
}

// Show selected section only with display block/none
function showSection(sectionId) {
  // Stop camera if running and switching from camera section
  if (sectionId !== 'camera' && camStream) closeCamera();

  // Hide all sections
  document.querySelectorAll('section').forEach(s => s.style.display = 'none');
  // Show selected section
  const sec = document.getElementById(sectionId);
  if(sec) sec.style.display = 'block';

  // Auto fill items grids when showing category sections
  if(sectionId === 'food') fillItemImages('Food', 'foodItems');
  if(sectionId === 'medicine') fillItemImages('Medicine', 'medicineItems');
  if(sectionId === 'ecommerce') fillItemImages('E-Commerce', 'ecommerceItems');
  if(sectionId === 'orderForm') resetOrderForm();
  if(sectionId === 'camera') openCamera();
  if(sectionId === 'maps') showMap();
}

// Fill images grid for categories with placeholder images
function fillItemImages(category, containerId) {
  const container = document.getElementById(containerId);
  container.innerHTML = "";
  categories[category].forEach((url, idx) => {
    const img = document.createElement('img');
    img.src = url;
    img.alt = `${category} Item ${idx + 1}`;
    container.appendChild(img);
  });
}

// Order form reset and item population
function resetOrderForm() {
  const catSelect = document.getElementById('categorySelect');
  const itemSelect = document.getElementById('itemSelect');
  catSelect.value = "";
  itemSelect.innerHTML = '<option value="">--Select Item--</option>';
}

// Populate items in order form on category change
document.getElementById('categorySelect').addEventListener('change', function() {
  const items = categories[this.value] || [];
  const itemSelect = document.getElementById('itemSelect');
  itemSelect.innerHTML = '<option value="">--Select Item--</option>';
  items.forEach((url, idx) => {
    const opt = document.createElement('option');
    opt.value = idx;
    opt.textContent = `${this.value} Item ${idx + 1}`;
    itemSelect.appendChild(opt);
  });
});

// Submit order - sending WhatsApp message
function submitOrder(e) {
  e.preventDefault();
  const category = document.getElementById('categorySelect').value;
  const itemIdx = document.getElementById('itemSelect').value;

  if(itemIdx === "") {
    alert("Please select an item.");
    return;
  }

  const itemName = `${category} Item ${parseInt(itemIdx) + 1}`;
  const name = document.getElementById('name').value.trim();
  const mobile = document.getElementById('mobile').value.trim();
  const details = document.getElementById('orderDetails').value.trim();
  const address = document.getElementById('address').value.trim();

  const message = `JKS Order:\nCategory: ${category}\nItem: ${itemName}\nName: ${name}\nMobile: ${mobile}\nOrder Details: ${details}\nDelivery Address: ${address}`;

  const whatsappNumber = '919877143043'; // CHANGE to your WhatsApp number without '+'
  window.open(`https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`, '_blank');
  alert('Order sent successfully!');
  showSection('home');
}

// --- Camera code ---
let camStream;
const video = document.getElementById('video');
const canvas = document.getElementById('canvas');

function openCamera() {
  navigator.mediaDevices.getUserMedia({ video: true })
  .then(stream => {
    camStream = stream;
    video.srcObject = camStream;
  })
  .catch(() => alert('Camera access denied or not available.'));
}

function takePhoto() {
  const ctx = canvas.getContext('2d');
  canvas.style.display = 'block';
  ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
  alert('Photo captured.');
}

function closeCamera() {
  if(camStream) {
    camStream.getTracks().forEach(track => track.stop());
    camStream = null;
  }
  showSection('home');
}

// --- Google Maps code ---
let map, marker;
function initMap() {
  if(map) return; // only init once
  const defaultPos = { lat: 17.3850, lng: 78.4867 };
  map = new google.maps.Map(document.getElementById('map'), { zoom: 12, center: defaultPos });
  marker = new google.maps.Marker({ position: defaultPos, map: map, draggable: true });
}

function showMap() {
  showSection('maps');
  if(!map) initMap();
}

function confirmLocation() {
  if(marker) {
    const pos = marker.getPosition();
    alert(`Location selected: ${pos.lat().toFixed(6)}, ${pos.lng().toFixed(6)}`);
    showSection('home');
  }
}
</script>

<!-- Add your valid Google Maps API key below -->
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&callback=initMap" async defer></script>

</body>
</html>
