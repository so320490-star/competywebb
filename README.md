<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CompetYWeb</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      font-family: 'Orbitron', sans-serif;
      background: linear-gradient(to bottom right, #2b0000, #660000);
      color: #fff;
      overflow-x: hidden;
    }
    header {
      background: #1a0000;
      padding: 15px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    header h1 {
      margin: 0;
      font-size: 1.5em;
      animation: glow 2s ease-in-out infinite alternate;
    }
    @keyframes glow {
      from { text-shadow: 0 0 10px #ff3333, 0 0 20px #ff0000; }
      to { text-shadow: 0 0 20px #ff6666, 0 0 30px #cc0000; }
    }
    .menu {
      font-size: 2em;
      cursor: pointer;
    }
    nav {
      display: none;
      position: absolute;
      top: 60px;
      right: 0;
      background: #1a0000;
      width: 200px;
      border-left: 2px solid #ff3333;
    }
    nav ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }
    nav ul li {
      padding: 15px;
      border-bottom: 1px solid #330000;
      cursor: pointer;
      transition: background 0.3s;
    }
    nav ul li:hover {
      background: #330000;
    }
    .container {
      text-align: center;
      padding: 40px 20px;
    }
    .option-box {
      background: rgba(255, 255, 255, 0.05);
      border: 2px solid #ff3333;
      border-radius: 12px;
      padding: 20px;
      margin: 20px auto;
      width: 90%;
      max-width: 400px;
      cursor: pointer;
      transition: transform 0.3s, background 0.3s;
    }
    .option-box:hover {
      background: rgba(255, 255, 255, 0.1);
      transform: scale(1.05);
    }
    .hidden { display: none; }
    .back {
      display: block;
      margin-bottom: 15px;
      cursor: pointer;
      font-size: 1.2em;
      color: #ff6666;
      text-align: left;
    }
    input, select, button {
      padding: 10px;
      border-radius: 6px;
      border: none;
      font-family: 'Orbitron', sans-serif;
      margin: 5px;
    }
    button {
      background: #ff3333;
      color: white;
      cursor: pointer;
      transition: background 0.3s, transform 0.2s;
    }
    button:hover {
      background: #cc0000;
      transform: scale(1.05);
    }
    .team-list li {
      list-style: none;
      background: rgba(255, 255, 255, 0.1);
      padding: 8px;
      border-radius: 6px;
      margin: 5px auto;
      max-width: 300px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .result-table {
      margin-top: 20px;
      width: 90%;
      max-width: 800px;
      margin-left: auto;
      margin-right: auto;
      border-collapse: collapse;
    }
    .result-table th, .result-table td {
      padding: 10px;
      border: 1px solid #ff3333;
    }
    .result-table th {
      background-color: #990000;
    }
    .result-table td {
      background-color: rgba(255, 255, 255, 0.05);
    }
  </style>
</head>
<body>
  <header>
    <h1>CompetYWeb</h1>
    <div class="menu" onclick="toggleMenu()">☰</div>
  </header>
  <nav id="menuNav">
    <ul>
      <li onclick="mostrarSeccion('inicio')">🏠 Inicio</li>
      <li onclick="mostrarSeccion('resultados')">📊 Resultados</li>
      <li onclick="mostrarEstadisticas()">📈 Estadísticas</li>
      <li onclick="mostrarSeccion('cruces')">⚔️ Cruces</li>
    </ul>
  </nav>

  <!-- Pantalla principal -->
  <div id="inicio" class="container">
    <h2>Bienvenido a CompetYWeb</h2>
    <div class="option-box" onclick="mostrarSeccion('modo')">Crear Torneo</div>
  </div>

  <!-- Selección PC o Teléfono -->
  <div id="modo" class="container hidden">
    <span class="back" onclick="volver('modo','inicio')">⬅ Volver</span>
    <h2>Selecciona el dispositivo</h2>
    <div class="option-box" onclick="iniciarTorneo('pc')">💻 PC</div>
    <div class="option-box" onclick="iniciarTorneo('telefono')">📱 Teléfono</div>
  </div>

  <!-- Nombre del Torneo -->
  <div id="torneo" class="container hidden">
    <span class="back" onclick="volver('torneo','modo')">⬅ Volver</span>
    <h2>Configura tu Torneo</h2>
    <input type="text" id="nombreTorneo" placeholder="Nombre del torneo"><br>
    <select id="tipoTorneo">
      <option value="">Seleccionar...</option>
      <option value="futbol">Fútbol</option>
      <option value="voleibol">Voleibol</option>
    </select><br>
    <button onclick="crearTorneo()">Crear</button>
  </div>

  <!-- Registro de Equipos y Jugadores -->
  <div id="registro" class="container hidden">
    <span class="back" onclick="volver('registro','torneo')">⬅ Volver</span>
    <h2>Registro de Equipos</h2>
    <input type="text" id="nombreEquipo" placeholder="Nombre del Equipo">
    <input type="number" id="numJugadores" placeholder="Número de Jugadores">
    <button onclick="prepararJugadores()">Añadir Equipo</button>
    <ul class="team-list" id="listaEquipos"></ul>
    
    <!-- Botón para ir a cruces -->
    <button id="btnCruces" class="hidden" onclick="mostrarCruces()">Continuar a Cruces ⚔️</button>
  </div>

  <!-- Resultados -->
  <div id="resultados" class="container hidden">
    <h2>Tabla de Resultados</h2>
    <table class="result-table">
      <thead>
        <tr>
          <th>Equipo</th>
          <th>Puntos</th>
          <th>Ganados</th>
          <th>Empates</th>
          <th>Perdidos</th>
        </tr>
      </thead>
      <tbody id="tablaResultados"></tbody>
    </table>
  </div>

  <!-- Estadísticas de jugadores -->
  <div id="estadisticas" class="container hidden">
    <span class="back" onclick="volver('estadisticas','resultados')">⬅ Volver</span>
    <h2>📊 Estadísticas de Jugadores</h2>
    <table class="result-table">
      <thead>
        <tr>
          <th>Jugador</th>
          <th>Equipo</th>
          <th>Goles</th>
          <th>Asistencias</th>
          <th>Partidos</th>
        </tr>
      </thead>
      <tbody id="tablaJugadores"></tbody>
    </table>
  </div>

  <!-- Cruces -->
  <div id="cruces" class="container hidden">
    <span class="back" onclick="volver('cruces','registro')">⬅ Volver</span>
    <h2>⚔️ Cruces del Torneo</h2>
    <div id="crucesContainer"></div>
  </div>

  <!-- Estadísticas de un partido -->
  <div id="partidoStats" class="container hidden">
    <span class="back" onclick="volver('partidoStats','cruces')">⬅ Volver</span>
    <h2>📊 Estadísticas del Partido</h2>
    <table class="result-table">
      <thead>
        <tr>
          <th>Equipo</th>
          <th>Goles</th>
          <th>Faltas</th>
          <th>Tiros</th>
        </tr>
      </thead>
      <tbody id="tablaPartido"></tbody>
    </table>
  </div>

  <script>
    let equipos = [];
    let modoSeleccionado = "";

    function toggleMenu() {
      const nav = document.getElementById("menuNav");
      nav.style.display = nav.style.display === "block" ? "none" : "block";
    }

    function mostrarSeccion(id) {
      document.querySelectorAll(".container").forEach(sec => sec.classList.add("hidden"));
      document.getElementById(id).classList.remove("hidden");
    }

    function volver(actual, anterior) {
      document.getElementById(actual).classList.add("hidden");
      document.getElementById(anterior).classList.remove("hidden");
    }

    function iniciarTorneo(modo) {
      modoSeleccionado = modo;
      mostrarSeccion("torneo");
    }

    function crearTorneo() {
      const nombre = document.getElementById("nombreTorneo").value;
      const tipo = document.getElementById("tipoTorneo").value;
      if (!nombre || !tipo) {
        alert("Completa todos los campos");
        return;
      }
      mostrarSeccion("registro");
    }

    function prepararJugadores() {
      const nombreEquipo = document.getElementById("nombreEquipo").value.trim();
      const numJugadores = parseInt(document.getElementById("numJugadores").value);
      if (!nombreEquipo || !numJugadores) {
        alert("Completa todos los campos del equipo");
        return;
      }

      let jugadores = [];
      for (let i = 1; i <= numJugadores; i++) {
        const jugador = prompt(`Nombre del jugador ${i} de ${nombreEquipo}`);
        if (jugador) jugadores.push(jugador);
      }

      equipos.push({ nombre: nombreEquipo, jugadores });
      actualizarListaEquipos();
      document.getElementById("nombreEquipo").value = "";
      document.getElementById("numJugadores").value = "";
    }

    function actualizarListaEquipos() {
      const lista = document.getElementById("listaEquipos");
      lista.innerHTML = "";
      equipos.forEach(eq => {
        const li = document.createElement("li");
        li.textContent = `${eq.nombre} (${eq.jugadores.length} jugadores)`;
        lista.appendChild(li);
      });
      document.getElementById("btnCruces").classList.toggle("hidden", equipos.length < 2);
    }

    function mostrarEstadisticas() {
      mostrarSeccion("estadisticas");
      const tbody = document.getElementById("tablaJugadores");
      tbody.innerHTML = "";
      equipos.forEach(eq => {
        eq.jugadores.forEach(j => {
          let row = document.createElement("tr");
          row.innerHTML = `
            <td>${j}</td>
            <td>${eq.nombre}</td>
            <td contenteditable="true">0</td>
            <td contenteditable="true">0</td>
            <td contenteditable="true">0</td>
          `;
          tbody.appendChild(row);
        });
      });
    }

    function mostrarCruces() {
      mostrarSeccion("cruces");
      const container = document.getElementById("crucesContainer");
      container.innerHTML = "";

      if (equipos.length === 2) {
        container.innerHTML = `<div class="option-box" onclick="mostrarPartido('${equipos[0].nombre}','${equipos[1].nombre}')">${equipos[0].nombre} 🆚 ${equipos[1].nombre} (Final)</div>`;
      } else if (equipos.length === 4) {
        container.innerHTML += `<div class="option-box" onclick="mostrarPartido('${equipos[0].nombre}','${equipos[1].nombre}')">Semifinal: ${equipos[0].nombre} 🆚 ${equipos[1].nombre}</div>`;
        container.innerHTML += `<div class="option-box" onclick="mostrarPartido('${equipos[2].nombre}','${equipos[3].nombre}')">Semifinal: ${equipos[2].nombre} 🆚 ${equipos[3].nombre}</div>`;
        container.innerHTML += `<div class="option-box">Final (Pendiente)</div>`;
      } else {
        equipos.forEach((eq,i) => {
          for (let j=i+1;j<equipos.length;j++){
            let match=document.createElement("div");
            match.className="option-box";
            match.innerText=`${equipos[i].nombre} 🆚 ${equipos[j].nombre}`;
            match.onclick=()=>mostrarPartido(equipos[i].nombre,equipos[j].nombre);
            container.appendChild(match);
          }
        });
      }
    }

    function mostrarPartido(equipoA,equipoB){
      mostrarSeccion("partidoStats");
      const tbody=document.getElementById("tablaPartido");
      tbody.innerHTML=`
        <tr>
          <td>${equipoA}</td>
          <td contenteditable="true">0</td>
          <td contenteditable="true">0</td>
          <td contenteditable="true">0</td>
        </tr>
        <tr>
          <td>${equipoB}</td>
          <td contenteditable="true">0</td>
          <td contenteditable="true">0</td>
          <td contenteditable="true">0</td>
        </tr>`;
    }
  </script>
</body>
</html>
