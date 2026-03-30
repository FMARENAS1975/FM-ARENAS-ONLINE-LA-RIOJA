!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>FM ARENAS ONLINE - La Rioja</title>
  <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
  <script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
</head>
<body>

<div class="app">

  <aside class="sidebar">
    <h1>🎧 FM ARENAS</h1>
    <p>Online La Rioja</p>
    <nav>
      <a href="#">Inicio</a>
      <a href="#">En Vivo</a>
      <a href="#">Programación</a>
    </nav>
  </aside>

  <main class="main">
    <h2>🔴 EN VIVO</h2>

    <div class="player-card">
      <div class="cover">
        <img id="coverImg" src="{{ url_for('static', filename='images/default.jpg') }}" alt="Portada">
      </div>
      <div class="info">
        <h3 id="songTitle">Cargando...</h3>
        <p id="artist">FM Arenas Online</p>
      </div>
      <button id="playBtn">▶️ Play</button>
      <audio id="radio" src="http://TU_IP:8000/radio.mp3"></audio>
    </div>

    <div class="programacion">
      <h2>📻 FM ARENAS ONLINE - La Rioja</h2>
      <p><strong>¡Bienvenidos!</strong> Acompañándote 24/7 con música Y noticias.</p>
      <ul>
        <li>06:00 - 09:00 | Buenos Días La Rioja</li>
        <li>09:00 - 12:00 | Hits del Momento</li>
        <li>12:00 - 15:00 | Mediodía en Vivo</li>
        <li>15:00 - 18:00 | Tarde de Éxitos</li>
        <li>18:00 - 21:00 | Show Nocturno</li>
        <li>21:00 - 00:00 | Noche Chill</li>
        <li>00:00 - 06:00 | Modo 24/7</li>
      </ul>

      <h3>📢 Publicidad</h3>
      <p><strong>HC PRODUCCIONES</strong> – TODO PARA TU FIESTA – DJ SONIDO LUCES FOTO LOCUCION PANTALLA Y PROYECTOR 📞 3804805889 - 3804557244</p>
    </div>

    <div class="history">
      <h3>🎶 Historial de canciones</h3>
      <ul id="historyList"></ul>
    </div>

    <div class="chat">
      <h3>💬 Chat en vivo</h3>
      <ul id="chatList"></ul>
      <input type="text" id="mensajeInput" placeholder="Escribí tu mensaje">
      <button id="enviarBtn">Enviar</button>
    </div>

    <p id="publicidad"></p>

  </main>
</div>

<script src="{{ url_for('static', filename='script.js') }}"></script>
</body>
</html>


🎨 5. CSS: static/styles.css

body { margin:0; font-family:Arial; background:#121212; color:white; }
.app { display:flex; height:100vh; }
.sidebar { width:220px; background:#000; padding:20px; }
.sidebar a { display:block; color:#b3b3b3; margin:10px 0; text-decoration:none; }
.sidebar a:hover { color:white; }
.main { flex:1; padding:30px; overflow-y:auto; }
.player-card, .programacion, .history, .chat { background:#181818; padding:20px; border-radius:10px; width:600px; margin-bottom:20px; }
.cover img { width:100%; border-radius:10px; }
button { background:#1db954; border:none; padding:10px 20px; color:white; border-radius:20px; cursor:pointer; }
button:hover { background:#1ed760; }
.programacion h2, .programacion h3 { color:#1db954; }
.history ul, .chat ul { list-style:none; padding:0; }
.history li, .chat li { color:#b3b3b3; margin-bottom:5px; }


🧠 6. JS dinámico: static/script.js


const audio = document.getElementById("radio");
const btn = document.getElementById("playBtn");
const historyList = document.getElementById("historyList");
const songTitle = document.getElementById("songTitle");
const artist = document.getElementById("artist");
const coverImg = document.getElementById("coverImg");

let playing = false;
let lastSong = "";

btn.addEventListener("click", () => {
  if(!playing){audio.play(); btn.textContent="⏸️ Pause";}
  else {audio.pause(); btn.textContent="▶️ Play";}
  playing = !playing;
});

// Chat en vivo WebSocket
const socket = io();
document.getElementById("enviarBtn").addEventListener("click", ()=>{
  const msg = document.getElementById("mensajeInput").value;
  socket.emit("mensaje_oyente", {usuario:"Oyente", mensaje:msg});
  document.getElementById("mensajeInput").value = "";
});
socket.on("nuevo_mensaje", data => {
  const li = document.createElement("li");
  li.textContent = `${data.usuario}: ${data.mensaje}`;
  document.getElementById("chatList").appendChild(li);
});

// Publicidad
const anuncios = [
  "HC PRODUCCIONES – TODO PARA TU FIESTA – 3804805889",
  "LOCUTOR FOTOGRAFIA SONIDO DJS LUCES  – 3804557244",
  "HC PRO TRANSMISIONES EN VIVO - TEL.  3804805889o"
];
let idx = 0;
function mostrarPublicidad(){document.getElementById("publicidad").textContent = anuncios[idx]; idx=(idx+1)%anuncios.length;}
setInterval(mostrarPublicidad,15000);
mostrarPublicidad();

// Actualiza canción en vivo y locutor
async function reproducirLocutor(texto){
  try{ new Audio(`/locutor/${encodeURIComponent(texto)}`).play(); }
  catch(e){console.log("Error locutor:", e);}
}

async function actualizarCancion(){
  try{
    const res = await fetch("/current_song");
    const data = await res.json();
    const song = data.song;

    if(song && song !== lastSong){
      lastSong = song;
      const partes = song.split(" - ");
      const titulo = partes[0] || song;
      const artistaNombre = partes[1] || "Desconocido";

      songTitle.textContent = titulo;
      artist.textContent = artistaNombre;
      coverImg.src = `static/images/${titulo.replace(/\s+/g,"").toLowerCase()}.jpg`;

      const li = document.createElement("li");
      li.textContent = `${artistaNombre} - ${titulo}`;
      historyList.prepend(li);

      const locMsgs = [
        `Qué buena está sonando ${titulo} de ${artistaNombre}... ¡subí el volumen!`,
        `${titulo} de ${artistaNombre} es uno de mis favoritos`,
        `Disfrutá ${titulo} de ${artistaNombre}, Leo y Sofi te acompañan`
      ];
      reproducirLocutor(locMsgs[Math.floor(Math.random()*locMsgs.length)]);
    }
  }catch(e){console.log("Error:", e);}
}
setInterval(actualizarCancion,10000);
actualizarCancion();


📌 7. Imágenes de portadas (static/images/)
![WhatsApp Image 2026-03-29 at 8 35 20 PM (1)](https://github.com/user-attachments/assets/a5a5a685-77fc-4730-96cd-1eb4dacd96eb)
