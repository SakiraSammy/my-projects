
https://api.darululoom-islamia.org/api/notice



html file(index.html)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Login</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h2>Login</h2>
    <form id="loginForm">
      <input type="text" id="username" placeholder="Username" required />
      <input type="password" id="password" placeholder="Password" required />
      <button type="submit">Login</button>
    </form>
    <!-- <p>Default user: <strong>admin / 1234</strong></p> -->
  </div>

  <script src="script.js"></script>
</body>
</html>
--------------------------------------------------------------------

(dashboard.html)
<!-- dashboard.html (User CRUD Page) -->
 <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>User Dashboard</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h2>User Dashboard</h2>
    <button onclick="logout()" class="logout">Logout</button>

    <form id="userForm">
      <input type="text" id="name" placeholder="Name" required />
      <input type="email" id="email" placeholder="Email" required />
      <button type="submit">Add User</button>
    </form>

    <ul id="userList"></ul>
  </div>

  <script src="script.js"></script>
</body>
</html>
----------------------------------------------------------
(script.js)
// script.js (Login + CRUD Logic)
// Default login credentials
const defaultUser = { username: 'admin', password: '1234' };

// Save default login if not already present
if (!localStorage.getItem('loginUser')) {
  localStorage.setItem('loginUser', JSON.stringify(defaultUser));
}

// Check if user is logged in
if (location.pathname.includes('dashboard.html') && !localStorage.getItem('loggedIn')) {
  location.href = 'index.html';
}

// LOGIN Logic
const loginForm = document.getElementById('loginForm');
if (loginForm) {
  loginForm.onsubmit = function (e) {
    e.preventDefault();
    const username = document.getElementById('username').value.trim();
    const password = document.getElementById('password').value.trim();
    const saved = JSON.parse(localStorage.getItem('loginUser'));

    if (username === saved.username && password === saved.password) {
      localStorage.setItem('loggedIn', 'true');
      location.href = 'dashboard.html';
    } else {
      alert('Invalid credentials!');
    }
  };
}

// LOGOUT
function logout() {
  localStorage.removeItem('loggedIn');
  location.href = 'index.html';
}

// CRUD Logic
let users = JSON.parse(localStorage.getItem('users')) || [];
let editingId = null;

function saveUsers() {
  localStorage.setItem('users', JSON.stringify(users));
}

function renderUsers() {
  const userList = document.getElementById('userList');
  if (!userList) return;

  userList.innerHTML = '';
  users.forEach((user, index) => {
    const li = document.createElement('li');
    li.innerHTML = `
      <strong>${user.name}</strong> (${user.email})
      <button onclick="editUser(${index})">Edit</button>
      <button onclick="deleteUser(${index})">Delete</button>
    `;
    userList.appendChild(li);
  });
}

const userForm = document.getElementById('userForm');
if (userForm) {
  userForm.onsubmit = function (e) {
    e.preventDefault();
    const name = document.getElementById('name').value.trim();
    const email = document.getElementById('email').value.trim();

    if (editingId === null) {
      users.push({ name, email });
    } else {
      users[editingId] = { name, email };
      editingId = null;
    }

    saveUsers();
    renderUsers();
    userForm.reset();
  };
}

function deleteUser(index) {
  if (confirm('Delete this user?')) {
    users.splice(index, 1);
    saveUsers();
    renderUsers();
  }
}

function editUser(index) {
  const user = users[index];
  document.getElementById('name').value = user.name;
  document.getElementById('email').value = user.email;
  editingId = index;
}

renderUsers();
-----------------------------------------------
(style.css)
body {
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', sans-serif;
  height: 100vh;
  background: linear-gradient(to right, #b09ac7, #566c91);
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  background-color: white;
  padding: 30px 40px;
  border-radius: 10px;
  box-shadow: 0px 10px 30px rgba(0, 0, 0, 0.2);
  width: 100%;
  max-width: 400px;
  text-align: center;
}

h2 {
  margin-bottom: 20px;
  color: #333;
}

input {
  width: 100%;
  padding: 12px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 16px;
}

button {
  width: 100%;
  padding: 12px;
  border: none;
  background: #6589c9;
  color: white;
  font-size: 16px;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background: #7f7985;
}

p {
  margin-top: 10px;
  font-size: 14px;
  color: #555;
}
---------------------------------------------
{)settings.json
{
    "liveServer.settings.port": 5501
}
