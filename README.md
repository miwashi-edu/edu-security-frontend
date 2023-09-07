# edu-web-security-frontend

## Instructions

```bash
cd ~
cd ws
mdkdir web-security-frontend
cd web-security-frontend
touch index.html
npm install -g http-server
http-server
```
## index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Simple Auth Frontend</title>
</head>
<body>
  <h1>Login</h1>
  <form id="login-form">
    <input type="text" id="username" placeholder="Username" required>
    <input type="password" id="password" placeholder="Password" required>
    <button type="submit">Login</button>
  </form>
  <hr>
  <h1>Secret Info</h1>
  <div id="secret-info"></div>

  <script>
    const loginForm = document.getElementById('login-form');
    const secretInfo = document.getElementById('secret-info');

    loginForm.addEventListener('submit', async (e) => {
      e.preventDefault();

      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;

      try {
        const response = await fetch('/login', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ username, password }),
        });

        if (!response.ok) {
          throw new Error('Authentication failed.');
        }

        const { accessToken } = await response.json();

        // Use the received access token to access the protected endpoint
        const infoResponse = await fetch('/info', {
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${accessToken}`,
          },
        });

        if (!infoResponse.ok) {
          throw new Error('Unauthorized.');
        }

        const { message } = await infoResponse.json();
        secretInfo.textContent = message;
      } catch (error) {
        console.error(error.message);
        secretInfo.textContent = 'Login failed or Unauthorized.';
      }
    });
  </script>
</body>
</html>
```
