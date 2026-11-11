python
import streamlit as st
import pandas as pd

# ==========================================
# NUTRIATHLETE - SISTEMA DE NUTRIÇÃO ESPORTIVA
# ==========================================

st.set_page_config(
    page_title="NutriAthlete",
    page_icon="🏋️",
    layout="wide"
)

# ESTILO VISUAL
st.markdown("""
<style>
.stApp {
    background-color: #101820;
    color: white;
}
h1, h2, h3 {
    color: #00DFA2;
}
.stButton>button {
    background-color: #00DFA2;
    color: black;
    border-radius: 10px;
    font-weight: bold;
}
</style>
""", unsafe_allow_html=True)

# TÍTULO
st.title("🏋️ NUTRIATHLETE")
st.subheader("Sistema Inteligente de Nutrição Esportiva")

st.markdown("---")

# CADASTRO DO ATLETA
st.header("👤 Cadastro do Atleta")

col1, col2 = st.columns(2)

with col1:
    nome = st.text_input("Nome do atleta")
    idade = st.number_input("Idade", 15, 100, 25)
    sexo = st.selectbox("Sexo", ["Masculino", "Feminino"])
    peso = st.number_input("Peso (kg)", 30.0, 250.0, 75.0)

with col2:
    altura = st.number_input("Altura (m)", 1.30, 2.30, 1.75)
    atividade = st.selectbox(
        "Nível de atividade",
        ["Sedentário", "Leve", "Moderado", "Intenso", "Muito intenso"]
    )

    objetivo = st.selectbox(
        "Objetivo esportivo",
        ["Hipertrofia", "Emagrecimento", "Manutenção"]
    )

    modalidade = st.selectbox(
        "Modalidade esportiva",
        ["Musculação", "Corrida", "Futebol", "Natação",
         "Ciclismo", "Crossfit", "Outro"]
    )

# FATORES DE ATIVIDADE
fatores = {
    "Sedentário": 1.2,
    "Leve": 1.375,
    "Moderado": 1.55,
    "Intenso": 1.725,
    "Muito intenso": 1.9
}

# CÁLCULOS
if st.button("CALCULAR PLANEJAMENTO NUTRICIONAL"):

    if not nome.strip():
        st.error("Informe o nome do atleta.")
        st.stop()

    altura_cm = altura * 100

    # IMC
    imc = peso / (altura ** 2)

    # METABOLISMO BASAL
    if sexo == "Masculino":
        tmb = (10 * peso) + (6.25 * altura_cm) - (5 * idade) + 5
    else:
        tmb = (10 * peso) + (6.25 * altura_cm) - (5 * idade) - 161

    # GASTO ENERGÉTICO
    gasto = tmb * fatores[atividade]

    # AJUSTE DE CALORIAS
    ajustes = {
        "Hipertrofia": 300,
        "Emagrecimento": -300,
        "Manutenção": 0
    }

    calorias = gasto + ajustes[objetivo]

    # MACRONUTRIENTES ESTIMADOS
    proteina = peso * 2
    gordura = peso * 0.9

    carboidrato = (
        calorias - (proteina * 4) - (gordura * 9)
    ) / 4

    carboidrato = max(0, carboidrato)

    # RESULTADOS
    st.markdown("---")
    st.header("📊 Resultado Nutricional")

    c1, c2, c3, c4 = st.columns(4)

    c1.metric("IMC", f"{imc:.2f}")
    c2.metric("Metabolismo basal", f"{tmb:.0f} kcal")
    c3.metric("Gasto energético", f"{gasto:.0f} kcal")
    c4.metric("Meta calórica", f"{calorias:.0f} kcal")

    st.markdown("---")

    st.header("🥗 Distribuição de Macronutrientes")

    m1, m2, m3 = st.columns(3)

    m1.metric("Proteínas", f"{proteina:.1f} g")
    m2.metric("Carboidratos", f"{carboidrato:.1f} g")
    m3.metric("Gorduras", f"{gordura:.1f} g")

    # PLANO ALIMENTAR ILUSTRATIVO
    st.markdown("---")
    st.header("🍽️ Planejamento Alimentar")

    refeicoes = {
        "Café da manhã": [
            "Ovos",
            "Pão integral",
            "Fruta",
            "Leite ou iogurte"
        ],
        "Lanche da manhã": [
            "Fruta",
            "Iogurte natural",
            "Aveia"
        ],
        "Almoço": [
            "Arroz",
            "Feijão",
            "Frango ou peixe",
            "Salada e legumes"
        ],
        "Pré-treino": [
            "Banana",
            "Aveia",
            "Pão ou outra fonte de carboidrato"
        ],
        "Pós-treino": [
            "Proteína alimentar",
            "Batata ou arroz",
            "Fruta"
        ],
        "Jantar": [
            "Carne magra ou ovos",
            "Arroz ou batata",
            "Legumes"
        ],
        "Ceia": [
            "Iogurte natural",
            "Fruta ou outra opção proteica"
        ]
    }

    for refeicao, alimentos in refeicoes.items():
        with st.expander(refeicao):
            for alimento in alimentos:
                st.write("•", alimento)

    # GRÁFICO DE MACRONUTRIENTES
    st.markdown("---")
    st.header("📈 Gráfico Nutricional")

    dados = pd.DataFrame({
        "Macronutriente": ["Proteínas", "Carboidratos", "Gorduras"],
        "Quantidade (g)": [proteina, carboidrato, gordura]
    })

    st.bar_chart(
        dados.set_index("Macronutriente")
    )

    # RESUMO
    st.markdown("---")
    st.header("📋 Relatório do Atleta")

    relatorio = f"""
    NUTRIATHLETE - RELATÓRIO NUTRICIONAL

    Atleta: {nome}
    Idade: {idade}
    Sexo: {sexo}
    Peso: {peso} kg
    Altura: {altura} m
    Modalidade: {modalidade}
    Objetivo: {objetivo}

    IMC: {imc:.2f}
    Metabolismo basal: {tmb:.0f} kcal
    Gasto energético: {gasto:.0f} kcal

    Meta calórica estimada: {calorias:.0f} kcal

    PROTEÍNAS: {proteina:.1f} g
    CARBOIDRATOS: {carboidrato:.1f} g
    GORDURAS: {gordura:.1f} g

    Projeto NutriAthlete
    """

    st.download_button(
        "📥 BAIXAR RELATÓRIO",
        relatorio,
        file_name="relatorio_atleta.txt"
    )

    st.success("Planejamento calculado com sucesso!")

st.markdown("---")

st.caption(
    "NutriAthlete © 2026 | Sistema educacional de nutrição esportiva"
)

st.warning(
    "Os cálculos são estimativas educativas, não prescrições nutricionais. "
    "O planejamento alimentar individual deve ser validado por nutricionista. "
    "O sistema não deve ser utilizado como prescrição automática para menores."
)
