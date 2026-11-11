<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>NutriAthlete</title>

<style>
body {
    font-family: Arial, sans-serif;
    background: #eef2f3;
    padding: 20px;
}

.container {
    max-width: 600px;
    margin: auto;
    background: white;
    padding: 25px;
    border-radius: 12px;
}

h1 {
    color: #16834a;
    text-align: center;
}

input, select, button {
    width: 100%;
    padding: 12px;
    margin: 8px 0;
    box-sizing: border-box;
}

button {
    background: #16834a;
    color: white;
    border: none;
    cursor: pointer;
}

#resultado {
    background: #e1f5e9;
    padding: 15px;
    margin-top: 15px;
    border-radius: 8px;
}
</style>
</head>

<body>

<div class="container">

<h1>NutriAthlete</h1>
<p>Programa de acompanhamento alimentar para atletas</p>

<label>Nome do atleta:</label>
<input type="text" id="nome">

<label>Idade:</label>
<input type="number" id="idade">

<label>Peso (kg):</label>
<input type="number" id="peso">

<label>Altura (cm):</label>
<input type="number" id="altura">

<label>Sexo:</label>
<select id="sexo">
<option value="masculino">Masculino</option>
<option value="feminino">Feminino</option>
</select>

<label>Nível de atividade:</label>
<select id="atividade">
<option value="1.2">Sedentário</option>
<option value="1.375">Leve</option>
<option value="1.55">Moderado</option>
<option value="1.725">Intenso</option>
<option value="1.9">Muito intenso</option>
</select>

<button onclick="calcular()">Calcular necessidades</button>

<div id="resultado"></div>

</div>

<script>

function calcular() {

    let nome = document.getElementById("nome").value;
    let idade = Number(document.getElementById("idade").value);
    let peso = Number(document.getElementById("peso").value);
    let altura = Number(document.getElementById("altura").value);
    let sexo = document.getElementById("sexo").value;
    let atividade = Number(document.getElementById("atividade").value);

    if (!nome || idade <= 0 || peso <= 0 || altura <= 0) {
        alert("Preencha todos os campos corretamente!");
        return;
    }

    let alturaMetros = altura / 100;

    let imc = peso / (alturaMetros * alturaMetros);

    let tmb;

    if (sexo === "masculino") {
        tmb = (10 * peso) + (6.25 * altura)
        - (5 * idade) + 5;
    } else {
        tmb = (10 * peso) + (6.25 * altura)
        - (5 * idade) - 161;
    }

    let calorias = tmb * atividade;

    document.getElementById("resultado").innerHTML = `

        <h2>Resultado do atleta</h2>

        <p><strong>Nome:</strong> ${nome}</p>

        <p><strong>IMC:</strong> ${imc.toFixed(2)}</p>

        <p><strong>Metabolismo basal estimado:</strong>
        ${tmb.toFixed(0)} kcal</p>

        <p><strong>Gasto energético diário estimado:</strong>
        ${calorias.toFixed(0)} kcal</p>

        <h3>Sugestão de organização alimentar</h3>

        <ul>
            <li>Café da manhã: carboidratos e proteínas.</li>
            <li>Lanche: frutas e alimentos proteicos.</li>
            <li>Almoço: arroz, feijão, proteínas e vegetais.</li>
            <li>Pré-treino: alimentos ricos em carboidratos.</li>
            <li>Pós-treino: carboidratos e proteínas.</li>
            <li>Jantar: refeição equilibrada.</li>
        </ul>

        <p><strong>Atenção:</strong> os valores são estimativas
        e não substituem uma avaliação nutricional individualizada.</p>
    `;
}

</script>

</body>
</html>
