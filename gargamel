import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# --- Configurações da Página ---
st.set_page_config(
    page_title="Formulário de Registro de Dados",
    page_icon="📝",
    layout="centered",
    initial_sidebar_state="auto"
)

# --- Título e Subtítulo ---
st.title("Sistema de Registro de Usuários")
st.markdown("""
    Este formulário permite registrar dados de **Sexo** e **Idade** e visualizar estatísticas em tempo real.
    Desenvolvido com foco em **Usabilidade** e **Design de Interação Humano-Computador (IHC)**.
""")

# --- Inicialização da Sessão de Dados ---
# Usamos st.session_state para manter os dados persistentes entre as interações do usuário
if 'data' not in st.session_state:
    st.session_state.data = pd.DataFrame(columns=['Sexo', 'Idade', 'Timestamp'])

# --- Formulário de Entrada de Dados ---
st.header("📝 Registrar Novo Usuário")
with st.form(key='registration_form', clear_on_submit=True):
    col1, col2 = st.columns(2)
    with col1:
        sexo = st.selectbox("Sexo:", ["Masculino", "Feminino", "Outro", "Prefiro não dizer"], help="Selecione o sexo do usuário.")
    with col2:
        idade = st.number_input("Idade:", min_value=0, max_value=120, value=25, step=1, help="Insira a idade do usuário (entre 0 e 120 anos).")

    st.markdown("---") # Separador visual para melhor organização
    submit_button = st.form_submit_button(label="✅ Registrar Dados", use_container_width=True)

    if submit_button:
        if sexo and idade is not None:
            new_data = pd.DataFrame([{'Sexo': sexo, 'Idade': idade, 'Timestamp': pd.Timestamp.now()}])
            st.session_state.data = pd.concat([st.session_state.data, new_data], ignore_index=True)
            st.success("Dados registrados com sucesso!")
        else:
            st.error("Por favor, preencha todos os campos antes de registrar.")

# --- Exibição da Tabela de Dados ---
st.header("📊 Dados Registrados")
if not st.session_state.data.empty:
    # A tabela é interativa e permite ordenação
    st.dataframe(st.session_state.data.sort_values(by='Timestamp', ascending=False).reset_index(drop=True))
    st.markdown(f"Total de registros: **{len(st.session_state.data)}**")

    # Botão para limpar todos os registros
    if st.button("🗑️ Limpar Todos os Registros", help="Apaga todos os dados da tabela e dos gráficos.", type="secondary"):
        st.session_state.data = pd.DataFrame(columns=['Sexo', 'Idade', 'Timestamp'])
        st.success("Todos os registros foram apagados.")
        st.experimental_rerun() # Recarrega a página para refletir a limpeza
else:
    st.info("Nenhum dado registrado ainda. Utilize o formulário acima para começar!")

# --- Geração de Gráficos Estatísticos ---
st.header("📈 Estatísticas dos Registros")

if not st.session_state.data.empty:
    # Ajustando o estilo dos gráficos para uma estética formal
    sns.set_style("whitegrid")
    plt.style.use('seaborn-v0_8-deep') # Um estilo bonito e formal do matplotlib

    # Gráfico 1: Distribuição de Sexo
    st.subheader("Distribuição por Sexo")
    fig1, ax1 = plt.subplots(figsize=(8, 5))
    sex_counts = st.session_state.data['Sexo'].value_counts()
    sex_counts.plot(kind='bar', ax=ax1, color=sns.color_palette("pastel"))
    ax1.set_title('Contagem de Registros por Sexo', fontsize=14, fontweight='bold')
    ax1.set_xlabel('Sexo', fontsize=12)
    ax1.set_ylabel('Número de Registros', fontsize=12)
    ax1.tick_params(axis='x', rotation=45)
    plt.tight_layout() # Ajusta o layout para evitar sobreposição
    st.pyplot(fig1)

    # Gráfico 2: Distribuição de Idade (Histograma)
    st.subheader("Distribuição de Idade")
    fig2, ax2 = plt.subplots(figsize=(8, 5))
    sns.histplot(st.session_state.data['Idade'], bins=10, kde=True, ax=ax2, color=sns.color_palette("dark")[0])
    ax2.set_title('Distribuição de Idade dos Usuários', fontsize=14, fontweight='bold')
    ax2.set_xlabel('Idade', fontsize=12)
    ax2.set_ylabel('Frequência', fontsize=12)
    plt.tight_layout()
    st.pyplot(fig2)

    st.markdown("---")
    st.info("Os gráficos são atualizados automaticamente com cada novo registro.")
else:
    st.warning("Não há dados suficientes para gerar os gráficos. Registre alguns usuários primeiro!")

# --- Rodapé ---
st.markdown("---")
st.markdown("""
    <p style='text-align: center; color: grey;'>
        Desenvolvido com ❤️ por Seu Especialista em Python e Streamlit
    </p>
""", unsafe_allow_html=True)
