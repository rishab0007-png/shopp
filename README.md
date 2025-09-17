<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Product Catalog</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      background-color: #f9f9f9;
    }
    .product-container {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
    }
    .product-card {
      background: white;
      border: 1px solid #ddd;
      border-radius: 5px;
      width: 250px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      overflow: hidden;
    }
    .product-image img {
      width: 100%;
      height: auto;
      display: block;
    }
    .product-details {
      padding: 15px;
    }
    .product-name {
      font-size: 18px;
      font-weight: bold;
      margin: 0 0 10px 0;
    }
    .product-specs {
      font-size: 14px;
      color: #555;
    }
  </style>
</head>
<body>

  <h1>Our Products</h1>
  <div class="product-container">
    <div class="product-card">
      <div class="product-image">
        <img src="attached_image:1" alt="Product 1" />
      </div>
      <div class="product-details">
        <h2 class="product-name">Product 1</h2>
        <p class="product-specs">
          Specification A: Value 1<br />
          Specification B: Value 2<br />
          Specification C: Value 3
        </p>
      </div>
    </div>
    <div class="product-card">
      <div class="product-image">
        <img src="attached_image:2" alt="Product 2" />
      </div>
      <div class="product-details">
        <h2 class="product-name">Product 2</h2>
        <p class="product-specs">
          Specification A: Value 4<br />
          Specification B: Value 5<br />
          Specification C: Value 6
        </p>
      </div>
    </div>
    <div class="product-card">
      <div class="product-image">
        <img src="attached_image:3" alt="Product 3" />
      </div>
      <div class="product-details">
        <h2 class="product-name">Product 3</h2>
        <p class="product-specs">
          Specification A: Value 7<br />
          Specification B: Value 8<br />
          Specification C: Value 9
        </p>
      </div>
    </div>
  </div>
</body>
</html>
