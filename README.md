<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NovaAI - Inteligência Artificial</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, sans-serif;
    background: #0b1020;
    color: white;
    height: 100vh;
    display: flex;
}

.sidebar {
    width: 250px;
    background: #111827;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 15px;
}

.logo {
    font-size: 27px;
    font-weight: bold;
    color: #60a5fa;
    margin-bottom: 15px;
}

button {
    cursor: pointer;
    border: none;
    border-radius: 9px;
    padding: 12px;
    font-size: 14px;
}

.new-chat {
    background: #2563eb;
    color: white;
}

.new-chat:hover {
    background: #1d4ed8;
}

.sidebar p {
    color: #9ca3af;
    font-size: 13px;
}

.main {
    flex: 1;
    display: flex;
    flex-direction: column;
    min-width: 0;
}

header {
    padding: 20px;
    border-bottom: 1px solid #273449;
    font-size: 19px;
    font-weight: bold;
}

#messages {
    flex: 1;
    overflow-y: auto;
    padding: 25px;
    display: flex;
    flex-direction: column;
    gap: 18px;
}

.message {
    max-width: 800px;
    width: fit-content;
    padding: 15px 18px;
    border-radius: 14px;
    line-height: 1.6;
    white-space: pre-wrap;
    overflow-wrap: anywhere;
}

.user {
    align-self: flex-end;
    background: #2563eb;
}

.bot {
    align-self: flex-start;
    background: #1e293b;
}

.welcome {
    text-align: center;
    margin: auto;
    color: #dbeafe;
}

.welcome h1 {
    font-size: 36px;
    margin-bottom: 12px;
}

.welcome p {
    color: #94a3b8;
}

.input-area {
    padding: 20px;
    display: flex;
    justify-content: center;
    gap: 10px;
    border-top: 1px solid #273449;
}

.input-area input {
    width: 75%;
    max-width: 750px;
    padding: 16px;
    border-radius: 12px;
    border: 1px solid #334155;
    background: #1e293b;
    color: white;
    outline: none;
    font-size: 16px;
}

.send {
    background: #2563eb;
    color: white;
    min-width: 65px;
}

.send:hover {
    background: #1d4ed8;
}

footer {
    text-align: center;
    padding: 8px;
    color: #64748b;
    font-size: 11px;
}

@media(max-width: 650px) {
    .sidebar {
        width: 65px;
        padding: 10px;
        align-items: center;
    }

    .logo {
        font-size: 17px;
    }

    .sidebar p {
        display: none;
    }

    .new-chat {
        font-size: 11px;
        padding: 10px 5px;
    }

    #messages {
        padding: 15px;
    }

    .welcome h1 {
        font-size: 27px;
    }

    .input-area {
        padding: 12px;
    }

    .input-area input {
        width: 100%;
    }
}
</style>
</head>

<body>

<aside class="sidebar">

    <div class="logo">✦ NovaAI</div>

    <button class="new-chat" onclick="novaChat.novaConversa()">
        + Nova conversa
    </button>

    <p>Sua inteligência artificial pessoal.</p>

</aside>

<main class="main">

    <header>
        ✦ NovaAI <span style="color:#60a5fa">• Assistente</span>
    </header>

    <section id="messages">

        <div class="welcome" id="welcome">
            <h1>Olá! Eu sou a NovaAI ✨</h1>
            <p>Como posso ajudar você hoje?</p>
        </div>

    </section>

    <div class="input-area">

        <input
            id="question"
            type="text"
            placeholder="Pergunte qualquer coisa..."
            autocomplete="off"
        >

        <button class="send" onclick="novaChat.enviar()">
            ➤
        </button>

    </div>

    <footer>
        NovaAI © 2026 — Inteligência artificial experimental.
    </footer>

</main>

<script>

const novaChat = {

    historico: [],

    enviar: function() {

        const input = document.getElementById("question");
        const pergunta = input.value.trim();

        if (!pergunta) return;

        input.value = "";

        const welcome = document.getElementById("welcome");

        if (welcome) welcome.remove();

        this.adicionarMensagem(pergunta, "user");

        this.historico.push({
            usuario: pergunta
        });

        this.mostrarDigitando();

        setTimeout(() => {

            this.removerDigitando();

            const resposta = this.pensar(pergunta);

            this.adicionarMensagem(resposta, "bot");

            this.historico.push({
                assistente: resposta
            });

        }, 700);

    },

    adicionarMensagem: function(texto, tipo) {

        const area = document.getElementById("messages");

        const mensagem = document.createElement("div");

        mensagem.className = "message " + tipo;
        mensagem.textContent = texto;

        area.appendChild(mensagem);

        area.scrollTop = area.scrollHeight;

    },

    mostrarDigitando: function() {

        const area = document.getElementById("messages");

        const digitando = document.createElement("div");

        digitando.id = "digitando";
        digitando.className = "message bot";
        digitando.textContent = "NovaAI está pensando...";

        area.appendChild(digitando);

    },

    removerDigitando: function() {

        const elemento = document.getElementById("digitando");

        if (elemento) elemento.remove();

    },

    pensar: function(pergunta) {

        const texto = pergunta.toLowerCase();

        if (texto.includes("olá") || texto.includes("oi")) {
            return "Olá! 😊 Sou a NovaAI. É um prazer conversar com você!";
        }

        if (texto.includes("seu nome")) {
            return "Meu nome é NovaAI! Sou sua assistente virtual.";
        }

        if (texto.includes("quem criou")) {
            return "Fui desenvolvida como um projeto de inteligência artificial chamado NovaAI.";
        }

        if (texto.includes("hora")) {
            return "A hora atual do seu dispositivo é " +
                new Date().toLocaleTimeString("pt-BR") + ".";
        }

        if (texto.includes("data")) {
            return "Hoje é " +
                new Date().toLocaleDateString("pt-BR") + ".";
        }

        if (texto.includes("ajuda")) {
            return "Claro! Posso conversar com você, responder perguntas simples e ajudar com ideias. O que deseja fazer?";
        }

        if (texto.includes("programação") || texto.includes("código")) {
            return "Programação é a arte de criar instruções para computadores. Posso ajudar você a estudar lógica, HTML, CSS, JavaScript e Python!";
        }

        if (texto.includes("obrigado") || texto.includes("obrigada")) {
            return "Por nada! ❤️ Estou aqui para ajudar.";
        }

        if (texto.includes("tudo bem")) {
            return "Tudo funcionando por aqui! E com você? 😊";
        }

        return "Entendi sua mensagem! 😊\n\nSou a NovaAI, uma assistente experimental. Atualmente consigo responder algumas perguntas predefinidas, mas ainda não estou conectada a um modelo de inteligência artificial generativa.\n\nExperimente perguntar sobre programação, meu nome, data ou horário.";
    },

    novaConversa: function() {

        this.historico = [];

        document.getElementById("messages").innerHTML = `
            <div class="welcome" id="welcome">
                <h1>Olá! Eu sou a NovaAI ✨</h1>
                <p>Como posso ajudar você hoje?</p>
            </div>
        `;

    }

};

document.getElementById("question").addEventListener(
    "keydown",
    function(event) {

        if (event.key === "Enter") {
            novaChat.enviar();
        }

    }
);

</script>

</body>
</html>
