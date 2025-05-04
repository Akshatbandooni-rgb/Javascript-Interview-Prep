# Form Validation Example

---

## HTML Code

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        font-family: sans-serif;
        margin: 0;
        background: #f0f0f0;
      }

      form {
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
        padding: 1rem;
        background: white;
        border: 1px solid #ccc;
        border-radius: 4px;
        width: 260px;
      }

      input,
      button {
        padding: 0.5rem;
        font-size: 1rem;
      }

      .error {
        color: red;
        font-size: 0.85rem;
      }
    </style>
  </head>
  <body>
    <form id="loginForm">
      <label>Email:</label>
      <input type="text" id="email" />
      <div id="emailError" class="error"></div>

      <label>Password:</label>
      <input type="password" id="password" />
      <div id="passwordError" class="error"></div>

      <button type="submit">Login</button>
    </form>

    <script>
      document
        .getElementById("loginForm")
        .addEventListener("submit", function (e) {
          e.preventDefault(); // prevent page reload

          const email = document.getElementById("email").value.trim();
          const password = document.getElementById("password").value.trim();
          const emailError = document.getElementById("emailError");
          const passwordError = document.getElementById("passwordError");
          const emailRegex = /^[^@\s]+@[^@\s]+\.[^@\s]+$/;

          let valid = true;

          // Email Validation
          if (!email) {
            emailError.textContent = "Email is required";
            valid = false;
          } else if (!emailRegex.test(email)) {
            emailError.textContent = "Invalid email format";
            valid = false;
          } else {
            emailError.textContent = "";
          }

          // Password Validation
          if (!password) {
            passwordError.textContent = "Password is required";
            valid = false;
          } else if (password.length < 6) {
            passwordError.textContent = "Min 6 characters required";
            valid = false;
          } else {
            passwordError.textContent = "";
          }

          if (valid) {
            alert("Login successful");
          }
        });
    </script>
  </body>
</html>
```

---

## Features

1. **Email Validation**:

   - Ensures the email field is not empty.
   - Validates the email format using a regular expression.

2. **Password Validation**:

   - Ensures the password field is not empty.
   - Checks that the password has a minimum of 6 characters.

3. **Error Messages**:

   - Displays error messages below the respective fields if validation fails.

4. **Success Alert**:
   - Displays a success alert if all validations pass.

---

## How to Use

1. Copy the code above and save it as an `.html` file (e.g., `form-validation.html`).
2. Open the file in a browser.
3. Enter an email and password to test the validation logic.
