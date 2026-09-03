<div align="center">

# 🧠 Mental Health Signal

### Predicting student mental wellness from social media & lifestyle habits

*A full-stack machine learning app — from raw data to a live, deployed prediction API and UI.*  

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Render](https://img.shields.io/badge/Deployed_on-Render-46E3B7?style=for-the-badge&logo=render&logoColor=white)](https://render.com/)

**[🚀 Live App](https://mental-health-predictor-1-fw9u.onrender.com)** &nbsp;•&nbsp; **[📘 API Docs](https://mental-health-predictor-ow9r.onrender.com/docs)** &nbsp;•&nbsp; **[🐛 Report a Bug](../../issues)**

</div>    
    
<br>             
    
## 📖 Overview      

**Mental Health Signal** predicts a student's mental wellness score (0–10) from their digital habits — screen time, sleep, stress, study load, and platform usage. It's built end-to-end: exploratory analysis on 5,000 student records, a leak-free preprocessing + modeling pipeline, a tuned Random Forest regressor, a FastAPI backend, and a clean, animated frontend that turns a prediction into a readable "signal."
   
> ⚠️ **Not a clinical tool.** This is a data-driven reflection of habits, not a diagnosis. If you're struggling, please talk to someone you trust or a mental health professional.
   
<br>   
   
## ✨ Features
    
| | |
|---|---|
| 🎯 **Real-time predictions** | Submit your habits, get an instant 0–10 wellness score |
| 🔬 **Two models compared** | Linear Regression baseline vs. tuned Random Forest |
| 🧹 **Careful data cleaning** | Outlier detection, invalid-value correction, skew handling |
| 🌍 **Smart encoding** | High-cardinality `Country` (111 values) grouped intelligently instead of blindly one-hot encoded |
| 🛡️ **Full validation** | Pydantic schema validation on the backend, live inline validation on the frontend |
| 🎨 **Polished UI** | Animated gauge, segmented controls, loading/error states — no generic Bootstrap form |
| ⚡ **Single-pipeline deployment** | Preprocessing + model saved as one `.pkl`, so the API just loads and predicts |

<br>

## 🖥️ Live Demo

| | |
|---|---|
| **Frontend** | [mental-health-predictor-1-fw9u.onrender.com](https://mental-health-predictor-1-fw9u.onrender.com) |
| **API + Swagger Docs** | [mental-health-predictor-ow9r.onrender.com/docs](https://mental-health-predictor-ow9r.onrender.com/docs) |

> Hosted on Render's free tier — the first request after inactivity may take ~30–60s to spin up. Thanks for your patience! ☕

<br>

## 🧰 Tech Stack

```
Data & Modeling     Pandas · NumPy · scikit-learn · Matplotlib · Seaborn
Backend             FastAPI · Pydantic · Uvicorn · Joblib
Frontend            HTML5 · CSS3 · Vanilla JavaScript (no framework)
Deployment          Render
```

<br>

## 🧪 How It Was Built

<table>
<tr><td width="40" align="center">1️⃣</td><td><b>Exploratory Data Analysis</b><br>Distribution of the target, correlation heatmap, stress vs. score, screen time vs. score, sleep vs. score, platform popularity.</td></tr>
<tr><td align="center">2️⃣</td><td><b>Data Cleaning</b><br>Fixed a data-entry glitch (negative <code>Physical_Activity_Hours</code>) with <code>.clip()</code>, removed duplicates, checked outliers via IQR.</td></tr>
<tr><td align="center">3️⃣</td><td><b>Feature Engineering</b><br>Grouped 111 raw <code>Country</code> values into the top 10 + "Other" — kept the signal, dropped the noise.</td></tr>
<tr><td align="center">4️⃣</td><td><b>Encoding Strategy</b><br><code>Stress_Level</code> → ordinal (Low < Medium < High < Very High). Nominal categories → one-hot. Skewed <code>Study_Hours</code> → log-transformed + scaled.</td></tr>
<tr><td align="center">5️⃣</td><td><b>Modeling</b><br>Linear Regression baseline vs. Random Forest, tuned with <code>RandomizedSearchCV</code> (5-fold CV, 15 iterations).</td></tr>
<tr><td align="center">6️⃣</td><td><b>Deployment</b><br>Full pipeline (preprocessing + model) serialized with Joblib, served via FastAPI, consumed by a static frontend.</td></tr>
</table>

<br>

## 📊 Model Performance

| Model | Test R² | Train R² | MAE | RMSE |
|---|:---:|:---:|:---:|:---:|
| Linear Regression | 0.740 | 0.724 | 0.536 | 0.676 |
| Random Forest (default) | 0.878 | 0.981 | 0.347 | 0.464 |
| **Random Forest (tuned)** | **0.865** | 0.955 | 0.369 | 0.487 |

The tuned Random Forest trades a touch of raw test R² for a much smaller train/test gap — meaning it generalizes more honestly rather than memorizing the training set.

**Best hyperparameters:**
```python
{
    'n_estimators': 200,
    'min_samples_split': 5,
    'min_samples_leaf': 2,
    'max_depth': 15
}
```

<br>

## 🚀 Running It Locally

### 1. Clone & install
```bash
git clone https://github.com/<your-username>/mental-health-predictor.git
cd mental-health-predictor
pip install -r requirements.txt
```

### 2. Start the API
```bash
uvicorn main:app --reload --port 8000
```
The interactive Swagger docs will be live at `http://127.0.0.1:8000/docs`.

### 3. Open the frontend
Just open `index.html` in your browser — it talks to the local API automatically.

<br>

## 🔌 API Reference

**`POST /predict`**

<details>
<summary>Request body</summary>

```json
{
  "age": 21,
  "gender": "Male",
  "country": "India",
  "academic_level": "Undergraduate",
  "most_used_platform": "Instagram",
  "purpose_of_use": "Entertainment",
  "avg_daily_usage_hours": 4.5,
  "daily_unlocks": 60,
  "study_hours": 3.0,
  "physical_activity_hours": 1.0,
  "sleep_hours_per_night": 6.5,
  "stress_level": "High"
}
```
</details>

<details>
<summary>Response</summary>

```json
{
  "predicted_mental_health_score": 6.42
}
```
</details>

Full interactive documentation, including validation rules and try-it-now testing, is available at [`/docs`](https://mental-health-predictor-ow9r.onrender.com/docs).

<br>

## 📁 Project Structure

```
├── ML_Project.ipynb                                    # EDA → cleaning → modeling notebook
├── Mental_Health_Model.pkl                             # Saved sklearn pipeline (preprocessing + model)
├── Student_Social_Media_And_Mental_Health_Impact.csv   # Dataset (5,000 rows)
├── main.py                                             # FastAPI backend
├── index.html                                          # Frontend markup
├── style.css                                           # Frontend styling
├── script.js                                           # Frontend logic
├── requirements.txt
└── README.md
```

<br>

## 🗺️ Roadmap

- [ ] Feature importance / SHAP explanations in the UI ("what drove my score")
- [ ] Cross-validated scoring for the baseline models
- [ ] Swap deployed model to `rf_best_pipeline` (tuned) for consistency with the reported results
- [ ] Add crisis-resource links in the UI footer

<br>

## ⚠️ Disclaimer

This project is for **educational and informational purposes only**. It is not a diagnostic or clinical tool, and predictions should not be used as a substitute for professional mental health advice. If you or someone you know is struggling, please reach out to a mental health professional or a trusted person in your life.

<br>

<div align="center">

**Built as a first end-to-end machine learning project** — data cleaning to deployed API, all in one repo.

⭐ If this helped you, consider starring the repo!

</div>
