# ICU Liberation Bundle Cards

This repository provides the reproducible workflow for extracting A–F ICU Liberation Bundle elements from MIMIC-IV v3.1, including regular expressions, and a notebook that assembles the bundle cards. The goal is to make the identification logic transparent and portable across environments while adhering to data-use requirements.

---

## 📁 Repository Structure

| File | Description |
|---|---|
| **A-F Liberation Bundle Cards.ipynb** | Jupyter notebook implementing the end-to-end workflow: environment setup, BigQuery access, regex definitions, SQL queries, and card generation. |
| **README.md** | Project overview, setup instructions, and reproducibility notes. |

---

## 📦 Getting Started

To reproduce results, you can either work locally with PhysioNet files or query the hosted copy in Google BigQuery. Both paths are outlined below.

After you complete the credentialed-access steps described under **MIMIC-IV Data Source**, you may download the dataset directly or via the command line. The example below mirrors the dialog shown in the PhysioNet file browser.

```bash
# Replace USERNAME with your PhysioNet username.
wget -r -N -c -np --user USERNAME --ask-password https://physionet.org/files/mimiciv/3.1/
```
---

## 🧬 MIMIC-IV Data Source
The bundle cards were evaluated using data and clinical scenarios derived from the **MIMIC-IV**.  
Access: [https://physionet.org/content/mimiciv/3.1/](https://physionet.org/content/mimiciv/3.1/) (credentialed access required).


## Bundle Cards

[Bundle Card A - Assess, Prevent, and Manage Pain](#bundle-card-a)  
[Bundle Card B - Part I: Spontaneous Awakening Trials (SAT)](#bundle-card-b-part-i)  
[Bundle Card B - Part II: Spontaneous Breathing Trials (SBT)](#bundle-card-b-part-ii)  
[Bundle Card C - Choice of Analgesia and Sedation (RASS)](#bundle-card-c)  
[Bundle Card D - Delirium: Assess, Prevent, and Manage (CAM-ICU)](#bundle-card-d)  
[Bundle Card E - Early Mobility and Exercise](#bundle-card-e)  
[Bundle Card F - Family Engagement and Empowerment](#bundle-card-f)

---

<a id="bundle-card-a"></a>
<details open>
<summary><h2><strong>Bundle Card A - Assess, Prevent, and Manage Pain</strong></h2></summary>

### Card Overview

- **Card Definition:** A - Assessment, Prevention, and Management of Pain.
- **Purpose:** To observe the pain level, assign the pain score, and document a significant pain assessment.

### Assessment Instruments

- **Critical Care Pain Observation Tool (CPOT)** is a four-item scale - vocalization, body movements, facial expression, and muscle tension - each scored from 0 to 2.
- **Alternative pain assessment tools:** Numeric Pain Rating Scale (NRS), and Behavioral Pain Scale (BPS)

### Compliance Checks

- Significant pain level is defined as CPOT > 2, or BPS > 5, or NRS > 3
- Conduct pain assessments at least 6 times within 24 hours to monitor compliance.

### Mapping to Standard Vocabularies: CPOT

| Clinical Concept | Concept Values | Standardized Vocabulary: Code,<br>Concept ID | CDM Table |
|------------------|----------------|-------------------------------------------|-----------|
| CPOT: Muscle Tension Score | 0: Relaxed<br>1: Tense, Rigid<br>2: Very tense or rigid | OMOP Extension: OMOP5214828,<br>ID: 722052 | Measurement |
| CPOT: Vocalization Score | 0: No sound<br>1: Sighing, moaning<br>2: Crying out, groaning | OMOP Extension: OMOP5214835,<br>ID: 722060 | Measurement |
| CPOT: Body Movements Score | 0: No movement<br>1: Protection<br>2: Restlessness | OMOP Extension: OMOP5214824,<br>ID: 722048 | Measurement |
| CPOT: Facial Expression Score | 0: Relaxed, neutral<br>1: Tense<br>2: Grimacing | OMOP Extension: OMOP5214820,<br>ID: 722044 | Measurement |
| CPOT: Total Score | Sum of scores from each of the four indicators | OMOP Extension: OMOP5214819,<br>ID: 722043 | Measurement |

### Alternative Pain Assessment Tools

| Clinical Concept | Concept Values | Standardized Vocabulary: Code, Concept ID | CDM Table |
|------------------|----------------|-------------------------------------------|-----------|
| Numeric Pain Rating Scale (NRS) | Rate pain on a scale of 0 to 10 | SNOMED: 225398001,<br>ID: 3715127 | Measurement |
| Behavioral Pain Scale (BPS) | Facial expression, Upper limb movements, and compliance with mechanical ventilation | SNOMED: 273313008,<br>ID: 3399022 | Measurement |

</details>

---

<a id="bundle-card-b-part-i"></a>
<details>
<summary><h2><strong>Bundle Card B - Part I: Spontaneous Awakening Trials (SAT)</strong></h2></summary>

### Card Overview
- **Card Definition:** B: SAT - Spontaneous Awakening Trials.
- **Purpose:** To assess the ability to breathe without assistance by periodically stopping sedative drugs for mechanically ventilated patients.

### Assessment Instruments
- Assess whether the patient has received sedatives or opioid medications.
- Observe whether the sedative medium is continuous (IV-infusion) or intermittent (Bolus).

### Compliance Checks
- Periodically stop or pause sedatives for mechanically intubated patients to help wean from ventilation.
- Record if patients were successfully weaned from mechanical ventilation during the trial.

### Mapping to Standard Vocabularies
| Clinical Concept | Concept Values | Standardized Vocabulary: Code,<br>Concept ID | CDM Table |
|------------------|----------------|-------------------------------------------|-----------|
| Sedative | Midazolam, Propofol, Lorazepam,<br>Dexmedetomidine, etc. | SNOMED: 372614000 | Drug |
| Sedative Route | • Continuously Infused<br>• Intermittent/Bolus Scheduled<br>• PRN ("as needed") | SNOMED: 72641008 | Procedure |
| SAT Assessment | Sedative Medication Stopped or<br>Paused | SNOMED: 241713008 | Procedure |
| SAT Failure | • Anxiety, agitation, or pain<br>• Respiratory rate > 35/min<br>• Oxygen saturation < 88%<br>• Respiratory distress<br>• Acute cardiac arrhythmia | SNOMED | Observation |

</details>

---

<h2 id="bundle-card-b-part-ii"></h2>
<details>
<summary><h2><strong>Bundle Card B - Part II: Spontaneous Breathing Trials (SBT)</strong></h2></summary>

### Card Overview
- **Card Definition:** B: SBT - Spontaneous Breathing Trials.
- **Purpose:** To assess the ability to breathe without assistance by temporarily reducing or removing mechanical ventilatory support.

### Assessment Instruments
- Assess whether the patients are on mechanical ventilation modes, such as PSV, CPAP, or SPONT ventilation support.
- Start the SBT trial if the patient meets the criteria of Oxygen saturation ≥ 88%, FiO₂ ≤50% and PEEP ≤7.5 cmH₂O. Record the results if started or not.
- Reduce mechanical support, monitor the patient's ability to sustain independent breathing, and adjust ventilator settings such as PEEP and FiO₂ as needed to perform the SBT.

### Compliance Checks
- Record the results if a patient completed the trial or documented the reason for the failure if different.

### Mapping to Standard Vocabularies
| Clinical Concept | Concept Values | Standardized Vocabulary: Code,<br>Concept ID | CDM Table |
|------------------|----------------|-------------------------------------------|-----------|
| Ventilation Modes | • PSV: Pressure Support Ventilation<br>• SPONT: Spontaneous<br>• CPAP: Continuous Positive Airway Pressure<br>• SIMV: Synchronized Intermittent Mandatory Ventilation<br>• APRV: Airway Pressure Release Ventilation | SNOMED: 1258885005,<br>ID: 37158404 | Procedure |
| SBT Started | Yes or No | LOINC: 93203-8,<br>ID: 1001774 | Observation |
| SBT Stopped | • RR > 35 for >5 min<br>• Accessory Muscle Use<br>• SpO₂ <90% for >2 min<br>• BP >180 or <90<br>• HR >140<br>• Arrhythmia | LOINC | Observation |
| SBT Completion | Yes or No | LOINC: 87542-7,<br>ID: 36305814 | Observation |
| SBT Deferred | • Unable to perform RSBI<br>• Chronic vent dependent patient<br>• Pending procedure<br>• RSBI > 105 | LOINC | Observation |
*RSBI: Rapid shallow breathing index*

</details>

---

<a id="bundle-card-c"></a>
<details>
<summary><h2><strong>Bundle Card C - Choice of Analgesia and Sedation (RASS)</strong></h2></summary>

### Card Overview
- **Card Definition:** C - Choice of Analgesia and Sedation levels
- **Purpose:** To select appropriate sedation and analgesia levels

### Assessment Instruments
- Establish individual sedation level targets based on Richmond Agitation-Sedation Scale (RASS), a 10-point scale.

### Compliance Checks
- Perform Arousal Assessments: Conduct arousal assessments at least 6 times within 24 hours to monitor compliance with the sedation targets.
- Continuously evaluate and adjust sedation and analgesia such as Fentanyl and Dexmedetomidine
- Record the results of each assessment, including those outside the sedation target

### Mapping to Standard Vocabularies
| Clinical Concept | Concept Values | Standardized Vocabulary: Code,<br>Concept ID | CDM Table |
|------------------|----------------|-------------------------------------------|-----------|
| RASS Score | Ranges: +4 (Combative) to -5 (Unarousable) | OMOP Extension: OMOP50429(54)-(63),<br>ID: 7050(06)-(15) | Measurement |
| Goal RASS Score | Ranges: 0 (Alert and Calm) to -2 (Light sedation). | OMOP Extension: OMOP50429(58)-(60),<br>ID: 7050(09)-(11) | Observation |
| Sedative Medications | See list below | RxNorm | Drug |
| Analgesic Medications | See list below | RxNorm | Drug |

<div align="center">

| Sedatives | Analgesics |
|-----------|------------|
| • Alprazolam<br>• Amobarbital sodium<br>• Ativan<br>• Clonazepam<br>• Dexmedetomidine<br>• Diazepam<br>• Ketamine<br>• Lorazepam<br>• Meprobamate<br>• Midazolam<br>• Pentobarbital<br>• Phenobarbital<br>• Propofol | • Codeine Phosphate<br>• Codeine sulfate<br>• Fentanyl<br>• Ketamine<br>• Meperidine<br>• Hydromorphone<br>• Methadone<br>• Mod-oxy-codone<br>• Morphine<br>• Oxycodone |

</div>

</details>

---

<a id="bundle-card-d"></a>
<details>
<summary><h2><strong>Bundle Card D - Delirium: Assess, Prevent, and Manage (CAM-ICU)</strong></h2></summary>

### Card Overview
- **Card Definition:** D: Delirium: Assess, Prevent, and Manage
- **Purpose:** To assess delirium status from changes in consciousness, inattention, and mental status

### Assessment Instruments
- Administer the Confusion Assessment Method for the ICU (CAM-ICU)?

### Compliance Checks
- Conduct delirium assessments at least 2 times within 24 hours to monitor compliance.

### Mapping to Standard Vocabularies
| Clinical Concept | Concept Values | Standardized Vocabulary: Code,<br>Concept ID | CDM Table |
|------------------|----------------|-------------------------------------------|-----------|
| CAM-ICU RASS LOC | Yes or No | LOINC: 95857-9,<br>ID: 36031363 | Observation |
| CAM-ICU mental status Change | Yes or No | LOINC: 54632-5,<br>ID: 40757763 | Observation |
| CAM-ICU Inattention | • Yes (3 or more errors)<br>• No (less than 3 errors)<br>• Unable to Assess (UTA)<br>• Language Barrier<br>• Preexisting Advanced Dementia | LOINC: 106402-1,<br>ID: 1092114 | Observation |
| CAM-ICU Altered LOC | Yes or No | LOINC: 95815-7,<br>ID: 36031661 | Observation |
| CAM-ICU Disorganized thinking | Yes or No or UTA | LOINC: 95814-0,<br>ID: 36031539 | Observation |
| Delirium Assessment Status | • Positive<br>• Negative<br>• UTA | SNOMED: 733870009,<br>ID: 37116854 | Procedure |

</details>

---

<a id="bundle-card-e"></a>
<details>
<summary><h2><strong>Bundle Card E - Early Mobility and Exercise</strong></h2></summary>

### Card Overview
- **Card Definition:** E - Early Mobility and Exercise
- **Purpose:** To encourage early physical activity to prevent muscle weakness and fasten the overall recovery.

### Compliance Checks
- Evaluate the patient's mobility capabilities using exercises such as "Sit to Stand Ability" or "Transfer Ability".
- Document the results of each mobility or exercise
- After the initial evaluation, perform assessments for higher levels of exercise to further determine the patient's mobility range.
- Continuously observe and record the highest level of exercise or mobilization the patient achieves, noting any challenges or successes encountered.

### Mapping to Standard Vocabulary
| Clinical Concept | Concept Values | Standardized Vocabulary: Code,<br>Concept ID | CDM Table |
|------------------|----------------|-------------------------------|-----------|
| Range of Motion (ROM) Status | • Passive<br>• Active<br>• min A (Minimal Assistance) | SNOMED: 228528009,<br>ID: 4036092 | Procedure |
| Sit to Stand Ability | • mod A (Moderate Assistance)<br>• max A (Maximum Assistance)<br>• Contact guarding<br>• Independent<br>• Supervision | LOINC: 89393-3,<br>ID: 36306060 | Observation |
| Activity / Mobility | • Lift to chair/bed<br>• Turning in bed<br>• Chair: Transfer to chair/bed<br>• Stand: >/= One minute<br>• Walk: 10+/25+/250+ Feet<br>• Stand step<br>• Stand pivot | SNOMED: 363803005,<br>ID: 4178501 | Observation |
| Transfer Ability | • Contact guarding<br>• Supervision<br>• Squat pivot<br>• Slide board<br>• Independent | LOINC: 83185-9,<br>ID: 42529379 | Observation |

</details>

---

<a id="bundle-card-f"></a>
<details>
<summary><h2><strong>Bundle Card F - Family Engagement and Empowerment</strong></h2></summary>

### Card Overview
- **Card Definition:** F - Family Engagement and Empowerment
- **Purpose:** To involve and engage families in the care process

### Compliance Checks
- Record each instance of family visit, call, or communications with healthcare providers, including nurses and doctors.
- Hold family meetings to discuss the patient's care plan, ensuring consistent family involvement.
- Ensure that family decision-makers are informed about the ICU Liberation Bundle and the patient's care status.
- Allow and track family or friends participation in care rounds, noting their contribution to decision-making processes.

### Mapping to Standard Vocabularies
| Clinical Concept | Concept Values | Standardized Vocabulary: Code,<br>Concept ID | CDM Table |
|------------------|----------------|-------------------------------|-----------|
| Family Communication | • Family Visited<br>• Family Talked to Registered Nurse<br>• Family Called<br>• Family Talked to Doctor<br>• Family Conference | SNOMED: 14461000202103,<br>ID: 1450126 | Observation |
| Family Meeting | Yes or NO | SNOMED: 711070007,<br>ID: 46272666 | Observation |
| Family Informed | • Yes<br>• No<br>• Not Applicable | SNOMED: 406141002,<br>ID: 4247377 | Observation |

</details>

---

## 📬 Contact
For questions or collaboration inquiries, please contact:

**Md Fantacher Islam**  
PhD Student, Systems and Industrial Engineering  
University of Arizona  
📧 fantacher@arizona.edu
