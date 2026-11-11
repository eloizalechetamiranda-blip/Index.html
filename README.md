<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>NutriAthlete</title>

<style>
* {
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background: #eef2f3;
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 750px;
    margin: auto;
    background: white;
    padding: 25px;
    border-radius: 15px;
    box-shadow: 0 3px 15px #ccc;
}

h1 {
    text-align: center;
    color: #16834a;
}

h2 {
    color: #16834a;
}

input, select, button {
    width: 100%;
    padding: 13px;
    margin: 7px 0 15px;
    border-radius: 7px;
    border: 1px solid #ccc;
    font-size: 16px;
}

button {
    background: #16834a;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background: #106638;
}

button:active {
    transform: scale(0.99);
}

.resultado {
    display: none;
    background: #e5f5e9;
    padding: 15px;
    border-radius: 10px;
    margin-top: 15px;
}

.refeicao {
    background: #f1f5f2;
    padding: 10px;
    margin: 8px 0;
    border-radius: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
}

.refeicao button {
    width: auto;
    background: #d93434;
    padding: 8px;
    margin: 0;
}

.agua {
    background: #e4f4ff;
    padding: 15px;
    border-radius: 10px;
    margin-top: 20px;
}

.pequeno {
    font-size: 13px;
    color: #666;
}

</style>
</head>

<body>

<div class="container">

<h1>🏃 NutriAthlete</h1>

<p>Plataforma de acompanhamento nutricional esportivo</p>

<hr>

<h2>📋 Cadastro do atleta</h2>

<form id="formAtleta">

<label>Nome:</label>
<input id="nome" type="text" placeholder="Nome do atleta" required>

<label>Idade:</label>
<input id="idade" type="number" min="1" max="120" required>

<label>Peso (kg):</label>
<input id="peso" type="number" min="1" step="0.1" required>

<label>Altura (cm):</label>
<input id="altura" type="number" min="1" required>

<label>Sexo:</label>
<select id="sexo">
<option value="masculino">Masculino</option>
<option value="feminino">Feminino</option>
</select>

<label>Atividade física:</label>

<select id="atividade">
<option value="1.2">Leve</option>
<option value="1.375">Moderada</option>
<option value="1.55">Intensa</option>
<option value="1.725">Muito intensa</option>
<option value="1.9">Extremamente intensa</option>
</select>

<button type="submit">Calcular necessidades</button>

</form>

<div id="resultado" class="resultado"></div>

<hr>

<h2>🍎 Controle de refeições</h2>

<label>Nome da refeição:</label>
<input id="refeicao" type="text"
placeholder="Ex: Café da manhã">

<label>Descrição:</label>
<input id="descricao" type="text"
placeholder="Ex: Ovos, frutas e aveia">

<button type="button" id="adicionar">
Adicionar refeição
</button>

<div id="listaRefeicoes"></div>

<hr>

<h2>💧 Controle de hidratação</h2>

<div class="agua">

<p>Água consumida:</p>

<h2 id="quantidadeAgua">0 ml</h2>

<button type="button" id="adicionarAgua">
+ Adicionar 250 ml
</button>

<button type="button" id="zerarAgua"
style="background:#777">
Zerar consumo
</button>

</div>

<p class="pequeno">
Sistema demonstrativo. As necessidades nutricionais devem
ser individualizadas por nutricionista esportivo.
</p>

</div>

<script>

document.addEventListener("DOMContentLoaded", function() {

    const form = document.getElementById("formAtleta");

    const resultado = document.getElementById("resultado");

    let agua = 0;

    let refeicoes = [];

    // CALCULAR NECESSIDADES

    form.addEventListener("submit", function(event) {

        event.preventDefault();

        const nome = document.getElementById("nome").value.trim();

        const idade = Number(document.getElementById("idade").value);

        const peso = Number(document.getElementById("peso").value);

        const altura = Number(document.getElementById("altura").value);

        const sexo = document.getElementById("sexo").value;

        const atividade = Number(
            document.getElementById("atividade").value
        );

        if (!nome || idade <= 0 || peso <= 0 || altura <= 0) {
            alert("Preencha todos os campos corretamente!");
            return;
        }

        const alturaMetros = altura / 100;

        const imc = peso / (alturaMetros * alturaMetros);

        let tmb;

        if (sexo === "masculino") {

            tmb = (10 * peso) + (6.25 * altura)
            - (5 * idade) + 5;

        } else {

            tmb = (10 * peso) + (6.25 * altura)
            - (5 * idade) - 161;

        }

        const calorias = tmb * atividade;

        resultado.style.display = "block";

        resultado.innerHTML = `
            <h2>Resultado do atleta</h2>

            <p><strong>Nome:</strong> ${nome}</p>

            <p><strong>IMC:</strong> ${imc.toFixed(2)}</p>

            <p><strong>Metabolismo basal estimado:</strong>
            ${tmb.toFixed(0)} kcal</p>

            <p><strong>Gasto energético estimado:</strong>
            ${calorias.toFixed(0)} kcal/dia</p>
        `;

    });

    // ADICIONAR REFEIÇÕES

    document.getElementById("adicionar")
    .addEventListener("click", function() {

        const nome = document.getElementById("refeicao").value.trim();

        const descricao = document.getElementById("descricao").value.trim();

        if (!nome || !descricao) {

            alert("Preencha o nome e a descrição da refeição!");

            return;

        }

        refeicoes.push({
            nome: nome,
            descricao: descricao
        });

        document.getElementById("refeicao").value = "";

        document.getElementById("descricao").value = "";

        mostrarRefeicoes();

    });

    function mostrarRefeicoes() {

        const lista = document.getElementById("listaRefeicoes");

        lista.innerHTML = "";

        refeicoes.forEach(function(item, index) {

            const div = document.createElement("div");

            div.className = "refeicao";

            const texto = document.createElement("span");

            texto.textContent = item.nome + ": " + item.descricao;

            const botao = document.createElement("button");

            botao.textContent = "Excluir";

            botao.type = "button";

            botao.addEventListener("click", function() {

                refeicoes.splice(index, 1);

                mostrarRefeicoes();

            });

            div.appendChild(texto);

            div.appendChild(botao);

            lista.appendChild(div);

        });

    }

    // CONTROLE DE ÁGUA

    document.getElementById("adicionarAgua")
    .addEventListener("click", function() {

        agua += 250;

        atualizarAgua();

    });

    document.getElementById("zerarAgua")
    .addEventListener("click", function() {

        agua = 0;

        atualizarAgua();

    });

    function atualizarAgua() {

        document.getElementById("quantidadeAgua")
        .textContent = agua + " ml";

    }

});

</script>

</body>
</html>
