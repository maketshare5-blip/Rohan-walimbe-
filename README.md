# Rohan-walimbe-
import streamlit as st
from flatlib import datetime, geopos
from flatlib.chart import Chart
from flatlib import aspect
import matplotlib.pyplot as plt

--- App Styling ---
st.set_page_config(page_title="Astro-Trade Analytics", layout="wide")
st.title("ðŸ¹ Varunya Astro-Trading Intelligence")

--- Sidebar Inputs ---
with st.sidebar:
st.header("ðŸ“ Data Input")
dob = st.date_input("Select Date")
tob = st.time_input("Select Time")
lat = st.number_input("Lat", value=28.61)
lon = st.number_input("Lon", value=77.2)

st.divider()
mode = st.radio("Analysis Mode", ["Personal Kundli", "Market Sentiment"])
if st.sidebar.button("Analyze Flow"):
# Core Astrology Engine
date = datetime.Date(dob.strftime('%Y/%m/%d'), tob.strftime('%H:%M'), "+05:30")
pos = geopos.GeoPos(lat, lon)
chart = Chart(date, pos)

col1, col2, col3 = st.columns([1, 1, 1.2])

with col1:
    st.subheader("ðŸª Planet Degrees")
    # List comprehension for clean data
    p_list = [{"Planet": obj.id, "Sign": obj.sign, "Deg": round(obj.angle, 2)} for obj in chart.objects]
    st.dataframe(p_list, use_container_width=True)

with col2:
    st.subheader("ðŸ“Š Chart Visualization")
    fig, ax = plt.subplots(figsize=(5, 5))
    # Logic for Diamond Chart
    ax.plot([0, 5, 10, 5, 0], [5, 10, 5, 0, 5], color='darkorange', lw=3)
    ax.plot([0, 10], [10, 0], color='gray', alpha=0.3)
    ax.plot([0, 10], [0, 10], color='gray', alpha=0.3)
    
    lagna = chart.get(chart.ASC)
    ax.text(5, 5.5, f"{lagna.sign}", fontsize=15, ha='center', fontweight='bold')
    ax.text(5, 4.5, "1st House", fontsize=8, ha='center', color='gray')
    ax.axis('off')
    st.pyplot(fig)

with col3:
    st.subheader("ðŸ§  Trading Psychology Insight")
    
    # Simple Logic: Moon represents Mind (Psychology)
    moon = chart.get('Moon')
    mercury = chart.get('Mercury') # Logic/Calculation
    
    # Analyzing Mercury (Trading/Broking/Logic)
    if mercury.sign in ['Gemini', 'Virgo']:
        st.success("âœ… **Strong Logic:** Today is good for data-driven decisions.")
    elif mercury.sign in ['Pisces']:
        st.warning("âš ï¸ **Confused Mind:** Mercury is weak. Avoid over-trading.")

    # Psychology Tip based on Moon
    st.info(f"**Current Mindset:** Your Moon is in {moon.sign}. Manage your emotions accordingly.")
    
    # Adding a Gauge for Risk (Static example, can be calculated)
    st.metric("Market Volatility Score", "Medi
