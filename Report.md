## Report: Predicting 30-Day Readmission and Extracting Clinical Entities from Discharge Notes

### Overview and Approach

This project addressed two related tasks using a shared clinical dataset:  
(1) predicting whether a patient would be readmitted within 30 days of discharge using structured clinical data, and  
(2) extracting clinically relevant information from free-text discharge notes.

For the predictive modeling task, I began with basic data cleaning and exploratory analysis to confirm data quality, understand feature distributions, and assess class balance. The dataset contained no missing values or duplicate patient records, and the readmission outcome showed a moderate class imbalance. I then applied light feature engineering to improve signal without overcomplicating the model, including age group binning, a binary indicator for any prior admissions, and a flag for longer length of stay. A logistic regression model was chosen as a transparent and clinically interpretable baseline, with class weighting applied to account for imbalance.

For the clinical text extraction task, I initially tested a general-purpose spaCy NER model, which produced sparse and clinically uninformative results. Given this limitation, I pivoted to a domain-specific clinical NLP approach using a Stanza i2b2-style NER model. This model is trained on clinical text and designed to recognize healthcare-specific concepts, making it better suited for short, templated discharge notes. The model was used to extract and label entities such as diagnoses/problems, treatments or medications, procedures/tests, and follow-up actions.

---

### Key Results

The readmission prediction model achieved modest performance, with a ROC AUC of approximately 0.42 and an F1-score around 0.40. While this performance is limited, it is consistent with expectations for a small dataset containing primarily high-level structured variables. Feature analysis showed that older age (particularly 65+), certain diagnosis categories, medication type, and longer length of stay were associated with higher readmission risk, while younger age groups and lack of prior admissions were associated with lower risk. These findings align with clinically intuitive risk factors and provide a reasonable baseline understanding of readmission drivers.

The clinical NLP task yielded substantially more meaningful results using the Stanza i2b2-style model. The model successfully extracted clinically relevant entities from the discharge notes, including conditions such as pneumonia and infection, treatments such as surgery and antibiotics, diagnostic tests, and follow-up instructions. Compared to general-purpose NER, the clinical model demonstrated clearer alignment with medical concepts and produced structured outputs that are more suitable for downstream clinical analysis.

---

### Practical Implications

From a practical perspective, the readmission model demonstrates how even simple, interpretable approaches can surface clinically meaningful patterns, while also highlighting the limitations of relying solely on structured administrative data. The NLP component shows clear value in converting free-text discharge notes into structured clinical information, which could support care coordination, quality monitoring, or integration into downstream predictive models.

Importantly, the project illustrates the need to match modeling approaches to the domain: general-purpose NLP tools may perform poorly on clinical text, whereas domain-specific models can substantially improve relevance and reliability.

---

### Future Work and Extensions

With more time and additional data, several things could be improved. For the predictive task, incorporating larger sample sizes, richer clinical history, and social determinants of health would likely improve performance. Temporal features and post-discharge follow-up data could also add predictive value. For the NLP task, further normalization of extracted entities and integration with medical ontologies would enhance consistency and usability. Finally, combining structured features with NLP-derived variables in a single predictive model could provide a more comprehensive assessment of readmission risk.
