# 🏀 NBA Concessions Optimization Engine

An end-to-end data science project that combines **Machine Learning** (demand forecasting) with **Queueing Theory** (discrete event simulation) to optimize staffing levels at NBA concession stands.

-----

## 📖 Table of Contents

  - [Project Overview](https://www.google.com/search?q=%23project-overview)
  - [The Business Problem](https://www.google.com/search?q=%23the-business-problem)
  - [Methodology](https://www.google.com/search?q=%23methodology)
      - [1. Data Acquisition](https://www.google.com/search?q=%231-data-acquisition)
      - [2. Macro Forecasting](https://www.google.com/search?q=%232-macro-forecasting)
      - [3. Micro Simulation](https://www.google.com/search?q=%233-micro-simulation)
  - [Visualizations & Results](https://www.google.com/search?q=%23visualizations--results)
  - [Tech Stack](https://www.google.com/search?q=%23tech-stack)
  - [Getting Started](https://www.google.com/search?q=%23getting-started)
  - [Future Improvements](https://www.google.com/search?q=%23future-improvements)

-----

## 🔍 Project Overview

This repository contains a simulation engine designed to solve the "Halftime Bottleneck"—the operational challenge where concession demand spikes dramatically for a short window, often overwhelming staff and causing revenue loss due to long wait times.

The system scrapes real NBA schedule data, forecasts attendance-driven demand, and runs a minute-by-minute simulation of queue dynamics to recommend the optimal number of servers per stand.

## 💼 The Business Problem

Stadium operations managers face two distinct prediction problems:

1.  **Macro:** "How much inventory do we need tonight?" (Forecasting total sales volume).
2.  **Micro:** "How many staff do we need at Stand 4 during halftime?" (Managing queue throughput).

**Goal:** Minimize customer wait times to \<5 minutes while minimizing labor costs.

-----

## ⚙️ Methodology

### 1\. Data Acquisition

  * **Web Scraping:** Built a custom scraper using `requests` and `BeautifulSoup` to ingest the full 2024 NBA Schedule from Basketball-Reference.com.
  * **Feature Engineering:** Parsed raw dates into `Start_Time_ET` (to separate afternoon/night games), `Day_of_Week` (to flag weekends), and calculated game durations.

### 2\. Macro Forecasting (ML)

  * **Target:** Total items sold per game.
  * **Model:** Random Forest Regressor.
  * **Performance:** Achieved an **R² of 0.86**.
  * **Key Insight:** Attendance is the primary driver, but weekend games show a statistically significant 15% "uplift" in sales per capita compared to weekday games.

### 3\. Micro Simulation (Queueing Theory)

  * **Time Profiling:** Converted the macro sales forecast into a **Time-Decay Curve**, modeling specific event spikes (Pre-game, Halftime, Post-game).
  * **Discrete Event Simulator:** Built a custom Python simulator to model the physics of a waiting line.
      * **Input:** Minute-by-minute arrivals ($\lambda$).
      * **Process:** Stochastic service rates ($\mu$) based on staff count.
      * **Output:** Dynamic queue length ($L$) and wait times ($W$) via **Little's Law**.

-----

## 📊 Visualizations & Results

### The "Death Spiral" of Under-staffing

The simulation revealed a non-linear relationship between staff count and wait times.

  * **2-3 Servers:** The queue grows asymptotically; wait times exceed 60+ minutes (Operational Failure).
  * **5-6 Servers:** The "Elbow" of the curve. Wait times stabilize at \~3-5 minutes.
  * **8+ Servers:** Diminishing returns. Adding staff saves seconds, not minutes.

[Image of poisson distribution graph]

*(Example: Poisson arrival distribution used to model customer flow)*

-----

## 🛠 Tech Stack

  * **Language:** Python 3.9
  * **Data Processing:** Pandas, NumPy
  * **Machine Learning:** Scikit-Learn (Random Forest)
  * **Web Scraping:** BeautifulSoup4, Requests
  * **Visualization:** Plotly Express (Interactive heatmaps)

-----

## 🚀 Getting Started

### Prerequisites

```bash
pip install pandas numpy scikit-learn plotly beautifulsoup4 requests lxml
```

### Usage

1.  **Clone the repo:**
    ```bash
    git clone https://github.com/Rakabi007/Analyzing-NBA-Stadium-Concession-Staffing-Needs-and-Minimizing-the-Time-it-Takes-to-Serve-A-Customer.git
    ```
2.  **Run the analysis:**
    Open the Jupyter Notebook `NBA_Concessions_Analysis.ipynb` to run the full pipeline from scraping to simulation.

-----

## 🔮 Future Improvements

  * **Cost Optimization:** Integrate a "Cost of Labor" vs. "Cost of Lost Revenue" function to output a purely financial recommendation.
  * **Menu Complexity:** Add a variable for "Service Time Complexity" (e.g., pouring a beer is faster than making a complex food order).
  * **Real-time Data:** Connect to an API for live game data updates.



-----

**Star this repo if you found the simulation logic interesting\!** ⭐
