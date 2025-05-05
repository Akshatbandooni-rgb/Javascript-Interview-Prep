# Simple CRUD with JavaScript

---

## HTML Code

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Simple CRUD with JS</title>
  </head>
  <body>
    <h2>Add Post</h2>
    <form id="post-form">
      <input id="title" placeholder="Title" required /><br />
      <textarea id="body" placeholder="Body" required></textarea><br />
      <button type="submit">Add</button>
    </form>

    <h2>Posts</h2>
    <ul id="posts"></ul>

    <script>
      const postList = document.getElementById("posts");
      const form = document.getElementById("post-form");

      // Fetch posts (READ)
      fetch("https://jsonplaceholder.typicode.com/posts?_limit=5")
        .then((res) => res.json())
        .then((posts) => posts.forEach(addPostToDOM));

      // Add post to DOM
      function addPostToDOM(post) {
        const li = document.createElement("li");
        const span = document.createElement("span");
        span.textContent = post.title;
        li.appendChild(span);

        // Edit button
        const editBtn = document.createElement("button");
        editBtn.textContent = "Edit";
        editBtn.onclick = () => {
          const newTitle = prompt("Enter new title:", span.textContent);
          if (newTitle) {
            fetch(`https://jsonplaceholder.typicode.com/posts/${post.id}`, {
              method: "PUT",
              headers: { "Content-Type": "application/json" },
              body: JSON.stringify({ ...post, title: newTitle }),
            })
              .then((res) => res.json())
              .then((updated) => {
                span.textContent = updated.title;
              });
          }
        };

        // Delete button
        const delBtn = document.createElement("button");
        delBtn.textContent = "Delete";
        delBtn.onclick = () => {
          fetch(`https://jsonplaceholder.typicode.com/posts/${post.id}`, {
            method: "DELETE",
          }).then(() => li.remove());
        };

        li.appendChild(editBtn);
        li.appendChild(delBtn);
        postList.appendChild(li);
      }

      // Handle form submit (CREATE)
      form.onsubmit = function (e) {
        e.preventDefault();
        const title = document.getElementById("title").value;
        const body = document.getElementById("body").value;

        fetch("https://jsonplaceholder.typicode.com/posts", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ title, body, userId: 1 }),
        })
          .then((res) => res.json())
          .then((post) => {
            addPostToDOM(post);
            form.reset();
          });
      };
    </script>
  </body>
</html>
```

---

## Features

1. **Create**:

   - Add a new post by filling out the form and clicking the "Add" button.
   - The new post is sent to the API and added to the DOM.

2. **Read**:

   - Fetches the first 5 posts from the API and displays them in a list.

3. **Update**:

   - Edit a post's title by clicking the "Edit" button.
   - Prompts the user for a new title and updates the post both in the API and the DOM.

4. **Delete**:
   - Delete a post by clicking the "Delete" button.
   - Removes the post from the API and the DOM.

---

## How to Use

1. Copy the code above and save it as an `.html` file (e.g., `crud.html`).
2. Open the file in a browser.
3. Use the form to add posts, edit existing posts, or delete them.
4. The app interacts with the [JSONPlaceholder API](https://jsonplaceholder.typicode.com/) for demonstration purposes.
