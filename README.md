<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Art Showroom - Javeria Artology</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Quicksand:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body { 
            font-family: 'Quicksand', sans-serif; 
            margin: 0;
            padding: 0; 
            background: 
                radial-gradient(circle, rgba(139, 69, 19, 0.1) 1px, transparent 1px), 
                linear-gradient(#fce4ec, #ffb3ba), 
                url('https://images.unsplash.com/photo-1518709268805-4e9042af2176?w=1920');
            background-size: 20px 20px, cover, cover; 
            background-attachment: fixed; 
            color: #4a4a4a; 
        }
        header {
            background: rgba(255, 179, 186, 0.9); color: #fff; padding: 1rem; text-align: center; border-radius: 0 0 20px 20px; 
        }
        .header-content { display: flex; justify-content: space-between; align-items: center; }
        .header-left h1 { margin: 0; font-family: 'Dancing Script', cursive; font-size: 2rem; }
        .header-right { font-size: 1.2rem; font-weight: bold; font-family: 'Dancing Script', cursive; }
        nav { margin-top: 1rem; display: flex; justify-content: center; }
        nav a { color: #fff; margin: 0 1rem; text-decoration: none; padding: 0.5rem; border-radius: 20px; background: rgba(255,255,255,0.2); }
        .top-controls { display: flex; justify-content: center; align-items: center; margin-top: 1rem; gap: 1rem; }
        .top-controls input { padding: 0.5rem; border: 1px solid #ffb3ba; border-radius: 20px; width: 200px; }
        .top-controls button { padding: 0.5rem 1rem; background: rgba(255,255,255,0.2); color: #fff; border: none; cursor: pointer; border-radius: 20px; font-weight: bold; }
        
        .gallery { display: flex; flex-wrap: wrap; justify-content: center; padding: 2rem; }
        .gallery.list-view .art-piece { width: 100%; max-width: none; display: flex; align-items: center; margin: 0.5rem 0; padding: 1rem; }
        .gallery.list-view .art-piece img { width: 100px; height: 100px; margin-right: 1rem; }
        .gallery.list-view .art-piece .art-info { flex-grow: 1; text-align: left; }
        
        .art-piece { margin: 1rem; text-align: center; width: 300px; background: rgba(255,255,255,0.9); padding: 1rem; border-radius: 15px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        .art-piece img { width: 100%; height: 200px; object-fit: cover; border: 2px solid #ffb3ba; border-radius: 10px; cursor: zoom-in; transition: transform 0.3s; }
        .art-piece img:hover { transform: scale(1.02); }
        
        /* POPUP MODAL STYLES */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background-color: rgba(0,0,0,0.8);
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            max-width: 90%;
            max-height: 80%;
            border-radius: 15px;
            border: 5px solid white;
        }
        .close-modal {
            position: absolute;
            top: 20px;
            right: 30px;
            color: white;
            font-size: 40px;
            font-weight: bold;
            cursor: pointer;
        }

        .art-info p { margin: 5px 0; }
        .art-details { font-size: 0.85rem; color: #777; font-style: italic; }

        .quantity { display: flex; align-items: center; justify-content: center; margin: 0.5rem 0; }
        .quantity button { width: 30px; height: 30px; background: #ff9a9e; color: white; border: none; cursor: pointer; border-radius: 50%; font-size: 1.2rem; }
        .quantity span { margin: 0 1rem; font-weight: bold; }
        .add-btn { margin-top: 0.5rem; padding: 0.5rem 1.5rem; background: #ff9a9e; color: white; border: none; cursor: pointer; border-radius: 20px; font-weight: bold; }
        
        .about, .contact, .cart, .showcase, .custom-box { padding: 2rem; max-width: 800px; margin: 2rem auto; background: rgba(255,255,255,0.9); border-radius: 20px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        .custom-box { border: 3px dashed #ff9a9e; text-align: center; }
        .about h2, .contact h2, .cart h2, .showcase h2, .custom-box h2 { font-family: 'Dancing Script', cursive; color: #e91e63; }
        
        .whatsapp-btn { display: inline-block; background-color: #25d366; color: white; padding: 10px 20px; text-decoration: none; border-radius: 25px; font-weight: bold; margin-top: 10px; }
        form { display: flex; flex-direction: column; }
        input, textarea { margin-bottom: 1rem; padding: 0.5rem; border: 1px solid #ffb3ba; border-radius: 10px; }
        .cart-item { display: flex; justify-content: space-between; align-items: center; margin-bottom: 0.5rem; padding: 0.5rem; background: #fce4ec; border-radius: 10px; }
        
        .slideshow { position: relative; width: 100%; max-width: 500px; margin: auto; overflow: hidden; border-radius: 15px; }
        .slideshow img { width: 100%; height: 300px; object-fit: cover; display: none; }
        .slideshow img.active { display: block; }
        
        footer { text-align: center; padding: 1.5rem; background: rgba(255, 179, 186, 0.9); color: #fff; font-family: 'Dancing Script', cursive; border-radius: 20px 20px 0 0; }

        @media (max-width: 768px) {
            nav { flex-direction: column; }
            .art-piece { width: 90%; }
            .header-content { flex-direction: column; }
        }
    </style>
</head>
<body>

    <div id="imageModal" class="modal" onclick="closeZoom()">
        <span class="close-modal">&times;</span>
        <img class="modal-content" id="zoomedImg">
    </div>

    <header>
        <div class="header-content">
            <div class="header-left"><h1>Welcome to Our Art Showroom 💖</h1></div>
            <div class="header-right">Javeria Artology Store 🌸</div>
        </div>
        <nav>
            <a href="#gallery">Gallery</a>
            <a href="#custom-order">Customized Art</a>
            <a href="#about">About</a>
            <a href="#contact">Contact</a>
            <a href="#cart">Cart</a>
        </nav>
        <div class="top-controls">
            <input type="text" id="searchInput" placeholder="Search art...">
            <button id="listViewBtn">📋 List View</button>
            <button id="cartBtn">🛒 Cart (<span id="cartCount">0</span>)</button>
        </div>
    </header>

    <section id="gallery" class="gallery">
        <div class="art-piece">
            <img src="https://raw.githubusercontent.com/javeriamedical-cloud/artshowroom/main/WhatsApp%20Image%202026-02-09%20at%204.55.05%20PM.jpeg" alt="Acrylic sunset Painting" onclick="openZoom(this.src)">
            <div class="art-info">
                <p><strong>Acrylic sunset Painting</strong></p>
                <p class="art-details">Canvas: 8x8 inch | Varnish Coated ✨</p>
                <p>RS. 1000</p>
            </div>
            <div class="quantity">
                <button onclick="changeQuantity(this, -1)">-</button>
                <span>1</span>
                <button onclick="changeQuantity(this, 1)">+</button>
            </div>
            <button class="add-btn" onclick="addToCart('Acrylic sunset Painting', 1000, this.previousElementSibling.querySelector('span').textContent)">Add to Cart</button>
        </div>

        <div class="art-piece">
            <img src="https://raw.githubusercontent.com/javeriamedical-cloud/artshowroom/main/bubble.jpeg" alt="Acrylic bubble painting" onclick="openZoom(this.src)">
            <div class="art-info">
                <p><strong>Acrylic bubble painting</strong></p>
                <p class="art-details">Canvas: 8x8 inch | Varnish Coated ✨</p>
                <p>RS. 1000</p>
            </div>
            <div class="quantity">
                <button onclick="changeQuantity(this, -1)">-</button>
                <span>1</span>
                <button onclick="changeQuantity(this, 1)">+</button>
            </div>
            <button class="add-btn" onclick="addToCart('Acrylic bubble painting', 1000, this.previousElementSibling.querySelector('span').textContent)">Add to Cart</button>
        </div>

        <div class="art-piece">
            <img src="https://raw.githubusercontent.com/javeriamedical-cloud/artshowroom/main/candle.jpeg" alt="Acrylic realistic rain painting" onclick="openZoom(this.src)">
            <div class="art-info">
                <p><strong>Acrylic realistic rain painting</strong></p>
                <p class="art-details">Canvas: 8x8 inch | Varnish Coated ✨</p>
                <p>RS. 1000</p>
            </div>
            <div class="quantity">
                <button onclick="changeQuantity(this, -1)">-</button>
                <span>1</span>
                <button onclick="changeQuantity(this, 1)">+</button>
            </div>
            <button class="add-btn" onclick="addToCart('Acrylic realistic rain painting', 1000, this.previousElementSibling.querySelector('span').textContent)">Add to Cart</button>
        </div>
    </section>

    <section id="custom-order" class="custom-box">
        <h2>Customized Painting 🎨</h2>
        <p>Do you have a specific idea or a favorite photo you'd like painted?</p>
        <p><strong>I offer personalized art tailored to your requirements!</strong></p>
        <a href="https://wa.me/923181133708?text=Hi!%20I'm%20interested%20in%20ordering%20a%20customized%20painting." target="_blank" class="whatsapp-btn">
            📱 Message me on WhatsApp for Custom Orders
        </a>
    </section>

    <section id="about" class="about">
        <h2>About Us</h2>
        <p>We showcase contemporary art from emerging and established artists. Our showroom features paintings, sculptures, and digital works.</p>
        <p><strong>Delivery time: 5 to 7 days. Buy above RS. 2000 and get free delivery.</strong></p>
        <p><strong>Business Hours: 10am - 6pm Monday to Sunday</strong></p>
    </section>

    <section id="contact" class="contact">
        <h2>Contact Us</h2>
        <p>Phone: 0318-1133708</p>
        <p><a href="https://wa.me/923181133708" target="_blank" style="color: #25d366; text-decoration: none; font-weight: bold;">📱 WhatsApp Us</a></p>
        <form id="contactForm">
            <input type="text" placeholder="Your Name" required>
            <input type="email" placeholder="Your Email" required>
            <textarea placeholder="Your Message" required></textarea>
            <button type="submit" style="background: #ff9a9e; color: white; border: none; padding: 10px; border-radius: 20px; cursor: pointer;">Send Message</button>
        </form>
    </section>

    <section id="cart" class="cart">
        <h2>Your Shopping Cart</h2>
        <div id="cartItems"></div>
        <p><strong>Total: RS. <span id="cartTotal">0</span></strong></p>
        <button onclick="checkout()" style="width: 100%; padding: 10px; background: #e91e63; color: white; border: none; border-radius: 20px; cursor: pointer; font-weight: bold;">Checkout via WhatsApp</button>
        <p style="text-align:center; margin-top:1rem; font-weight:bold; color:#e91e63;">Thank you for shopping, visit again 💖</p>
    </section>

    <section id="showcase" class="showcase">
        <h2>Art Showcase 🎨</h2>
        <div class="slideshow">
            <img src="https://raw.githubusercontent.com/javeriamedical-cloud/artshowroom/main/WhatsApp%20Image%202026-02-09%20at%204.55.05%20PM.jpeg" class="active">
            <img src="https://raw.githubusercontent.com/javeriamedical-cloud/artshowroom/main/bubble.jpeg">
            <img src="https://raw.githubusercontent.com/javeriamedical-cloud/artshowroom/main/candle.jpeg">
        </div>
    </section>

    <footer>
        Dream big, create bigger! 💕 Javeria Artology Store<br>
        Email: javeriamedical@gmail.com
    </footer>

    <script>
        // NEW: ZOOM FUNCTIONS
        function openZoom(imgSrc) {
            document.getElementById("imageModal").style.display = "flex";
            document.getElementById("zoomedImg").src = imgSrc;
        }

        function closeZoom() {
            document.getElementById("imageModal").style.display = "none";
        }

        // ORIGINAL LOGIC (Remains exactly as you had it)
        let cart = JSON.parse(localStorage.getItem('cart')) || [];
        updateCartDisplay();

        function changeQuantity(button, delta) {
            const span = button.parentElement.querySelector('span');
            let qty = parseInt(span.textContent);
            qty = Math.max(1, qty + delta);
            span.textContent = qty;
        }

        function addToCart(name, price, qty) {
            qty = parseInt(qty);
            const existing = cart.find(item => item.name === name);
            if (existing) { existing.qty += qty; } 
            else { cart.push({ name, price, qty }); }
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCartDisplay();
            alert(`${qty} x ${name} added to cart! 💖`);
        }

        function updateCartDisplay() {
            const cartItems = document.getElementById('cartItems');
            const cartTotal = document.getElementById('cartTotal');
            const cartCount = document.getElementById('cartCount');
            cartItems.innerHTML = '';
            let total = 0, count = 0;
            cart.forEach((item, index) => {
                const div = document.createElement('div');
                div.className = 'cart-item';
                div.innerHTML = `
                    <span>${item.name} (x${item.qty})</span>
                    <span>RS. ${item.price * item.qty}</span>
                    <button onclick="removeFromCart(${index})" style="background:none; border:none; color:red; cursor:pointer;">❌</button>
                `;
                cartItems.appendChild(div);
                total += item.price * item.qty;
                count += item.qty;
            });
            cartTotal.textContent = total;
            cartCount.textContent = count;
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCartDisplay();
        }

        function checkout() {
            if (cart.length === 0) { alert('Your cart is empty! 😊'); return; }
            let message = 'Hi, I want to order:\n';
            cart.forEach(item => { message += `- ${item.name} (Qty: ${item.qty}) - RS.${item.price * item.qty}\n`; });
            message += `Total: RS.${document.getElementById('cartTotal').textContent}\nPlease confirm delivery details.`;
            window.open(`https://wa.me/923181133708?text=${encodeURIComponent(message)}`, '_blank');
            cart = [];
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCartDisplay();
        }

        let slideIndex = 0;
        const slides = document.querySelectorAll('.slideshow img');
        setInterval(() => {
            slides[slideIndex].classList.remove('active');
            slideIndex = (slideIndex + 1) % slides.length;
            slides[slideIndex].classList.add('active');
        }, 3000);

        document.getElementById('searchInput').addEventListener('input', function() {
            const query = this.value.toLowerCase();
            document.querySelectorAll('.art-piece').forEach(piece => {
                const text = piece.innerText.toLowerCase();
                piece.style.display = text.includes(query) ? 'block' : 'none';
            });
        });

        document.getElementById('listViewBtn').addEventListener('click', function() {
            const gallery = document.querySelector('.gallery');
            gallery.classList.toggle('list-view');
            this.textContent = gallery.classList.contains('list-view') ? "🖼 Grid View" : "📋 List View";
        });
    </script>
</body>
</html>
