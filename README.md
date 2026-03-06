<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Room Finder - Find Your Perfect PG or Room Rental</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            line-height: 1.6;
        }

        /* Header & Navigation */
        header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 1rem 0;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 2rem;
        }

        .logo {
            font-size: 1.8rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        nav ul {
            display: flex;
            list-style: none;
            gap: 2rem;
        }

        nav a {
            color: white;
            text-decoration: none;
            transition: opacity 0.3s;
        }

        nav a:hover {
            opacity: 0.8;
        }

        /* Hero Section */
        .hero {
            background: linear-gradient(135deg, rgba(102, 126, 234, 0.9) 0%, rgba(118, 75, 162, 0.9) 100%),
                        url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 600"><path d="M0,300 Q300,200 600,300 T1200,300 L1200,600 L0,600 Z" fill="white" opacity="0.1"/></svg>');
            background-size: cover;
            background-position: center;
            color: white;
            padding: 5rem 2rem;
            text-align: center;
        }

        .hero h1 {
            font-size: 3rem;
            margin-bottom: 1rem;
        }

        .hero p {
            font-size: 1.3rem;
            margin-bottom: 2rem;
        }

        /* Search Bar */
        .search-container {
            max-width: 1200px;
            margin: -2rem auto 3rem;
            padding: 0 2rem;
            position: relative;
            z-index: 10;
        }

        .search-bar {
            background: white;
            padding: 2rem;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            display: grid;
            grid-template-columns: 1fr 1fr 1fr auto;
            gap: 1rem;
            align-items: end;
        }

        .search-group {
            display: flex;
            flex-direction: column;
        }

        .search-group label {
            font-weight: 600;
            margin-bottom: 0.5rem;
            color: #667eea;
        }

        .search-group input,
        .search-group select {
            padding: 0.8rem;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1rem;
            transition: border-color 0.3s;
        }

        .search-group input:focus,
        .search-group select:focus {
            outline: none;
            border-color: #667eea;
        }

        .search-btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 0.8rem 2rem;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: transform 0.3s, box-shadow 0.3s;
        }

        .search-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        /* Main Content */
        main {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 2rem;
            min-height: 60vh;
        }

        .container-flex {
            display: grid;
            grid-template-columns: 250px 1fr;
            gap: 2rem;
        }

        /* Sidebar Filters */
        .sidebar {
            background: white;
            padding: 1.5rem;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            height: fit-content;
            position: sticky;
            top: 100px;
        }

        .filter-group {
            margin-bottom: 1.5rem;
            padding-bottom: 1.5rem;
            border-bottom: 1px solid #eee;
        }

        .filter-group:last-child {
            border-bottom: none;
        }

        .filter-group h3 {
            font-size: 1.1rem;
            margin-bottom: 1rem;
            color: #667eea;
        }

        .filter-group label {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            margin-bottom: 0.8rem;
            cursor: pointer;
            transition: color 0.3s;
        }

        .filter-group label:hover {
            color: #667eea;
        }

        .filter-group input[type="checkbox"],
        .filter-group input[type="radio"] {
            cursor: pointer;
            accent-color: #667eea;
        }

        .price-range {
            display: flex;
            gap: 0.5rem;
            align-items: center;
        }

        .price-range input {
            flex: 1;
            padding: 0.5rem;
        }

        /* Room Grid */
        .room-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 2rem;
        }

        .room-card {
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s, box-shadow 0.3s;
            cursor: pointer;
        }

        .room-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
        }

        .room-image {
            width: 100%;
            height: 200px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            position: relative;
            overflow: hidden;
        }

        .room-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .gallery-nav {
            position: absolute;
            bottom: 10px;
            width: 100%;
            display: flex;
            justify-content: center;
            gap: 5px;
        }

        .gallery-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.5);
            cursor: pointer;
            transition: background 0.3s;
        }

        .gallery-dot.active {
            background: white;
        }

        .price-tag {
            position: absolute;
            top: 10px;
            right: 10px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 0.5rem 1rem;
            border-radius: 20px;
            font-weight: bold;
        }

        .room-content {
            padding: 1.5rem;
        }

        .room-title {
            font-size: 1.3rem;
            margin-bottom: 0.5rem;
            color: #333;
        }

        .room-location {
            color: #666;
            font-size: 0.9rem;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .room-details {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 0.8rem;
            margin-bottom: 1rem;
            font-size: 0.9rem;
            color: #666;
        }

        .detail-item {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .room-description {
            color: #666;
            font-size: 0.9rem;
            margin-bottom: 1rem;
            line-height: 1.4;
        }

        .room-footer {
            display: flex;
            gap: 0.5rem;
            margin-bottom: 1rem;
        }

        .amenity-tag {
            background: #f0f0f0;
            padding: 0.3rem 0.8rem;
            border-radius: 15px;
            font-size: 0.8rem;
            color: #666;
        }

        .room-actions {
            display: flex;
            gap: 1rem;
        }

        .btn-contact {
            flex: 1;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 0.8rem;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 600;
            transition: transform 0.3s;
        }

        .btn-contact:hover {
            transform: scale(1.05);
        }

        .btn-map {
            background: #f0f0f0;
            border: none;
            padding: 0.8rem 1rem;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            transition: background 0.3s;
        }

        .btn-map:hover {
            background: #e0e0e0;
        }

        /* Map Section */
        .map-section {
            background: white;
            padding: 2rem;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            margin-top: 2rem;
            display: none;
        }

        .map-section.active {
            display: block;
        }

        #map {
            width: 100%;
            height: 500px;
            border-radius: 8px;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
        }

        .modal.active {
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background-color: white;
            padding: 2rem;
            border-radius: 10px;
            width: 90%;
            max-width: 600px;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
        }

        .close {
            color: #aaa;
            float: right;
            font-size: 2rem;
            font-weight: bold;
            cursor: pointer;
            transition: color 0.3s;
        }

        .close:hover {
            color: #000;
        }

        .modal h2 {
            color: #667eea;
            margin-bottom: 1rem;
            margin-top: 0;
        }

        .form-group {
            margin-bottom: 1rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: #333;
        }

        .form-group input,
        .form-group textarea {
            width: 100%;
            padding: 0.8rem;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1rem;
            font-family: inherit;
        }

        .form-group textarea {
            resize: vertical;
            min-height: 100px;
        }

        .form-group input:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: #667eea;
        }

        .submit-btn {
            width: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 1rem;
            border-radius: 5px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.3s;
        }

        .submit-btn:hover {
            transform: scale(1.02);
        }

        /* Footer */
        footer {
            background: #333;
            color: white;
            text-align: center;
            padding: 2rem;
            margin-top: 3rem;
        }

        footer a {
            color: #667eea;
            text-decoration: none;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .hero h1 {
                font-size: 2rem;
            }

            .search-bar {
                grid-template-columns: 1fr;
            }

            .container-flex {
                grid-template-columns: 1fr;
            }

            .sidebar {
                position: static;
            }

            .room-grid {
                grid-template-columns: 1fr;
            }

            nav ul {
                gap: 1rem;
                font-size: 0.9rem;
            }
        }

        .no-results {
            grid-column: 1 / -1;
            text-align: center;
            padding: 3rem;
            color: #666;
        }

        .view-toggle {
            display: flex;
            gap: 1rem;
            margin-bottom: 2rem;
            justify-content: flex-end;
        }

        .view-btn {
            padding: 0.5rem 1rem;
            border: 1px solid #ddd;
            background: white;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .view-btn.active {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header>
        <nav>
            <div class="logo">
                <i class="fas fa-home"></i> Room Finder
            </div>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#listings">Listings</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <!-- Hero Section -->
    <section class="hero">
        <h1>Find Your Perfect Room or PG</h1>
        <p>Search through hundreds of verified listings near you</p>
    </section>

    <!-- Search Bar -->
    <div class="search-container">
        <div class="search-bar">
            <div class="search-group">
                <label for="locationSearch"><i class="fas fa-map-marker-alt"></i> Location</label>
                <input type="text" id="locationSearch" placeholder="e.g., Delhi, Mumbai...">
            </div>
            <div class="search-group">
                <label for="priceSearch"><i class="fas fa-tag"></i> Max Price</label>
                <input type="number" id="priceSearch" placeholder="e.g., 15000">
            </div>
            <div class="search-group">
                <label for="typeSearch"><i class="fas fa-door-open"></i> Room Type</label>
                <select id="typeSearch">
                    <option value="">All Types</option>
                    <option value="single">Single Room</option>
                    <option value="double">Double Room</option>
                    <option value="shared">Shared Room</option>
                    <option value="apartment">Apartment</option>
                </select>
            </div>
            <button class="search-btn" onclick="filterRooms()">
                <i class="fas fa-search"></i> Search
            </button>
        </div>
    </div>

    <!-- Main Content -->
    <main>
        <div class="container-flex">
            <!-- Sidebar Filters -->
            <aside class="sidebar">
                <div class="filter-group">
                    <h3><i class="fas fa-filter"></i> Filters</h3>
                    <label>
                        <input type="checkbox" id="filter-wifi" onchange="filterRooms()"> WiFi
                    </label>
                    <label>
                        <input type="checkbox" id="filter-parking" onchange="filterRooms()"> Parking
                    </label>
                    <label>
                        <input type="checkbox" id="filter-furnished" onchange="filterRooms()"> Furnished
                    </label>
                    <label>
                        <input type="checkbox" id="filter-ac" onchange="filterRooms()"> Air Conditioned
                    </label>
                    <label>
                        <input type="checkbox" id="filter-gym" onchange="filterRooms()"> Gym
                    </label>
                </div>

                <div class="filter-group">
                    <h3>Price Range</h3>
                    <div class="price-range">
                        <input type="number" id="minPrice" placeholder="Min" value="0">
                        <span>-</span>
                        <input type="number" id="maxPrice" placeholder="Max" value="50000">
                    </div>
                    <button class="search-btn" style="width: 100%; margin-top: 1rem;" onclick="filterRooms()">
                        Apply
                    </button>
                </div>

                <div class="filter-group">
                    <h3>Room Type</h3>
                    <label>
                        <input type="checkbox" id="type-single" onchange="filterRooms()"> Single
                    </label>
                    <label>
                        <input type="checkbox" id="type-double" onchange="filterRooms()"> Double
                    </label>
                    <label>
                        <input type="checkbox" id="type-shared" onchange="filterRooms()"> Shared
                    </label>
                    <label>
                        <input type="checkbox" id="type-apartment" onchange="filterRooms()"> Apartment
                    </label>
                </div>

                <div class="filter-group">
                    <h3>Ratings</h3>
                    <label>
                        <input type="radio" name="rating" value="5" onchange="filterRooms()"> ⭐⭐⭐⭐⭐
                    </label>
                    <label>
                        <input type="radio" name="rating" value="4" onchange="filterRooms()"> ⭐⭐⭐⭐
                    </label>
                    <label>
                        <input type="radio" name="rating" value="3" onchange="filterRooms()"> ⭐⭐⭐
                    </label>
                    <label>
                        <input type="radio" name="rating" value="" onchange="filterRooms()"> All Ratings
                    </label>
                </div>
            </aside>

            <!-- Room Listings -->
            <div>
                <div class="view-toggle">
                    <button class="view-btn active" onclick="switchView('grid')">
                        <i class="fas fa-th"></i> Grid View
                    </button>
                    <button class="view-btn" onclick="switchView('map')">
                        <i class="fas fa-map"></i> Map View
                    </button>
                </div>

                <div id="roomGrid" class="room-grid"></div>

                <!-- Map Section -->
                <div id="mapSection" class="map-section">
                    <div id="map"></div>
                </div>
            </div>
        </div>
    </main>

    <!-- Contact Modal -->
    <div id="contactModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeModal()">&times;</span>
            <h2>Contact Property Owner</h2>
            <form onsubmit="submitForm(event)">
                <div class="form-group">
                    <label for="ownerName">Property Name:</label>
                    <input type="text" id="ownerName" disabled>
                </div>
                <div class="form-group">
                    <label for="yourName">Your Name:</label>
                    <input type="text" id="yourName" required>
                </div>
                <div class="form-group">
                    <label for="yourEmail">Your Email:</label>
                    <input type="email" id="yourEmail" required>
                </div>
                <div class="form-group">
                    <label for="yourPhone">Your Phone:</label>
                    <input type="tel" id="yourPhone" required>
                </div>
                <div class="form-group">
                    <label for="message">Message:</label>
                    <textarea id="message" required placeholder="Tell them why you're interested..."></textarea>
                </div>
                <button type="submit" class="submit-btn">Send Message</button>
            </form>
        </div>
    </div>

    <!-- Footer -->
    <footer>
        <p>&copy; 2026 Room Finder. All rights reserved. | <a href="#">Privacy Policy</a> | <a href="#">Terms of Service</a></p>
    </footer>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>
    <script>
        // Sample room data with image placeholders
        const rooms = [
            {
                id: 1,
                name: "Cozy Single Room in Sector 5",
                location: "Delhi",
                price: 8000,
                type: "single",
                beds: 1,
                baths: 1,
                area: 200,
                description: "Spacious single room with attached bathroom, perfect for students.",
                amenities: ["WiFi", "Furnished", "AC"],
                rating: 4.5,
                owner: "Rajesh Kumar",
                phone: "+91-9876543210",
                images: ["https://images.unsplash.com/photo-1586023492125-27b2c045b917?w=300&h=200&fit=crop", "https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=300&h=200&fit=crop"],
                lat: 28.5355,
                lng: 77.3910,
                amenitiesList: ["WiFi", "Furnished", "AC", "Study Table"]
            },
            {
                id: 2,
                name: "Shared Room with 2 Beds",
                location: "Bangalore",
                price: 5500,
                type: "shared",
                beds: 2,
                baths: 1,
                area: 250,
                description: "Comfortable shared room in a well-maintained PG with friendly roommates.",
                amenities: ["WiFi", "Parking", "Laundry"],
                rating: 4,
                owner: "Priya Sharma",
                phone: "+91-8765432109",
                images: ["https://images.unsplash.com/photo-1522708323590-d24dbb6b0267?w=300&h=200&fit=crop", "https://images.unsplash.com/photo-1552321554-5fefe8c9ef14?w=300&h=200&fit=crop"],
                lat: 12.9716,
                lng: 77.5946,
                amenitiesList: ["WiFi", "Parking", "Laundry", "Common Kitchen"]
            },
            {
                id: 3,
                name: "Premium Double Room",
                location: "Mumbai",
                price: 12000,
                type: "double",
                beds: 2,
                baths: 1,
                area: 300,
                description: "Luxurious double room with modern amenities and city view.",
                amenities: ["WiFi", "Furnished", "AC", "Gym"],
                rating: 5,
                owner: "Arjun Singh",
                phone: "+91-7654321098",
                images: ["https://images.unsplash.com/photo-1631049307264-da0ec9d70304?w=300&h=200&fit=crop", "https://images.unsplash.com/photo-1603808033192-082d6919d3e1?w=300&h=200&fit=crop"],
                lat: 19.0760,
                lng: 72.8777,
                amenitiesList: ["WiFi", "Furnished", "AC", "Gym", "Security"]
            },
            {
                id: 4,
                name: "Apartment with Kitchen",
                location: "Delhi",
                price: 15000,
                type: "apartment",
                beds: 1,
                baths: 1,
                area: 400,
                description: "Independent apartment with separate kitchen and balcony.",
                amenities: ["WiFi", "Furnished", "AC", "Parking"],
                rating: 4.5,
                owner: "Neha Gupta",
                phone: "+91-6543210987",
                images: ["https://images.unsplash.com/photo-1571508601891-f6d1a3f6f05a?w=300&h=200&fit=crop", "https://images.unsplash.com/photo-1512047950186-cc2b41ce233d?w=300&h=200&fit=crop"],
                lat: 28.6139,
                lng: 77.2090,
                amenitiesList: ["WiFi", "Furnished", "AC", "Parking", "Kitchen"]
            },
            {
                id: 5,
                name: "Budget Single Room",
                location: "Hyderabad",
                price: 6000,
                type: "single",
                beds: 1,
                baths: 1,
                area: 150,
                description: "Compact but clean single room ideal for budget-conscious students.",
                amenities: ["WiFi"],
                rating: 3.5,
                owner: "Ravi Patel",
                phone: "+91-5432109876",
                images: ["https://images.unsplash.com/photo-1538108149393-fbbd81895907?w=300&h=200&fit=crop", "https://images.unsplash.com/photo-1522708323590-d24dbb6b0267?w=300&h=200&fit=crop"],
                lat: 17.3850,
                lng: 78.4867,
                amenitiesList: ["WiFi", "Basic Furniture"]
            },
            {
                id: 6,
                name: "Luxury Shared Apartment",
                location: "Pune",
                price: 9000,
                type: "shared",
                beds: 1,
                baths: 1,
                area: 280,
                description: "Premium shared apartment with all modern facilities and 24x7 security.",
                amenities: ["WiFi", "Furnished", "AC", "Gym", "Parking"],
                rating: 5,
                owner: "Vikram Das",
                phone: "+91-4321098765",
                images: ["https://images.unsplash.com/photo-1576540611351-0bda4e7b4f4d?w=300&h=200&fit=crop", "https://images.unsplash.com/photo-1552374196-1ab2748f3d3f?w=300&h=200&fit=crop"],
                lat: 18.5204,
                lng: 73.8567,
                amenitiesList: ["WiFi", "Furnished", "AC", "Gym", "Security", "Parking"]
            }
        ];

        let currentImageIndex = {};
        let map;
        let markers = [];

        // Initialize image indices
        rooms.forEach(room => {
            currentImageIndex[room.id] = 0;
        });

        // Display rooms on page load
        window.addEventListener('load', () => {
            displayRooms(rooms);
            initMap();
        });

        function displayRooms(roomsToDisplay) {
            const grid = document.getElementById('roomGrid');
            
            if (roomsToDisplay.length === 0) {
                grid.innerHTML = '<div class="no-results"><i class="fas fa-search"></i><p>No rooms found matching your criteria. Try adjusting your filters.</p></div>';
                return;
            }

            grid.innerHTML = roomsToDisplay.map(room => `
                <div class="room-card">
                    <div class="room-image">
                        <img src="${room.images[currentImageIndex[room.id]]}" alt="${room.name}">
                        <div class="price-tag">₹${room.price}/mo</div>
                        <div class="gallery-nav">
                            ${room.images.map((_, i) => `
                                <div class="gallery-dot ${i === currentImageIndex[room.id] ? 'active' : ''}" 
                                     onclick="changeImage(${room.id}, ${i})"></div>
                            `).join('')}
                        </div>
                    </div>
                    <div class="room-content">
                        <h3 class="room-title">${room.name}</h3>
                        <div class="room-location">
                            <i class="fas fa-map-marker-alt"></i> ${room.location}
                        </div>
                        <div class="room-details">
                            <div class="detail-item"><i class="fas fa-door-open"></i> ${room.type}</div>
                            <div class="detail-item"><i class="fas fa-bed"></i> ${room.beds} bed${room.beds > 1 ? 's' : ''}</div>
                            <div class="detail-item"><i class="fas fa-bath"></i> ${room.baths} bath</div>
                            <div class="detail-item"><i class="fas fa-ruler-combined"></i> ${room.area} sqft</div>
                        </div>
                        <p class="room-description">${room.description}</p>
                        <div class="room-footer">
                            ${room.amenitiesList.map(a => `<span class="amenity-tag">${a}</span>`).join('')}
                        </div>
                        <div style="margin-bottom: 1rem;">
                            <span style="color: #ff9800;">${'⭐'.repeat(Math.floor(room.rating))}</span>
                            <span style="color: #999; font-size: 0.9rem;">(${room.rating}/5)</span>
                        </div>
                        <div class="room-actions">
                            <button class="btn-contact" onclick="openModal(${room.id}, '${room.name}')">
                                <i class="fas fa-phone"></i> Contact
                            </button>
                            <button class="btn-map" onclick="focusOnMap(${room.id})">
                                <i class="fas fa-map-marker-alt"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function changeImage(roomId, imageIndex) {
            currentImageIndex[roomId] = imageIndex;
            const grid = document.getElementById('roomGrid');
            const dots = grid.querySelectorAll(`[room-id="${roomId}"] .gallery-dot`);
            dots.forEach((dot, i) => {
                dot.classList.toggle('active', i === imageIndex);
            });
            displayRooms(filterRooms(true));
        }

        function filterRooms(displayOnly = false) {
            const location = document.getElementById('locationSearch').value.toLowerCase();
            const maxPrice = parseInt(document.getElementById('priceSearch').value) || 999999;
            const roomType = document.getElementById('typeSearch').value;
            const minPrice = parseInt(document.getElementById('minPrice').value) || 0;
            const maxPriceFilter = parseInt(document.getElementById('maxPrice').value) || 999999;
            
            const filters = {
                wifi: document.getElementById('filter-wifi').checked,
                parking: document.getElementById('filter-parking').checked,
                furnished: document.getElementById('filter-furnished').checked,
                ac: document.getElementById('filter-ac').checked,
                gym: document.getElementById('filter-gym').checked,
                single: document.getElementById('type-single').checked,
                double: document.getElementById('type-double').checked,
                shared: document.getElementById('type-shared').checked,
                apartment: document.getElementById('type-apartment').checked
            };

            const selectedRating = document.querySelector('input[name="rating"]:checked')?.value;

            const filtered = rooms.filter(room => {
                const locationMatch = !location || room.location.toLowerCase().includes(location);
                const priceMatch = room.price <= Math.min(maxPrice, maxPriceFilter) && room.price >= minPrice;
                const typeMatch = !roomType || room.type === roomType;
                
                const amenityMatch = 
                    (!filters.wifi || room.amenities.includes("WiFi")) &&
                    (!filters.parking || room.amenities.includes("Parking")) &&
                    (!filters.furnished || room.amenities.includes("Furnished")) &&
                    (!filters.ac || room.amenities.includes("AC")) &&
                    (!filters.gym || room.amenities.includes("Gym"));

                const typeFiltersActive = Object.values(filters).some((v, i) => i >= 4 && v);
                const typeFilterMatch = !typeFiltersActive || 
                    (filters.single && room.type === 'single') ||
                    (filters.double && room.type === 'double') ||
                    (filters.shared && room.type === 'shared') ||
                    (filters.apartment && room.type === 'apartment');

                const ratingMatch = !selectedRating || room.rating >= parseInt(selectedRating);

                return locationMatch && priceMatch && typeMatch && amenityMatch && typeFilterMatch && ratingMatch;
            });

            if (!displayOnly) {
                displayRooms(filtered);
            }
            return filtered;
        }

        function openModal(roomId, roomName) {
            const room = rooms.find(r => r.id === roomId);
            document.getElementById('ownerName').value = roomName;
            document.getElementById('yourName').value = '';
            document.getElementById('yourEmail').value = '';
            document.getElementById('yourPhone').value = '';
            document.getElementById('message').value = '';
            document.getElementById('contactModal').classList.add('active');
            document.getElementById('contactModal').dataset.roomId = roomId;
        }

        function closeModal() {
            document.getElementById('contactModal').classList.remove('active');
        }

        function submitForm(e) {
            e.preventDefault();
            const roomId = document.getElementById('contactModal').dataset.roomId;
            const room = rooms.find(r => r.id === roomId);
            const formData = {
                yourName: document.getElementById('yourName').value,
                yourEmail: document.getElementById('yourEmail').value,
                yourPhone: document.getElementById('yourPhone').value,
                message: document.getElementById('message').value,
                propertyName: room.name,
                ownerName: room.owner,
                ownerPhone: room.phone
            };

            alert(`Thank you! Your message has been sent to ${room.owner}.\n\nOwner Details:\nName: ${room.owner}\nPhone: ${room.phone}\n\nTheir contact details will reach out to you soon at ${formData.yourPhone}`);
            closeModal();
        }

        function switchView(view) {
            const buttons = document.querySelectorAll('.view-btn');
            buttons.forEach(btn => btn.classList.remove('active'));
            event.target.closest('.view-btn').classList.add('active');

            const grid = document.getElementById('roomGrid');
            const mapSection = document.getElementById('mapSection');

            if (view === 'grid') {
                grid.style.display = 'grid';
                mapSection.classList.remove('active');
            } else {
                grid.style.display = 'none';
                mapSection.classList.add('active');
                if (map) {
                    map.setView([20.5937, 78.9629], 5);
                    updateMapMarkers(filterRooms(true));
                }
            }
        }

        function initMap() {
            map = L.map('map').setView([20.5937, 78.9629], 5);
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© OpenStreetMap contributors',
                maxZoom: 19
            }).addTo(map);
        }

        function updateMapMarkers(roomsToShow) {
            markers.forEach(marker => map.removeLayer(marker));
            markers = [];

            roomsToShow.forEach(room => {
                const marker = L.marker([room.lat, room.lng])
                    .bindPopup(`
                        <div style="font-weight: bold;">${room.name}</div>
                        <div>₹${room.price}/mo</div>
                        <div style="font-size: 0.9rem; color: #666;">${room.location}</div>
                    `)
                    .addTo(map);
                markers.push(marker);
            });
        }

        function focusOnMap(roomId) {
            const room = rooms.find(r => r.id === roomId);
            switchView('map');
            setTimeout(() => {
                map.setView([room.lat, room.lng], 13);
                if (markers.length > 0) {
                    markers.find(m => {
                        if (m.getLatLng().lat === room.lat && m.getLatLng().lng === room.lng) {
                            m.openPopup();
                            return true;
                        }
                    });
                }
            }, 100);
        }

        // Close modal when clicking outside
        window.onclick = function(event) {
            const modal = document.getElementById('contactModal');
            if (event.target === modal) {
                closeModal();
            }
        }
    </script>
</body>
</html>
