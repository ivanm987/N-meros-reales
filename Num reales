import streamlit as st
import random
import time

# Lista de números y sus equivalencias
numbers = [
    ("π", "3.14"), ("√2", "1.41"), ("√3", "1.73"),
    ("√5", "2.24"), ("1/3", "0.33"), ("0.5", "1/2"),
    ("5/10", "0.5"), ("e", "2.72"), ("7/4", "1.75"),
    ("10%", "0.1"), ("2/5", "0.4"), ("1.25", "5/4")
]

# Duplicar valores para hacer pares y barajarlos
cards = numbers + [(b, a) for a, b in numbers]
random.shuffle(cards)

# Configuración de estado en Streamlit
if "flipped" not in st.session_state:
    st.session_state.flipped = [False] * 25  # Estado de cada carta
if "selected" not in st.session_state:
    st.session_state.selected = []  # Cartas seleccionadas
if "matched" not in st.session_state:
    st.session_state.matched = set()  # Índices de cartas encontradas
if "grid" not in st.session_state:
    st.session_state.grid = cards  # Asignación de cartas

# Función para voltear una carta
def flip_card(idx):
    if idx not in st.session_state.matched and idx not in st.session_state.selected:
        st.session_state.selected.append(idx)
        st.session_state.flipped[idx] = True

        # Verificar si hay dos cartas seleccionadas
        if len(st.session_state.selected) == 2:
            time.sleep(0.5)  # Pequeño retraso para que el jugador vea la carta
            idx1, idx2 = st.session_state.selected
            if st.session_state.grid[idx1][0] == st.session_state.grid[idx2][1]:
                st.session_state.matched.update([idx1, idx2])
            else:
                st.session_state.flipped[idx1] = st.session_state.flipped[idx2] = False
            st.session_state.selected.clear()

# Mostrar el tablero de cartas
st.title("🃏 Juego de Memoria: Números Reales")
st.write("Encuentra los pares de números equivalentes.")

cols = 5
for i in range(0, 25, cols):
    row = st.columns(cols)
    for j in range(cols):
        idx = i + j
        if idx < len(st.session_state.grid):
            if idx in st.session_state.matched or st.session_state.flipped[idx]:
                row[j].button(st.session_state.grid[idx][0], key=idx, disabled=True)
            else:
                if row[j].button("🔲", key=idx):
                    flip_card(idx)

# Verificar si el juego ha terminado
if len(st.session_state.matched) == 24:
    st.success("🎉 ¡Felicidades! Has encontrado todos los pares.")
