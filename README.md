# 🩺 Health Risk Evaluation using TS Fuzzy Logic Controller

## 📌 Overview

This project implements a **Takagi-Sugeno (TS) Fuzzy Logic Controller** to evaluate a person's health risk based on:

* Blood Pressure (BP)
* Cholesterol Level

The system computes a **TS Score** and classifies the health condition into:

* Low Risk
* Moderate Risk
* High Risk

---

## ⚙️ How It Works

### 1. Fuzzification

Input values (BP and Cholesterol) are converted into fuzzy membership values:

* **Blood Pressure**

  * Low
  * Normal
  * High

* **Cholesterol**

  * Low
  * Medium
  * High

---

### 2. Rule Base

The system uses fuzzy rules:

| Rule | Condition                 | Output |
| ---- | ------------------------- | ------ |
| R1   | BP Low AND Chol Low       | 10     |
| R2   | BP Low AND Chol Medium    | 30     |
| R3   | BP Normal AND Chol Medium | 50     |
| R4   | BP High AND Chol High     | 90     |

---

### 3. Inference Mechanism

Weights are calculated using:

* Product of membership values

---

### 4. Defuzzification

Final TS Score is calculated using:

```
TS Score = (Σ wi * yi) / (Σ wi)
```

Where:

* wi = rule firing strength
* yi = rule output

---

### 5. Decision Making

Based on TS Score:

* **> 70** → High Risk
* **40 – 70** → Moderate Risk
* **< 40** → Low Risk

---

## 💻 Code Implementation

```python
def ts_health(bp, chol):
    mu_bp_low = max(0, (120 - bp) / 40)
    mu_bp_normal = max(0, 1 - abs(bp - 120) / 40)
    mu_bp_high = max(0, (bp - 120) / 40)

    mu_ch_low = max(0, (200 - chol) / 100)
    mu_ch_med = max(0, 1 - abs(chol - 200) / 100)
    mu_ch_high = max(0, (chol - 200) / 100)

    w1 = mu_bp_low * mu_ch_low
    w2 = mu_bp_low * mu_ch_med
    w3 = mu_bp_normal * mu_ch_med
    w4 = mu_bp_high * mu_ch_high

    y1, y2, y3, y4 = 10, 30, 50, 90

    numerator = w1*y1 + w2*y2 + w3*y3 + w4*y4
    denominator = w1 + w2 + w3 + w4

    if denominator == 0:
        return 0

    ts_score = numerator / denominator

    if ts_score > 70:
        verdict = "High Risk"
    elif ts_score > 40:
        verdict = "Moderate Risk"
    else:
        verdict = "Low Risk"

    return round(ts_score, 2), verdict
```

---

## 📊 Sample Inputs

```
(150, 250)
(130, 210)
(120, 200)
(100, 170)
(90, 150)
```

---

## ▶️ How to Run

1. Clone the repository:

```
git clone https://github.com/your-username/ts-health-flc.git
```

2. Run the Python script:

```
python ts_health.py
```

---

## 📈 Example Output

```
------ Health Risk Evaluation (TS FLC) ------

--- Health Report ---
BP          : 150
Cholesterol : 250
TS Score    : 90.0
Risk        : High Risk
```

---

## 🚀 Features

* Simple and intuitive TS fuzzy model
* Lightweight Python implementation
* Easy to extend with more rules
* Useful for academic demonstrations

---

## 📚 Future Improvements

* Add more health parameters (BMI, sugar level)
* GUI using Tkinter / Streamlit
* Visualization of membership functions
* Real dataset integration

---

## 👩‍💻 Author

Hemdatta Das

---

## 📄 License

This project is open-source and available under the MIT License.

