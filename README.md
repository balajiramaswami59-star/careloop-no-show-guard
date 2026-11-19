# CareLoop Clinics — No-Show Guard

**Repository link:** Replace this line after creating the public GitHub repository.

## Business question
Which appointments are most likely to be missed, why do patients miss them, and what should CareLoop do first?

## Four-line summary
1. **Part 1:** 1,020 raw bookings were cleaned to **990 appointments**, with **218 no-shows (22.0%)**.
2. **Part 2:** No-show risk rises with lead time: **16.2%** for 1–7 days versus **60.5%** for 31+ days.
3. **Part 3:** **Logistic Regression** was selected using training-only 5-fold CV ROC-AUC (**0.774**). The chosen threshold was **0.35**; final test ROC-AUC was **0.789** with recall **0.571**.
4. **Part 4:** The first 10 notes are processed through Gemini into structured causes. Run `04_genai.ipynb` with your own key and replace the pending `notes_analysed.csv` before submission.

## Recommendation
CareLoop should produce an evening-before risk list and call patients at or above the selected **0.35 threshold**, starting with long-lead appointments and patients with prior no-shows. The cleaned data contains **218 no-shows from 990 bookings (22.0%)**. Bookings made 31+ days ahead show a **60.5%** no-show rate, compared with **16.2%** for 1–7 days. The selected model achieved **0.774 CV ROC-AUC** and **0.789 test ROC-AUC**. CareLoop should also improve reminder reliability and use the validated Gemini categories to distinguish forgetting, transport, cost, emergency and scheduling causes. Update this paragraph with the actual note counts after running Part 4.

## Model and threshold
The final model was **Logistic Regression**, selected solely from training-set cross-validation. At threshold 0.35, the test set precision was **0.571**, recall **0.571**, and **49 of 198** test appointments were flagged. Recall matters because a missed warning can leave a doctor slot empty, while an extra call has a much smaller operational cost.

## Key SQL findings
- Overall: **990 appointments**, **218 no-shows**, **22.02%**.
- Highest unfiltered clinic rate: **Gomti Nagar Care Point**, **33.3%** across **90 appointments**.
- Lead bands: 1–7 days **16.2%**; 8–14 **17.4%**; 15–30 **30.0%**; 31+ **60.5%**.

## Limitations
1. Ten notes are illustrative and cannot establish clinic-wide reasons.
2. Cleaning medians and outlier fences were calculated on all rows before the split, allowing small indirect test influence.
3. A random appointment-level split can place the same patient in train and test and does not reproduce a past-to-future deployment.

## Repository contents
- `01_ingest_clean.ipynb`
- `rejected_rows.json`
- `appointments_clean.csv`
- `clinics_enriched.csv`
- `load_data.sql`
- `02_queries.sql`
- `03_model.ipynb`
- `test_risk_scores.csv`
- `04_genai.ipynb`
- `notes_analysed.csv` (replace after running Gemini)
- `data/appointments.csv`, `data/clinics.csv`, `data/followup_notes.csv`

## Reproduction order
1. Run `01_ingest_clean.ipynb`.
2. Create/load MySQL using `02_queries.sql` and `load_data.sql`.
3. Run `03_model.ipynb`.
4. Run `04_genai.ipynb` in Colab with a runtime-only Gemini key.
5. Confirm the API key is absent, update Part 4 counts in this README, and upload all files to a public GitHub repository.
