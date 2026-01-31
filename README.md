<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <title>Gerenciador de Notas (LocalStorage)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      padding: 30px;
    }

    .container {
      max-width: 500px;
      margin: auto;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    input, button {
      padding: 8px;
      margin: 5px 0;
    }

    ul {
      list-style: none;
      padding: 0;
      margin-top: 15px;
    }

    li {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #eee;
      padding: 8px;
      margin-bottom: 5px;
      border-radius: 4px;
    }

    .remover {
      background: #ff4d4d;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      padding: 5px 8px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>📝 Gerenciador de Notas</h2>

    <input type="text" id="tituloNota" placeholder="Digite o título da nota" />
    <button id="adicionarNota">Adicionar Nota</button>

    <ul id="listaNotas"></ul>
  </div>

  <script>
    const inputTitulo = document.querySelector("#tituloNota");
    const botaoAdicionar = document.querySelector("#adicionarNota");
    const listaNotas = document.querySelector("#listaNotas");

    function obterNotas() {
      const notas = localStorage.getItem("notas");
      return notas ? JSON.parse(notas) : [];
    }

    function salvarNotas(notas) {
      localStorage.setItem("notas", JSON.stringify(notas));
    }

    function renderizarNotas() {
      listaNotas.innerHTML = "";

      const notas = obterNotas();

      notas.forEach((nota) => {
        const li = document.createElement("li");
        li.textContent = nota.titulo;

        const botaoRemover = document.createElement("button");
        botaoRemover.textContent = "Remover";
        botaoRemover.classList.add("remover");

        botaoRemover.addEventListener("click", () => {
          removerNota(nota.id);
        });

        li.appendChild(botaoRemover);
        listaNotas.appendChild(li);
      });
    }

    botaoAdicionar.addEventListener("click", () => {
      const titulo = inputTitulo.value.trim();

      if (!titulo) return;

      const notas = obterNotas();

      const novaNota = {
        id: Date.now(),       
        titulo: titulo
      };

      notas.push(novaNota);
      salvarNotas(notas);

      inputTitulo.value = "";
      renderizarNotas();
    });

    function removerNota(id) {
      let notas = obterNotas();
      notas = notas.filter((nota) => nota.id !== id);
      salvarNotas(notas);
      renderizarNotas();
    }

    renderizarNotas();
  </script>
</body>
</html>
