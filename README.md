[!DOCTYPEr.html](https://github.com/user-attachments/files/23103428/DOCTYPEr.html)
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Rose Gold — Página Principal
Salome Bedoya, Paulina Correa</title>
  <style>
    body {
      background: linear-gradient(135deg, #FFD700, #E6A8D7);
      font-family: 'Poppins', sans-serif;
      text-align: center;
      color: #1a1a1a;
      margin: 0;
      padding: 0;
      min-height: 100vh;
    }

    h1 {
      color: #000;
      text-shadow: 2px 2px 6px rgba(255, 215, 0, 0.6);
      margin-top: 40px;
      font-size: 2.2em;
      letter-spacing: 1px;
    }

    nav {
      margin-top: 25px;
    }

    button {
      margin: 10px;
      text-decoration: none;
      color: #FFD700;
      background: #000;
      padding: 12px 25px;
      border-radius: 25px;
      font-weight: bold;
      font-size: 1em;
      border: none;
      cursor: pointer;
      box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.4);
      transition: all 0.3s ease;
    }

    button:hover {
      background: #222;
      color: #fff;
      transform: scale(1.08);
      box-shadow: 0 0 15px rgba(255, 215, 0, 0.7);
    }

    .section {
      display: none;
      padding: 20px;
      margin-top: 30px;
      text-align: center;
      background: rgba(255, 255, 255, 0.9);
      border-radius: 20px;
      width: 90%;
      max-width: 900px;
      margin-left: auto;
      margin-right: auto;
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
    }

    .active {
      display: block;
    }

    /* Estilos del formulario */
    .form-container input, .form-container button {
      display: block;
      width: 80%;
      margin: 10px auto;
      padding: 10px;
      border-radius: 10px;
      border: 2px solid #FFD700;
      font-size: 1rem;
    }

    .form-container button {
      background: #000;
      color: #FFD700;
      font-weight: bold;
      cursor: pointer;
    }

    .form-container button:hover {
      background: #222;
      color: #fff;
    }

    /* Sopa de letras */
    .grid {
      display: grid;
      grid-template-columns: repeat(12, 35px);
      justify-content: center;
      gap: 5px;
      margin-top: 20px;
    }

    .cell {
      width: 35px;
      height: 35px;
      background: #fff0c4;
      border-radius: 6px;
      font-weight: bold;
      font-size: 18px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      user-select: none;
    }

    .cell.selected {
      background: #FFD700;
      color: #000;
    }

    .cell.found {
      background: #000;
      color: #FFD700;
    }

    ul.words {
      list-style: none;
      padding: 0;
      margin-top: 15px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>✨ Bienvenido a la página ROSE GOLD ✨</h1>
  <p>🌸 Explora nuestras secciones interactivas 🌸</p>

  <nav>
    <button onclick="mostrar('inicio')">Inicio</button>
    <button onclick="mostrar('juego')">Sopa de Letras</button>
    <button onclick="mostrar('formulario')">Formulario</button>
  </nav>

  <!-- Sección Inicio -->
  <div id="inicio" class="section active">
    <h2>🏠 Página Principal</h2>
    <p>Bienvenido a la experiencia Rose Gold — disfruta de los juegos y el estilo elegante. ✨</p>
  </div>

  <!-- Sección Sopa de Letras -->
  <div id="juego" class="section">
    <h2>🍰 Sopa de Letras — Postres</h2>
    <p>Encuentra todos los postres escondidos en la cuadrícula.</p>
    <div id="board" class="grid"></div>
    <ul class="words" id="wordList"></ul>
  </div>

  <!-- Sección Formulario -->
  <div id="formulario" class="section form-container">
    <h2>📝 Registro</h2>
    <form id="registerForm">
      <input type="text" id="nombre" name="nombre" placeholder="Nombre completo" required>
      <input type="email" id="email" name="email" placeholder="Correo electrónico" required>
      <input type="tel" id="telefono" name="telefono" placeholder="Teléfono (10 dígitos)" pattern="[0-9]{10}" required>
      <input type="password" id="password" name="password" placeholder="Contraseña" required>
      <input type="password" id="confirm" name="confirm" placeholder="Confirmar contraseña" required>
      <button type="submit">Registrarme</button>
    </form>
    <p id="successMsg" style="display:none; color:green; font-weight:bold;">¡Registro exitoso! 🎉</p>
  </div>

  <script>
    function mostrar(id) {
      document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    // Formulario
    document.getElementById('registerForm').addEventListener('submit', (e) => {
      e.preventDefault();
      const pass = document.getElementById('password').value;
      const conf = document.getElementById('confirm').value;
      if (pass !== conf) {
        alert('Las contraseñas no coinciden');
        return;
      }
      e.target.reset();
      document.getElementById('successMsg').style.display = 'block';
    });

    // --- Sopa de letras ---
    const words = ['TARTA','PASTEL','FLAN','COOKIE','DONUT','BROWNIE','HELADO','MOUSSE','CHURRO','PANNA'];
    const rows = 12, cols = 12;
    const boardEl = document.getElementById('board');
    const wordListEl = document.getElementById('wordList');
    let grid = Array.from({length: rows}, () => Array(cols).fill(''));
    const dirs = [[1,0],[-1,0],[0,1],[0,-1],[1,1],[-1,1],[1,-1],[-1,-1]];
    const found = new Set();

    function colocarPalabras() {
      words.sort((a,b)=>b.length-a.length);
      for (const w of words) {
        for (let intento = 0; intento < 100; intento++) {
          const dir = dirs[Math.floor(Math.random()*dirs.length)];
          const [dx, dy] = dir;
          const x = Math.floor(Math.random()*cols);
          const y = Math.floor(Math.random()*rows);
          const endX = x + dx*(w.length-1);
          const endY = y + dy*(w.length-1);
          if (endX<0||endX>=cols||endY<0||endY>=rows) continue;
          let ok = true;
          for (let i=0;i<w.length;i++){
            const nx=x+dx*i, ny=y+dy*i;
            if(grid[ny][nx] && grid[ny][nx]!==w[i]) {ok=false;break;}
          }
          if(!ok) continue;
          for (let i=0;i<w.length;i++) grid[y+dy*i][x+dx*i]=w[i];
          break;
        }
      }
    }

    function rellenar() {
      const letras = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
      for(let y=0;y<rows;y++) for(let x=0;x<cols;x++)
        if(!grid[y][x]) grid[y][x]=letras[Math.floor(Math.random()*letras.length)];
    }

    function renderizar() {
      boardEl.innerHTML = '';
      for(let y=0;y<rows;y++){
        for(let x=0;x<cols;x++){
          const div = document.createElement('div');
          div.className = 'cell';
          div.textContent = grid[y][x];
          div.dataset.x = x;
          div.dataset.y = y;
          boardEl.appendChild(div);
        }
      }
      wordListEl.innerHTML = words.map(w=>`<li>${w}</li>`).join('');
    }

    function iniciarJuego(){
      colocarPalabras();
      rellenar();
      renderizar();
    }

    iniciarJuego();
  </script>
</body>
</html>
