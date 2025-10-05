// === DANH SÁCH SỰ KIỆN ===
const events = [
  { title: "🗓️ Ngoại ngữ chuyên ngành CNTT", date: "2025-10-13T15:40:00", info: "Ca 6 (15h40 - 16h55) | C4.PM203" },
  { title: "💻 Mạng máy tính", date: "2025-10-17T07:00:00", info: "Ca 1 (07h00 - 11h00) | C5.103" },
  { title: "🧩 Cơ sở dữ liệu", date: "2025-10-19T09:20:00", info: "Ca 2 (09h20 - 11h20) | C6.PM203" },
  { title: "🌐 Thiết kế web", date: "2025-10-21T09:20:00", info: "Ca 2 (09h20 - 11h20) | C4.PM101" },
  { title: "🎄 Giáng sinh", date: "2025-12-25T00:00:00", info: "" },
  { title: "🎆 Tết Dương lịch", date: "2026-01-01T00:00:00", info: "" },
  { title: "🎂 Sinh nhật", date: "2026-08-22T00:00:00", info: "" }
];

const container = document.getElementById("events");
const eventSound = document.getElementById("eventSound");
const bgMusic = document.getElementById("bgMusic");
const toggleMusic = document.getElementById("toggleMusic");

// === Render sự kiện ===
events.forEach((e, i) => {
  const div = document.createElement("div");
  div.className = "event";
  div.innerHTML = `
    <div class="title">${e.title}</div>
    <div id="time${i}" class="time"></div>
    <div>${e.info}</div>
  `;
  container.appendChild(div);
});

// === Đếm ngược ===
function countdown(id, dateStr) {
  const target = new Date(dateStr).getTime();
  let triggered = false;
  const el = document.getElementById(id);

  const interval = setInterval(() => {
    const now = new Date().getTime();
    const dist = target - now;

    if (dist <= 0) {
      el.innerHTML = "🎉 Đã đến ngày!";
      if (!triggered) {
        eventSound.play();
        startFireworks();
        showPopup("💥 ĐÃ ĐẾN NGÀY SỰ KIỆN 💻");
        triggered = true;
      }
      clearInterval(interval);
      return;
    }

    const d = Math.floor(dist / (1000 * 60 * 60 * 24));
    const h = Math.floor((dist / (1000 * 60 * 60)) % 24);
    const m = Math.floor((dist / (1000 * 60)) % 60);
    const s = Math.floor((dist / 1000) % 60);
    el.innerHTML = `${d}d ${h}h ${m}m ${s}s`;
  }, 1000);
}

events.forEach((e, i) => countdown(`time${i}`, e.date));

// === Pháo hoa ===
const canvas = document.getElementById("fireworks");
const ctx = canvas.getContext("2d");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let fireworks = [];

function startFireworks() {
  for (let i = 0; i < 100; i++) {
    fireworks.push({
      x: canvas.width / 2,
      y: canvas.height / 2,
      color: `hsl(${Math.random() * 360}, 100%, 60%)`,
      angle: Math.random() * 2 * Math.PI,
      speed: Math.random() * 6 + 2,
      life: 100
    });
  }
}

function animate() {
  ctx.fillStyle = "rgba(0, 0, 0, 0.2)";
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  fireworks.forEach((f, i) => {
    f.x += Math.cos(f.angle) * f.speed;
    f.y += Math.sin(f.angle) * f.speed;
    f.life--;
    f.speed *= 0.97;
    ctx.beginPath();
    ctx.arc(f.x, f.y, 3, 0, Math.PI * 2);
    ctx.fillStyle = f.color;
    ctx.fill();
    if (f.life <= 0) fireworks.splice(i, 1);
  });
  requestAnimationFrame(animate);
}
animate();

// === Popup khi đến sự kiện ===
function showPopup(text) {
  const popup = document.createElement("div");
  popup.textContent = text;
  popup.style.position = "fixed";
  popup.style.top = "50%";
  popup.style.left = "50%";
  popup.style.transform = "translate(-50%, -50%)";
  popup.style.padding = "30px 60px";
  popup.style.background = "rgba(0,255,204,0.15)";
  popup.style.border = "2px solid #00fff2";
  popup.style.borderRadius = "15px";
  popup.style.color = "#00fff2";
  popup.style.fontSize = "28px";
  popup.style.textShadow = "0 0 15px #00fff2";
  popup.style.boxShadow = "0 0 30px #00fff2";
  popup.style.animation = "popupAnim 2s ease";
  popup.style.zIndex = 9999;
  document.body.appendChild(popup);
  setTimeout(() => popup.remove(), 2500);
}

const style = document.createElement('style');
style.textContent = `
@keyframes popupAnim {
  0% { opacity: 0; transform: translate(-50%, -60%) scale(0.8); }
  50% { opacity: 1; transform: translate(-50%, -50%) scale(1.05); }
  100% { opacity: 0; transform: translate(-50%, -40%) scale(1); }
}`;
document.head.appendChild(style);

// === Nhạc nền ===
toggleMusic.addEventListener("click", () => {
  if (bgMusic.paused) {
    bgMusic.play();
  } else {
    bgMusic.pause();
  }
});


// === Responsive canvas ===
window.addEventListener("resize", () => {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
});
