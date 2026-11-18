
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ShopEase - Online Shopping</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    background: #f5f5f5;
    color: #333;
}

header {
    background: #ff6600;
    color: white;
    padding: 20px 7%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 15px;
}

header h1 {
    font-size: 28px;
}

nav a {
    color: white;
    text-decoration: none;
    margin-left: 20px;
    font-weight: bold;
}

.search {
    padding: 12px;
    width: 100%;
    max-width: 400px;
    border: none;
    border-radius: 5px;
}

.hero {
    background: linear-gradient(120deg, #ff6600, #ff9933);
    color: white;
    text-align: center;
    padding: 70px 20px;
}

.hero h2 {
    font-size: 38px;
    margin-bottom: 15px;
}

.hero p {
    font-size: 18px;
}

.container {
    padding: 40px 7%;
}

.container h2 {
    margin-bottom: 25px;
}

.products {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
    gap: 25px;
}

.product {
    background: white;
    padding: 15px;
    border-radius: 10px;
    text-align: center;
    box-shadow: 0 2px 8px #ddd;
}

.product img {
    width: 100%;
    height: 180px;
    object-fit: contain;
    margin-bottom: 15px;
}

.product h3 {
    margin-bottom: 10px;
}

.price {
    color: #ff6600;
    font-weight: bold;
    font-size: 20px;
    margin: 10px;
}

button {
    background: #ff6600;
    color: white;
    border: none;
    padding: 12px 18px;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background: #e65c00;
}

.cart {
    background: white;
    padding: 25px;
    margin-top: 30px;
    border-radius: 10px;
}

.cart-item {
    display: flex;
    justify-content: space-between;
    padding: 12px 0;
    border-bottom: 1px solid #ddd;
    gap: 10px;
}

footer {
    background: #222;
    color: white;
    text-align: center;
    padding: 25px;
    margin-top: 40px;
}
</style>
</head>

<body>

<header>
    <h1>ShopEase</h1>
    <input
        class="search"
        type="text"
        id="search"
        placeholder="Search products..."
        oninput="searchProducts()"
    >
    <nav>
        <a href="#home">Home</a>
        <a href="#products">Products</a>
        <a href="#cart">Cart 🛒 (<span id="count">0</span>)</a>
    </nav>
</header>

<section class="hero" id="home">
    <h2>Welcome to ShopEase!</h2>
    <p>Discover amazing products at affordable prices.</p>
</section>

<section class="container" id="products">
    <h2>Our Products</h2>
    <div class="products" id="product-list"></div>
</section>

<section class="container" id="cart">
    <h2>🛒 Shopping Cart</h2>
    <div class="cart">
        <div id="cart-items">Your cart is empty.</div>
        <h3 id="total" style="margin:20px 0;">Total: ₱0</h3>
        <button onclick="checkout()">Checkout</button>
    </div>
</section>

<footer>
    <p>© 2026 ShopEase. All Rights Reserved.</p>
    <p>Your favorite online shopping destination!</p>
</footer>

<script>
const products = [
    {
        name: "Wireless Headphones",
        price: 850,
        image: "https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=400"
    },
    {
        name: "Smart Watch",
        price: 1200,
        image: "https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=400"
    },
    {
        name: "Running Shoes",
        price: 1500,
        image: "https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=400"
    },
    {
        name: "Backpack",
        price: 950,
        image: "https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=400"
    },
    {
        name: "Camera",
        price: 2500,
        image: "https://images.unsplash.com/photo-1516035069371-29a1b244cc32?w=400"
    },
    {
        name: "Sunglasses",
        price: 500,
        image: "https://images.unsplash.com/photo-1511499767150-a48a237f0083?w=400"
    }
];

let cart = [];

function displayProducts(items = products) {
    const list = document.getElementById("product-list");
    list.innerHTML = "";

    items.forEach((product) => {
        list.innerHTML += `
            <div class="product">
                <img src="${product.image}" alt="${product.name}">
                <h3>${product.name}</h3>
                <p class="price">₱${product.price.toLocaleString()}</p>
                <button onclick="addToCart('${product.name}')">
                    Add to Cart
                </button>
            </div>
        `;
    });
}

function addToCart(name) {
    const product = products.find(p => p.name === name);
    const existing = cart.find(item => item.name === name);

    if (existing) {
        existing.quantity++;
    } else {
        cart.push({...product, quantity: 1});
    }

    updateCart();
}

function updateCart() {
    const cartItems = document.getElementById("cart-items");
    let total = 0;
    let count = 0;

    if (cart.length === 0) {
        cartItems.innerHTML = "Your cart is empty.";
    } else {
        cartItems.innerHTML = cart.map((item, index) => {
            total += item.price * item.quantity;
            count += item.quantity;

            return `
                <div class="cart-item">
                    <span>
                        ${item.name} x ${item.quantity}
                        - ₱${(item.price * item.quantity).toLocaleString()}
                    </span>
                    <button onclick="removeItem(${index})">
                        Remove
                    </button>
                </div>
            `;
        }).join("");
    }

    document.getElementById("total").innerText =
        "Total: ₱" + total.toLocaleString();

    document.getElementById("count").innerText = count;
}

function removeItem(index) {
    cart.splice(index, 1);
    updateCart();
}

function searchProducts() {
    const keyword = document.getElementById("search")
        .value.toLowerCase();

    const filtered = products.filter(product =>
        product.name.toLowerCase().includes(keyword)
    );

    displayProducts(filtered);
}

function checkout() {
    if (cart.length === 0) {
        alert("Your shopping cart is empty!");
        return;
    }

    const total = cart.reduce(
        (sum, item) => sum + item.price * item.quantity, 0
    );

    alert(
        "Thank you for shopping at ShopEase!\n" +
        "Your total is ₱" + total.toLocaleString() +
        "\nThis is a demo checkout."
    );

    cart = [];
    updateCart();
}

displayProducts();
</script>

</body>
</html>
