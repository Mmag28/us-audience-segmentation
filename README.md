# U.S. Audience Segmentation Pipeline

## Overview

This project builds a **data-driven baseline segmentation of the U.S. population** by integrating demographic structure, psychological traits, and media consumption behavior.

The objective is to generate **interpretable audience segments** that combine structural characteristics (demographics and socioeconomic variables), inferred psychological signals, and media behavior patterns.

The resulting segments provide a **baseline audience intelligence layer** that can later support downstream applications such as:

* audience analysis
* communication strategy development
* message targeting
* behavioral segmentation
* AI-assisted audience retrieval and comparison

The pipeline integrates multiple nationally representative datasets and produces **structured audience segment cards** suitable for both human interpretation and machine retrieval.

---

## Example Audience Segments

### Early-Career Homeowners (~11% of U.S. population)

**Structural profile**  
Predominantly White individuals aged **18–24**, typically **employed**, **never married**, and with **high school education or less**. Despite their young age, many appear in **homeowner households with incomes between \$100K–\$199K**, which may reflect multigenerational households, shared-income living arrangements, or early entry into skilled trades.

**Psychological signals**  
Members of this segment tend to report **moderate life satisfaction**, clustering around a “pretty happy” disposition rather than extremes of wellbeing. Media engagement signals are **mixed**, suggesting that some individuals are highly engaged with media while others interact with information sources more sporadically.

**Media environment**  
This segment shows strong signals of **mobile-first digital media use**, particularly on **TikTok and Snapchat**, combined with high overall internet frequency. Their media environment appears oriented around **short-form, algorithmically curated content** and social platforms optimized for rapid consumption and sharing.

---

### Retired Renters (~13% of U.S. population)

**Structural profile**  
Predominantly **White adults aged 65 and older**, typically **retired**, **married**, and with **high school education or less**. Household incomes generally fall between **\$50K–\$99K**, and most individuals in this segment live in **renter households**, suggesting modest but relatively stable financial circumstances in later life.

**Psychological signals**  
This segment shows a tendency toward **conservative ideological alignment**, suggesting traditional or right-leaning values. Media engagement indicators appear **mixed**, implying that consumption habits vary widely within the group.

**Media environment**  
Despite belonging to an older demographic, this segment displays signs of **regular internet use**, with platform signals including **Instagram and TikTok**. This may indicate growing adoption of visually driven social media platforms among older populations, although engagement may vary between habitual and occasional use.

---

## Data Sources

This project integrates multiple nationally representative U.S. datasets.

### American Community Survey (ACS)

Source: U.S. Census Bureau

ACS Public Use Microdata Sample (PUMS) provides the demographic and socioeconomic structure of the U.S. population.

Download:
https://www.census.gov/programs-surveys/acs/microdata.html

---

### General Social Survey (GSS)

Source: NORC at the University of Chicago

The GSS provides nationally representative data on social attitudes, political beliefs, and behavioral indicators.

Download:
https://gss.norc.org/Get-The-Data

Dataset used:
GSS 1972–2024 cumulative dataset (`gss7224_r2.sav`) filtered for the relevant wave.

---

### Pew Research Center (NPORS)

Source: Pew Research Center

The National Public Opinion Reference Survey (NPORS) provides nationally representative measures of media consumption and digital platform usage.

Download:
https://www.pewresearch.org/methods/fact-sheet/national-public-opinion-reference-survey-npors/

Dataset used:
NPORS public release dataset.

### American Community Survey (ACS)

Source: U.S. Census Bureau

Provides the **structural population backbone**, including demographic and socioeconomic variables such as:

* age
* sex
* race and ethnicity
* education
* employment status
* household income
* marital status
* housing tenure
* household composition

These features are used to construct a **synthetic structural population** representing U.S. society.

---

### General Social Survey (GSS)

Provides nationally representative **attitudinal and psychological indicators**, including:

* political ideology
* party alignment
* religiosity
* life satisfaction
* media engagement

These signals are statistically projected onto the ACS structural population to create a **psychological inference layer**.

---

### Pew Research Center (NPORS / ATP)

Provides **media consumption behavior**, including:

* internet access and usage
* social media platform adoption
* radio and digital media usage
* online engagement patterns

These behaviors are projected onto the synthetic population to create a **media environment layer**.

---

# Pipeline Architecture

The project follows an end-to-end analytical pipeline implemented in Jupyter notebooks.

ACS Demographics -> Structural Population Model ->Clustering -> Psychological Inference (GSS)->Media Behavior Projection (Pew)->Audience Segment Cards

### 01 — ACS Ingestion

Load and prepare ACS PUMS microdata.

### 02 — Structural Feature Engineering

Create demographic and socioeconomic features used for segmentation.

### 03 — Modeling Sample Creation

Construct a representative modeling dataset using weighted sampling.

### 04 — Household Feature Integration

Merge household-level variables into the individual population dataset.

### 05 — Population Clustering

Run clustering experiments using:

* K-Modes
* K-Prototypes
* MCA + K-Means

### 06 — Cluster Evaluation

Evaluate clustering solutions using:

* cluster balance
* interpretability
* socioeconomic differentiation

Select the final structural segmentation model.

### 07 — Psychological Inference

Use GSS data to estimate probabilistic psychological traits for the structural population.

### 08 — Cluster Psychological Profiles

Aggregate psychological probabilities to the cluster level to identify behavioral signatures.

### 09 — Media Behavior Projection

Project Pew media usage patterns onto the synthetic population.

### 10 — Cluster Media Interpretation

Identify media behaviors that distinguish each cluster relative to the national baseline.

### 11 — Audience Segment Cards

Convert clusters into structured **audience segment cards** including:

* structural profile
* psychological signals
* media signals
* narrative description
* machine-readable tags
* retrieval-ready segment text


---

# Final Outputs

The pipeline produces a set of reusable audience intelligence artifacts.

### Audience Segment Table

```
data/processed/11_us_audience_segments_v1.parquet
```

Contains one row per segment with:

* structural profile
* psychological signals
* media signals
* narrative summary
* machine-readable tags

---

### RAG Retrieval Corpus

```
data/processed/11_us_audience_segments_v1.jsonl
```

Contains a retrieval-ready text representation of each segment designed for:

* vector search
* LLM retrieval
* AI-assisted audience analysis

---

# Repository Structure

```
us-audience-segmentation
│
├── data
│   ├── raw
│   ├── interim
│   └── processed
|   └── versions
│
├── notebooks
│   ├── 01_acs_pums_ingest.ipynb
│   ├── 02_acs_structural_features.ipynb
│   ├── 03_acs_modeling_sample.ipynb
│   ├── 04_household_features_and_population_merge.ipynb
│   ├── 05_population_clustering.ipynb
│   ├── 06_cluster_evaluation_and_selection.ipynb
│   ├── 07_psychological_inference_gss.ipynb
│   ├── 08_cluster_psychological_profiles.ipynb
│   ├── 09_media_behavior_projection_pew.ipynb
│   ├── 10_segment_interpretation_and_archetypes.ipynb
│   └── 11_segment_cards.ipynb
│
├── outputs
|   ├── figures
│   └── tables
│
├── models
|   └── clustering
│
├── docs
│
├── src
├── LICENSE
├── README.md
└── requirements.txt
```

---

# Reproducibility

## Reproducing the Pipeline

1. Download the required datasets from the links above.

2. Place them in the following directory structure:
```
data/
└── raw/
    ├── acs/
    ├── gss/
    └── pew/
```

3. Install project dependencies:

pip install -r requirements.txt

4. Run the notebooks sequentially:

01_acs_pums_ingest.ipynb  
02_acs_structural_features.ipynb  
03_acs_modeling_sample.ipynb  
04_household_features_and_population_merge.ipynb  
05_population_clustering.ipynb  
06_cluster_evaluation_and_selection.ipynb  
07_psychological_inference_gss.ipynb  
08_cluster_psychological_profiles.ipynb  
09_media_behavior_projection_pew.ipynb  
10_segment_interpretation_and_archetypes.ipynb  
11_segment_cards.ipynb

# Applications

This baseline segmentation can support a variety of analytical tasks, including:

* audience research
* communication strategy design
* behavioral segmentation
* marketing analytics
* policy communication analysis
* AI-assisted audience retrieval systems

---

# License

This project is released under the MIT License.

---

## Data Notice

This repository does not distribute raw survey datasets.

Users must obtain the ACS, GSS, and Pew datasets from their original sources and comply with the licensing terms of those datasets.
