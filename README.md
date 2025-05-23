# Maxcare-Medical-Analysis
Max Care Hospital Admission Analysis This project analyzes real-time data to uncover trends in patient admissions, waiting times, health conditions, repeat visits, and departmental loads. Built in Power BI with 30+ DAX measures, custom visuals, and stakeholder-focused insights. Designed to support operational decisions and improve best delivery.


# 🏥 Max Care Hospital - Patient Admission Analysis (Power BI Project)

### 📘 Project Walkthrough & Documentation
the below data is filtered for the Month Feb 2018 but there is latest than that

## 📌 Stakeholder Questions & Story-Based Insights

### 📌 Executive Summary

**Q: How many total patients were admitted in February 2018?**  
📍 A total of **592** patients were admitted. This forms the core of operational capacity monitoring. The number indicates a moderate flow of patients for that month.

**Q: What is the Month-over-Month (MoM) change in total admissions?**  
📈 Patient admissions increased by **+3.0% vs Last Month**, signaling a growing demand for hospital services.

---

### 🕒 Efficiency & Process

**Q: What is the average patient waiting time, and how has it changed from last month?**  
⏱ The average waiting time reached **133.9 minutes**, which marks an **18.2% increase** compared to the previous month. This spike suggests an increase in patient load or operational bottlenecks.

**Q: Is the waiting time increasing in a concerning way? Which departments contribute most to the delay?**  
Yes — departments like **Cardiology and ENT** have high patient loads, possibly leading to longer queues. This points to the need for resource reallocation or schedule optimization.

---

### 🧑‍⚕️ Specialization & Capacity

**Q: Which medical specializations had the highest patient visits this month?**  
🩺 The most visited departments were **Cardiology**, **Dermatology**, and **ENT**. These areas are the backbone of current patient care demand.

**Q: Are there any underutilized or overburdened departments?**  
Yes. Departments like **Nephrology and General Surgery** showed relatively lower patient volume. Meanwhile, **ENT and General Medicine** were nearing overcapacity.

---

### ⚠️ Patient Risk & Health Conditions

**Q: How many patients were diagnosed with heart failure in February?**  
💔 There were **200 heart failure cases**, down by **2.4%** compared to last month. This drop may indicate improved preventive care or earlier detection.

**Q: What is the overall distribution of patient health status?**  
📊 Among the patients, **85%** were stable, **6%** needed assistance, and **9%** were deceased. This helps gauge not just quantity but quality of care.

**Q: Is there any pattern in heart failure by age or gender?**  
Most heart failure patients are in the **60–80** age bracket. Males slightly outnumber females in these cases, revealing a demographic risk trend.

---

### 📍 Demographics & Location

**Q: What is the gender distribution among patients?**  
👨 **7811 male** vs 👩 **4432 female** patients. A skewed distribution that may relate to cultural or accessibility factors in healthcare-seeking behavior.

**Q: How does patient inflow differ between rural and urban areas?**  
📍 **77% of patients came from rural areas**, with **23% from urban** zones. This emphasizes the hospital's role in rural outreach and care delivery.

**Q: Is the rural share of patients growing or declining?**  
📉 The rural patient share slightly declined this month (⇩ -2.4%), warranting an investigation into referral patterns or seasonal trends.

---

### 📆 Time & Frequency Analysis

**Q: Which weekdays have the highest patient admissions?**  
📅 **Tuesday and Wednesday** witnessed the most patient flow, particularly from the **40–80** age group. These can be targeted for more staff coverage.

**Q: Do specific age groups dominate certain days, and can this guide scheduling?**  
Yes — working-age adults prefer early weekdays, whereas elderly patients spread across mid to late week. Clinics can plan staffing accordingly.

---

### 🔁 Visit Type & Repeat Patterns

**Q: What is the ratio between OPD and Emergency visits?**  
🚨 OPD accounted for **69%**, while **Emergency visits were 31%**. This shows that most cases are manageable with routine care, reducing critical load.

**Q: Are repeat or follow-up patients being tracked, and how are they trending?**  
🔄 Yes — Follow-up visits rose by **+2.9%**, indicating strong patient retention and chronic case management.

---

### 🔎 Behavioral & Age Risk

**Q: How many patients show risk behaviors by age group?**  
🧠 Middle-aged and elderly patients (40–80, 60–80) lead in risky behaviors. These include habits linked to cardiac issues and require targeted awareness.

**Q: Do we need targeted awareness or screening for specific age brackets?**  
Absolutely. Screening programs should prioritize patients between **40–80 years**, as they carry the highest cumulative risk.

---

### 📈 Strategic & Comparative

**Q: What are the top insights when comparing this month to the last?**  
📈 Patient count is up, but so is waiting time. Heart failure cases slightly fell. These mixed signals show improved outcomes but slower throughput.

**Q: How can we reduce average wait times without compromising care quality?**  
🎯 Introduce digital pre-consultation, review high-traffic specializations for load balancing, and consider part-time consultants during peak days.

---
## 🛠 Project Workflow Overview

### 🔹 Step 1: Initial Setup via Notepad
This project was carefully tracked from start to finish using Notepad, Each transformation, formula, and design decision was stored live during development to maintain clarity, versioning, and accountability.

---

### 🔹 Step 2: Data Collection
- Source: **Real-time patient data** from Max Care Hospital, Warangal
- Content: Admission date, discharge date, age, gender, specialization, patient condition, visit type, and doctor consultation info.

---

### 🔹 Step 3: Data Cleaning & Transformation (Power Query)
- Changed date format from **MM-dd-yyyy** ➝ **dd-MM-yyyy**
- Replaced "Empty" strings in numeric fields with **null**
- Standardized:
  - Gender: `m` → `Men`, `f` → `Female`
  - Locality: `u` → `Urban`, `r` → `Rural`
- Removed unrelated fields
- Deleted exact duplicates, retaining re-consultation data based on: **Admission No + Date of Admission + Date of Discharge**
- Combined tables using **VLOOKUP** between Patient and Doctor datasets

---

### 🔹 Step 4: Power BI Data Modeling
- Performed **quality check** in Power Query
- Created new **Date Table** using:
```dax
Date = CALENDAR(MIN('HDHI Admission data'[D.O.A]), MAX('HDHI Admission data'[D.O.A]))
```
- Extracted additional columns:
  - Month Name: `FORMAT(Date, "mmm")`
  - Month Number: `MONTH(Date)`
  - Year: `YEAR(Date)`
  - Weekday Name: `FORMAT(Date, "ddd")`

- Created similar logic for **Discharge Dates**

---

### 🔹 Step 5: DAX Measures (30+ Used)
Examples include:

#### ➤ Month-To-Date Patients
```dax
CM Patients = 
VAR select_month = SELECTEDVALUE('Admission Date Table'[Month Name])
RETURN
TOTALMTD(
  CALCULATE(DISTINCTCOUNT('HDHI Admission data'[Admission No]), 
  'Admission Date Table'[Month Name] = select_month), 
  'Admission Date Table'[Date of Admission]
)
```

#### ➤ Month-over-Month Comparison
```dax
MoM = 
VAR momdiff = [CM Patients] - [PvM Patients]
VAR momrate = momdiff / [PvM Patients]
VAR _sign = IF(momdiff > 0, "+", "")
VAR _signsymbol = IF(momdiff > 0, "⇧", "⇩")
RETURN IF(ISBLANK([PvM Patients]), "No Previous Value", _signsymbol & " " & _sign & FORMAT(momrate, "#0.0%") & " Vs LM")
```

#### ➤ Conditional Colors
```dax
dynamic_colour = IF(ISBLANK([PvM patients]), "Black", IF([CM patients] > [PvM patients], "Green", "Red"))
```

#### ➤ Repeat Customers
```dax
Repeat customers = COUNT('HDHI Admission data'[Admission No]) - DISTINCTCOUNT('HDHI Admission data'[Admission No])
```

And many more, including waiting time metrics, heart failure rate tracking, weekday analysis, and risk assessment.
