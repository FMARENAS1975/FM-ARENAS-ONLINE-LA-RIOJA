FM ARENAS ONLINE
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

<script src="{{ [[url_for](https://music.youtube.com/playlist?list=LM)](https://music.youtube.com/playlist?list=LM)('static', filename='script.js') }}"></script>
</body>
</html>

// Publicidad
const anuncios = [
  "HC PRODUCCIONES – TODO PARA TU FIESTA – 3804805889",
  "LOCUTOR FOTOGRAFIA SONIDO DJS LUCES  – 3804557244",
  "HC PRO TRANSMISIONES EN VIVO - TEL.  3804805889o"


![WhatsApp Image 2026-03-29 at 8 35 20 PM (1)](https://github.com/user-attachments/assets/a5a5a685-77fc-4730-96cd-1eb4dacd96eb)
