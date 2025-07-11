<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Telegram Clone (Customizable)</title>
  <style>
    /* Basic Reset */
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Helvetica Neue', Arial, sans-serif;
    }
    body {
      background: #ffffff;
      color: #000;
    }

    /* Status Bar */
    .status-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 5px 12px;
      font-size: 14px;
      background: #f9f9f9;
      border-bottom: 1px solid #ddd;
    }

    /* Top Menu */
    .top-menu {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #0088cc; /* Telegram Blue */
      color: #fff;
      padding: 10px 15px;
    }
    .top-menu h1 {
      font-size: 20px;
    }
    .top-menu .search {
      font-size: 20px;
      cursor: pointer;
    }

    /* Chat List */
    .chat-list {
      padding: 10px;
    }
    .chat-item {
      display: flex;
      align-items: center;
      padding: 8px 0;
      border-bottom: 1px solid #eee;
      cursor: pointer;
    }
    .dp {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      background: #ccc;
      color: #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      margin-right: 10px;
    }
    .details {
      flex: 1;
    }
    .top {
      display: flex;
      justify-content: space-between;
      margin-bottom: 5px;
    }
    .name {
      font-weight: bold;
    }
    .time {
      font-size: 12px;
      color: #999;
    }
    .msg {
      font-size: 14px;
      color: #333;
    }
    .msg .unread {
      background: #0088cc;
      color: #fff;
      border-radius: 50%;
      padding: 2px 6px;
      font-size: 12px;
      margin-left: 5px;
    }
    .actions button {
      background: none;
      border: none;
      cursor: pointer;
      font-size: 16px;
      margin-left: 5px;
    }

    /* Form */
    .form {
      display: none;
      position: fixed;
      top: 70px;
      right: 20px;
      background: #fff;
      border: 1px solid #ddd;
      padding: 15px;
      border-radius: 6px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      z-index: 1000;
    }
    .form input {
      display: block;
      width: 200px;
      margin-bottom: 10px;
      padding: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
    }
    .form button {
      padding: 8px 12px;
      background: #0088cc;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    /* Add Chat Button */
    .add-chat-btn {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #0088cc;
      color: #fff;
      border: none;
      border-radius: 50%;
      width: 60px;
      height: 60px;
      font-size: 30px;
      cursor: pointer;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    }
  </style>
</head>
<body>
  <!-- Status Bar -->
  <div class="status-bar">
    <span id="clock">00:00</span>
    <span>📶 🛜 🔋</span>
  </div>

  <!-- Top Menu -->
  <div class="top-menu">
    <h1>Telegram</h1>
    <span class="search">🔍</span>
  </div>

  <!-- Chat List -->
  <div class="chat-list" id="chatList"></div>

  <!-- Add Chat Form -->
  <div class="form" id="form">
    <input type="text" id="firstName" placeholder="First Name" />
    <input type="text" id="lastName" placeholder="Last Name" />
    <input type="text" id="message" placeholder="Message" />
    <button onclick="addChat()">Add Chat</button>
  </div>

  <!-- Add Chat Floating Button -->
  <button class="add-chat-btn" onclick="toggleForm()">＋</button>

  <script>
    // Clock
    function updateClock() {
      const now = new Date();
      const hrs = String(now.getHours()).padStart(2, '0');
      const mins = String(now.getMinutes()).padStart(2, '0');
      document.getElementById('clock').textContent = `${hrs}:${mins}`;
    }
    setInterval(updateClock, 1000);
    updateClock();

    // Toggle Form
    function toggleForm() {
      const form = document.getElementById('form');
      form.style.display = (form.style.display === 'block') ? 'none' : 'block';
    }

    // Add Chat
    function addChat() {
      const firstName = document.getElementById('firstName').value.trim();
      const lastName = document.getElementById('lastName').value.trim();
      const message = document.getElementById('message').value.trim();
      if (!firstName || !message) {
        alert('First name and message required');
        return;
      }
      const time = new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
      const chatList = document.getElementById('chatList');
      const initials = firstName.charAt(0).toUpperCase() + (lastName.charAt(0) ? lastName.charAt(0).toUpperCase() : '');
      const chatItem = document.createElement('div');
      chatItem.className = 'chat-item';
      chatItem.innerHTML = `
        <div class="dp">${initials}</div>
        <div class="details">
          <div class="top">
            <span class="name">${firstName} ${lastName}</span>
            <span class="time">${time}</span>
          </div>
          <div class="msg">${message} <span class="unread">1</span></div>
        </div>
        <div class="actions">
          <button onclick="pinChat(this)">📌</button>
          <button onclick="deleteChat(this)">🗑️</button>
        </div>
      `;
      chatList.prepend(chatItem);
      toggleForm();
      document.getElementById('firstName').value = '';
      document.getElementById('lastName').value = '';
      document.getElementById('message').value = '';
    }

    // Pin Chat
    function pinChat(btn) {
      const chat = btn.closest('.chat-item');
      const list = document.getElementById('chatList');
      list.prepend(chat);
    }

    // Delete Chat
    function deleteChat(btn) {
      const chat = btn.closest('.chat-item');
      chat.remove();
    }
  </script>
</body>
</html>
