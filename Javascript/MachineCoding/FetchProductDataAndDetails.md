# Product Viewer

---

## HTML Code

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Product Viewer</title>

    <!-- [START] Styles -->
    <style>
      body {
        display: flex;
        flex-direction: column;
        gap: 10px;
        font-family: sans-serif;
        margin: 0;
        padding: 0;
      }

      .active {
        background-color: aqua;
        color: black;
      }

      .box {
        margin: 20px;
        padding: 20px;
        border: solid 1px black;
        background-color: #f9f9f9;
        border-radius: 6px;
      }

      ul {
        list-style: none;
        padding: 0;
      }

      li {
        padding: 5px;
        margin-bottom: 5px;
        cursor: pointer;
        border-radius: 4px;
      }

      li:hover {
        background-color: #e0f7fa;
      }
    </style>
    <!-- [END] Styles -->
  </head>

  <body>
    <!-- [START] Product List Section -->
    <div class="box product-list" id="product-list">
      <header>
        <h2>Product List</h2>
      </header>
      <ul id="product-ul"></ul>
    </div>
    <!-- [END] Product List Section -->

    <!-- [START] Product Details Section -->
    <div class="box product-details" id="product-details">
      <header>
        <h2>Product Details</h2>
      </header>
      <section>
        <p>Click a product above to see its details</p>
      </section>
    </div>
    <!-- [END] Product Details Section -->

    <!-- [START] JavaScript Logic -->
    <script>
      // DOM references
      const productUl = document.getElementById("product-ul");
      const productDetails = document.getElementById("product-details");

      let active = null;

      // Fetch top 5 products
      fetch("https://fakestoreapi.com/products?limit=5")
        .then((res) => res.json())
        .then((products) => {
          products.forEach((product) => {
            const li = document.createElement("li");
            li.textContent = product.title;

            // Add click handler for showing product details
            li.onclick = () => {
              // Remove active class from previous item
              if (active) {
                active.classList.remove("active");
              }

              // Add active class to current item
              li.classList.add("active");
              active = li;

              // Fetch and render details of selected product
              fetch(`https://fakestoreapi.com/products/${product.id}`)
                .then((res) => res.json())
                .then((productDetailsResponse) => {
                  productDetails.innerHTML = `
                  <header><h2>Product Details</h2></header>
                  <section>
                    <p><strong>Title:</strong> ${productDetailsResponse.title}</p>
                    <p><strong>Price:</strong> $${productDetailsResponse.price}</p>
                    <p><strong>Category:</strong> ${productDetailsResponse.category}</p>
                    <p><strong>Description:</strong> ${productDetailsResponse.description}</p>
                  </section>
                `;
                })
                .catch(() => {
                  productDetails.innerHTML =
                    "<p>Failed to load product details.</p>";
                });
            };

            productUl.appendChild(li);
          });
        })
        .catch((err) => {
          console.error("Failed to load products:", err.message);
          productUl.innerHTML = "<li>Failed to load products</li>";
        });
    </script>
    <!-- [END] JavaScript Logic -->
  </body>
</html>
```

---

## Features

1. **Dynamic Product List**:

   - Fetches the top 5 products from the API and displays them in a list.

2. **Interactive Product Details**:

   - Clicking on a product fetches and displays its details, including title, price, category, and description.

3. **Error Handling**:

   - Displays an error message if the product list or product details fail to load.

4. **Active State**:
   - Highlights the currently selected product in the list.

---

## How to Use

1. Copy the code above and save it as an `.html` file (e.g., `product-viewer.html`).
2. Open the file in a browser.
3. Click on a product to view its details.
