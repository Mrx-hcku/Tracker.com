const API = "https://tracker-5shn.onrender.com";
let map, marker, routeLine, token;

function showRegister() {
  document.getElementById("auth").classList.add("hidden");
  document.getElementById("register").classList.remove("hidden");
}

function showLogin() {
  document.getElementById("register").classList.add("hidden");
  document.getElementById("auth").classList.remove("hidden");
}

function register() {
  const username = document.getElementById("reg_username").value;
  const password = document.getElementById("reg_password").value;

  axios.post(`${API}/register`, { username, password })
    .then(() => {
      alert("Registered! Now login.");
      showLogin();
    })
    .catch(err => alert(err.response.data.message || "Registration failed"));
}

function login() {
  const username = document.getElementById("username").value;
  const password = document.getElementById("password").value;

  axios.post(`${API}/login`, { username, password })
    .then(res => {
      token = res.data.token;
      localStorage.setItem("token", token);
      startDashboard();
    })
    .catch(err => alert(err.response.data.message || "Login failed"));
}

function logout() {
  localStorage.removeItem("token");
  location.reload();
}

function startDashboard() {
  document.getElementById("auth").classList.add("hidden");
  document.getElementById("register").classList.add("hidden");
  document.getElementById("dashboard").classList.remove("hidden");
  initMap();
}

function initMap() {
  map = L.map("map").setView([20.5937, 78.9629], 5); // India Center
  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png").addTo(map);
  marker = L.marker([0, 0], {
    icon: L.icon({
      iconUrl: "https://cdn-icons-png.flaticon.com/512/684/684908.png",
      iconSize: [40, 40],
    }),
  }).addTo(map);
  routeLine = L.polyline([], { color: "lime" }).addTo(map);
  fetchLocation();
  setInterval(fetchLocation, 5000);
}

function fetchLocation() {
  axios.get(`${API}/get-location`, {
    headers: { Authorization: `Bearer ${token}` }
  })
    .then(res => {
      const coords = res.data.locations;
      if (coords.length > 0) {
        const last = coords[coords.length - 1];
        const latlng = [last.lat, last.lng];
        marker.setLatLng(latlng);
        routeLine.setLatLngs(coords.map(c => [c.lat, c.lng]));
        map.setView(latlng, 16);
      }
    })
    .catch(() => alert("Failed to fetch location"));
}

// Auto-login if token exists
if (localStorage.getItem("token")) {
  token = localStorage.getItem("token");
  startDashboard();
    }
