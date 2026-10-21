# Benchmarking Large Language Models for Bangladeshi Statutory Law: A Study on Accuracy and Hallucination in Bangla

---

## 📖 About the Project

Language models are now a common first stop for legal questions, but when the question is in Bangla and the law is a local statute, the answers are often unreliable. Fabricated penalties and wrong section numbers are easy to produce and hard for an ordinary reader to spot.

This repository contains a **native Bangla multiple-choice benchmark** created from the official text of the **Bangladesh Road Transport Act, 2018 (সড়ক পরিবহণ আইন, ২০১৮)**. It is meant to give researchers a fixed, answer-keyed way to test how well a model knows this Act.



### ✨ Highlights
* **Built from a real statute:** Questions come straight from an official government PDF, not from web articles or summaries.
* **Hand-checked:** **296 MCQs**, each written by a team member and verified by a second person against the original text.
* **Two kinds of reasoning:** Questions range from direct recall of a clause (definitions, section numbers, penalties) to scenario-style and compare-and-contrast questions.
* **Ready for evaluation:** Comes with a simple three-part scoring idea (accuracy, factual error, hallucination) that works for any model.

---

## 🛠️ Methodology Pipeline
The dataset was built in six stages, from law selection to the final MCQ set, with manual checking at almost every stage:



<img width="1693" height="929" alt="methodology" src="https://github.com/user-attachments/assets/149a149f-df2e-4857-ac23-e9cceee75f55" />




1. **Law selection:** The Bangladesh Road Transport Act, 2018 was chosen, using the official 38-page government PDF as the source.
2. **Text extraction:** Text was read with PyMuPDF. Pages that came out garbled were re-read using Tesseract OCR (Bangla).
3. **Raw text dataset:** Each page was stored as one record with its page number, word count and page content.
4. **Manual noise removal:** Headers, page numbers, broken hyphenation, OCR slips and stray symbols were removed by hand while comparing with the original PDF.
5. **MCQ generation:** Four-option questions were written from the cleaned text. Wrong options were designed to look believable (close numbers, right idea from the wrong section, opposite meaning).
6. **Final validation:** A second member re-checked wording, numbers and section references for every question, giving the final 296-question set.

---

## 📊 What's in the Data

The repository holds the final benchmark of **299 questions**.

### Columns
* **id:** Running number from 1 to 299.
* **Question:** The question in Bangla, followed by options `A)` to `D)` on separate lines.
* **Answer:** The correct option letter.

### Sample Preview
| Question | Options | Answer |
| :--- | :--- | :---: |
| সড়ক পরিবহণ আইন, ২০১৮ কত তারিখে কার্যকর হয়? | **A)** ১ নভেম্বর, ২০১৯ <br>**B)** ৮ অক্টোবর, ২০১৮ <br>**C)** ১ নভেম্বর, ২০১৮ <br>**D)** ২২ অক্টোবর, ২০১৯ | **A** |
| পেশাদার ড্রাইভিং লাইসেন্স প্রাপ্তির জন্য ন্যূনতম বয়স কত? | **A)** ১৮ বৎসর <br>**B)** ২০ বৎসর <br>**C)** ২১ (একুশ) বৎসর <br>**D)** ২২ বৎসর | **C** |

### Answer Balance
| Option | Count | Share |
| :---: | :---: | :---: |
| A | 22 | 7.4% |
| B | 152 | 51.4% |
| C | 95 | 32.1% |
| D | 27 | 9.1% |

About half the answers are `B`, because the dataset follows the Act rather than being re-balanced. When testing a model, compare its score with the "always answer B" baseline.

### Quick Load
```python
import pandas as pd

df = pd.read_csv("dataset_final.csv", encoding="utf-8-sig")
print(df.shape)   # (296, 3)
```

---

## 📏 Suggested Evaluation
* **Accuracy:** the chosen option matches the key.
* **Factual error:** wrong option because a number, section or condition was recalled incorrectly.
* **Hallucination:** the model cites a section, penalty or procedure that does not exist in the Act.

---

## ⚠️ Limitations
* Covers a single Act, with no amendments, rules or case law.
* 296 questions is small, so tiny score differences between models are not meaningful.
* Model results are not included yet; this repo provides the benchmark only.

---

## 🚀 Next Steps
* Evaluate open-source and proprietary LLMs (zero-shot, few-shot, reasoning prompts).
* Extend to other Bangladeshi laws such as labour and penal codes.
* Test light fine-tuning of small open models.

---

## 📁 Repository Structure
```
├── dataset_final.csv      # 296 MCQs (UTF-8)
├── dataset_final.xlsx     # Same data as a spreadsheet
├── methodology.png        # Pipeline diagram
└── README.md              # Project documentation
```

---

*For research use only. This is not legal advice; please consult the official text of the Act.*
