# Fetch Data and Render Details

---

## HTML Code

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Fetch Data and Render Details</title>
    <style>
      body {
        font-family: sans-serif;
        padding: 1rem;
        background: #f0f0f0;
      }
      .box {
        background: #fff;
        padding: 1rem;
        margin-bottom: 1rem;
        border-radius: 6px;
        height: 400px;
        overflow-y: scroll;
      }
      ul {
        padding: 0;
        list-style: none;
      }
      li {
        padding: 0.5rem;
        margin-bottom: 0.5rem;
        background: #f9f9f9;
        border-radius: 4px;
        cursor: pointer;
      }
      li:hover {
        background: #e0f7fa;
      }
      li.active {
        background: #b2ebf2;
      }
      img {
        max-width: 120px;
        border-radius: 4px;
        margin-bottom: 0.5rem;
      }
      .price {
        font-weight: bold;
        color: green;
      }
    </style>
  </head>
  <body>
    <div class="box">
      <h2>Products</h2>
      <div id="loading">Loading...</div>
      <ul id="products-ul"></ul>
    </div>

    <div class="box" id="product-details">
      <h2>Product Details</h2>
      <p>Select a product to view details</p>
    </div>

    <script>
      const list = document.getElementById("products-ul");
      const details = document.getElementById("product-details");
      const loading = document.getElementById("loading");
      let active = null;

      fetch("https://fakestoreapi.com/products")
        .then((res) => res.json())
        .then((products) => {
          loading.remove();
          products.forEach((p) => {
            const li = document.createElement("li");
            li.textContent = p.title;
            li.onclick = () => {
              if (active) active.classList.remove("active");
              li.classList.add("active");
              active = li;
              details.innerHTML = `
              <h2>${p.title}</h2>
              <img src="${p.image}" alt="${p.title}">
              <p class="price">$${p.price}</p>
              <p><b>Category:</b> ${p.category}</p>
              <p>${p.description}</p>
            `;
            };
            list.appendChild(li);
          });
        })
        .catch(() => (loading.textContent = "Failed to load"));
    </script>
  </body>
</html>
```
