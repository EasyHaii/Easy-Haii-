# Easy-Haii-<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Easy Vehicle Rental</title>
  <style>
    body { font-family: Arial; background: #f0f0f0; margin: 0; }
    .header {
      background: #075e54; color: white; padding: 15px;
      display: flex; align-items: center; justify-content: space-between;
    }
    .header span { font-size: 24px; margin-right: 15px; cursor: pointer; }
    .header h2 { margin: 0; font-size: 20px; flex-grow: 1; }
    .menu-icon {
      font-size: 24px; cursor: pointer; padding: 0 10px;
    }
    .dropdown {
      position: absolute; top: 60px; right: 20px;
      background: white; border: 1px solid #ccc;
      border-radius: 5px; display: none;
      flex-direction: column; width: 200px; z-index: 10;
      box-shadow: 0 0 10px rgba(0,0,0,0.2);
    }
    .dropdown div {
      padding: 10px; cursor: pointer; border-bottom: 1px solid #eee;
    }
    .dropdown div:last-child { border-bottom: none; }
    .container {
      background: white; padding: 20px; border-radius: 0;
      max-width: 600px; margin: auto; box-shadow: 0 0 5px #ccc;
    }
    input, textarea, select, button {
      width: 100%; padding: 10px; margin: 10px 0;
      border-radius: 5px; border: 1px solid #ccc;
    }
    .post { padding: 15px; border-left: 5px solid #007bff; background: #e3f2fd; margin-bottom: 10px; }
    .owner { border-left: 5px solid #ffc107; background: #fffbe6; }
    .hidden { display: none; }
    .fixed-help {
      position: fixed; bottom: 20px; right: 20px;
      background: #007bff; color: white;
      padding: 10px; border-radius: 50%;
      font-size: 20px; cursor: pointer;
    }
  </style>
</head>

<body onload="startSplash()">
<!-- Splash Screen -->
<div id="splashScreen" style="position:fixed;top:0;left:0;width:100%;height:100%;background:black;display:flex;align-items:center;justify-content:center;z-index:9999;">
  <img src="logo.png" style="width: 150px; height: 150px;" />
</div>

<!-- Header with back icon and menu -->
<div class="header hidden" id="headerBar">
  <span onclick="goBack()">←</span>
  <h2>Easy Vehicle Rental</h2>
  <div class="menu-icon" onclick="toggleMenu()">☰</div>
</div>

<!-- Dropdown menu -->
<div class="dropdown" id="menuDropdown">
  <div id="loginStatus">Not logged in</div>
  <div onclick="alert('Contact: vipindhundla@gmail.com\nPhone: 8901676783')">Helpline</div>
  <div onclick="logout()">Logout</div>
  <div onclick="alert('Your posts are shown on the main screen.')">My Posts</div>
</div>

<!-- Login Section -->
<div class="container" id="loginSection">
  <h2>Welcome to Easy</h2>
  <input type="text" id="loginInput" placeholder="Enter Phone or Email (optional)">
  <button onclick="startApp()">Continue</button>
  <button onclick="skipLogin()">Skip</button>
</div>

<!-- Main App Section -->
<div class="container hidden" id="mainSection">
  <select id="role">
    <option value="owner">Rent Out</option>
    <option value="renter">Hire</option>
  </select>
  <input type="text" id="name" placeholder="Your Name">
  <input type="text" id="vehicle" placeholder="Vehicle Type or Need">
  <input type="text" id="location" placeholder="Location">
  <input type="text" id="contact" placeholder="Contact Number">
  <textarea id="condition" placeholder="Conditions"></textarea>
  <input type="file" id="image" accept="image/*" onchange="previewImage()">
  <input type="hidden" id="imageData">
  <img id="preview" style="max-width:100%; display:none;">
  <button onclick="addPost()">Post</button>
</div>

<!-- Posts Section -->
<div class="container hidden" id="postsSection">
  <h3>All Posts</h3>
  <div id="postsList"></div>
</div>

<!-- Help Icon -->
<div class="fixed-help" onclick="alert('Contact: vipindhundla@gmail.com\nPhone: 8901676783')">?</div>

<script>
  let loggedInUser = "";

  function startApp() {
    loggedInUser = document.getElementById('loginInput').value.trim();
    document.getElementById('loginStatus').innerText = loggedInUser ? 'User: ' + loggedInUser : 'Not logged in';
    document.getElementById('loginSection').classList.add('hidden');
    document.getElementById('mainSection').classList.remove('hidden');
    document.getElementById('postsSection').classList.remove('hidden');
    document.getElementById('headerBar').classList.remove('hidden');
  }

  function skipLogin() {
    startApp();
  }

  function goBack() {
    document.getElementById('mainSection').classList.add('hidden');
    document.getElementById('postsSection').classList.add('hidden');
    document.getElementById('headerBar').classList.add('hidden');
    document.getElementById('loginSection').classList.remove('hidden');
  }

  function toggleMenu() {
    const menu = document.getElementById("menuDropdown");
    menu.style.display = menu.style.display === "flex" ? "none" : "flex";
  }

  function previewImage() {
    const file = document.getElementById("image").files[0];
    const reader = new FileReader();
    reader.onload = function(e) {
      document.getElementById("preview").src = e.target.result;
      document.getElementById("preview").style.display = "block";
      document.getElementById("imageData").value = e.target.result;
    };
    if (file) reader.readAsDataURL(file);
  }

  function addPost() {
    const role = document.getElementById("role").value;
    const name = document.getElementById("name").value;
    const vehicle = document.getElementById("vehicle").value;
    const location = document.getElementById("location").value;
    const contact = document.getElementById("contact").value;
    const condition = document.getElementById("condition").value;
    const image = document.getElementById("imageData").value;

    if (!name || !vehicle || !location || !contact || !condition) {
      alert("Please fill all fields.");
      return;
    }

    const div = document.createElement("div");
    div.className = "post " + (role === "owner" ? "owner" : "");
    div.innerHTML = `<b>${role === 'owner' ? 'Vehicle' : 'Need'}:</b> ${vehicle}<br>
      <b>Location:</b> ${location}<br>
      <b>Conditions:</b> ${condition}<br>
      <b>Contact:</b> ${contact}<br>
      <b>Posted by:</b> ${name}<br>
      ${image ? `<img src="${image}" style="max-width:100%; margin-top:10px;">` : ''}
      <br><button onclick="this.parentNode.remove()">Delete</button>`;
    document.getElementById("postsList").prepend(div);

    document.getElementById("name").value = '';
    document.getElementById("vehicle").value = '';
    document.getElementById("location").value = '';
    document.getElementById("contact").value = '';
    document.getElementById("condition").value = '';
    document.getElementById("image").value = '';
    document.getElementById("preview").style.display = "none";
    document.getElementById("imageData").value = '';
  }

  function startSplash() {
    setTimeout(() => {
      document.getElementById("splashScreen").style.display = "none";
    }, 1000);
  }

  function logout() {
    loggedInUser = "";
    document.getElementById('loginStatus').innerText = 'Not logged in';
    goBack();
    document.getElementById("menuDropdown").style.display = "none";
    document.getElementById("loginInput").value = '';
  }
</script>

</body>
</html>
