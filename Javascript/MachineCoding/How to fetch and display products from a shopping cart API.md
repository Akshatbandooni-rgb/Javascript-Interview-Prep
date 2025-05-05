# How to Fetch and Display Products from a Shopping Cart API

---

## JavaScript Code

```javascript
// Function to fetch cart data
async function fetchProductData() {
  try {
    const response = await fetch("https://fakestoreapi.com/carts?limit=1");
    const carts = await response.json();

    console.log("Fetched carts:", carts);

    // Extract products from all carts using map + spread
    let productsInCart = [];
    carts.forEach((cart) => {
      productsInCart = [...productsInCart, ...cart.products];
    });

    console.log("Products in Cart:", productsInCart);

    await fetchProductDetails(productsInCart);
  } catch (error) {
    console.error("Error fetching cart data:", error.message);
  }
}

// Function to fetch product details
async function fetchProductDetails(products) {
  try {
    const productResponses = await Promise.all(
      products.map((product) =>
        fetch(`https://fakestoreapi.com/products/${product.productId}`)
      )
    );

    const productDetails = await Promise.all(
      productResponses.map((response) => response.json())
    );

    console.log("Product Details:", productDetails);
  } catch (error) {
    console.error("Error fetching product details:", error.message);
  }
}

// Run the function
fetchProductData();
```

---

## Explanation

1. **Fetch Cart Data**:

   - The `fetchProductData` function fetches cart data from the API endpoint `https://fakestoreapi.com/carts?limit=1`.
   - The response is parsed into JSON format, and the cart data is logged to the console.

2. **Extract Products from Cart**:

   - The `forEach` method is used to extract all products from the fetched carts into a single array using the spread operator.

3. **Fetch Product Details**:

   - The `fetchProductDetails` function takes the list of products and fetches details for each product using their `productId`.
   - The `Promise.all` method is used to fetch all product details concurrently.

4. **Log Product Details**:

   - The fetched product details are logged to the console.

5. **Error Handling**:
   - Both functions include `try-catch` blocks to handle errors during the fetch operations.

---

## Output Example

### Console Logs:

```plaintext
Fetched carts: [
  {
    id: 1,
    userId: 1,
    date: "2023-05-05T00:00:00.000Z",
    products: [
      { productId: 1, quantity: 2 },
      { productId: 2, quantity: 1 }
    ]
  }
]
Products in Cart: [
  { productId: 1, quantity: 2 },
  { productId: 2, quantity: 1 }
]
Product Details: [
  {
    id: 1,
    title: "Product 1",
    price: 29.99,
    description: "Description of Product 1",
    category: "electronics",
    image: "https://example.com/product1.jpg"
  },
  {
    id: 2,
    title: "Product 2",
    price: 49.99,
    description: "Description of Product 2",
    category: "fashion",
    image: "https://example.com/product2.jpg"
  }
]
```

---

## Features

1. **Fetch Cart Data**:

   - Retrieves cart data from the API.

2. **Extract Products**:

   - Extracts all products from the cart using `forEach` and the spread operator.

3. **Fetch Product Details**:

   - Fetches detailed information for each product in the cart.

4. **Error Handling**:
   - Handles errors gracefully and logs them to the console.

---

## How to Use

1. Copy the code above and save it as a `.js` file (e.g., `fetchProducts.js`).
2. Run the file in a Node.js environment or include it in an HTML file with a `<script>` tag.
3. Open the browser console to view the output.

---

Let me know if you'd like further refinements or additional examples!
