# Food Delivery Operations & Data Analysis: BATR Performance and Root Cause Analysis

An end-to-end **data analysis and operations analytics** project focused on evaluating and improving **Biker Arrival at Restaurant (BATR)** performance in a food-delivery platform.

The project combines data preprocessing, exploratory data analysis, KPI design, statistical analysis, process decomposition, root cause analysis, geographic and temporal segmentation, and operational monitoring to identify where performance deteriorates and what factors are associated with that deterioration.

## Project Overview

In food-delivery operations, timely biker arrival at restaurants is an important factor in maintaining reliable delivery performance.

This project analyzes **Biker Arrival at Restaurant Time (BATR)**, defined as the elapsed time between the beginning of an order's operational time slot and the biker's arrival at the restaurant.

Rather than analyzing BATR only as a single aggregated metric, the project decomposes it into two operational stages:

$$
BATR = Acceptance\ Time + Post\text{-}Acceptance\ Arrival\ Time
$$

where:

* **Acceptance Time** measures how long an order waits before being accepted by a biker.
* **Post-Acceptance Arrival Time** measures how long the biker takes to reach the restaurant after accepting the order.

This decomposition allows the analysis to determine whether poor BATR performance originates mainly from the acceptance process, the post-acceptance arrival process, or a combination of both.

## Project Objectives

The project is designed to answer several key analytical and operational questions:

1. Where is poor BATR performance concentrated?
2. When does operational performance deteriorate?
3. Which cities, districts, and time periods contribute most to poor performance?
4. Is deterioration mainly caused by Acceptance Time or Post-Acceptance Arrival Time?
5. Which order and operational characteristics are associated with poor BATR?
6. Which findings are directly supported by the data?
7. Which potential root causes require additional operational data?
8. Which KPIs can be used for continuous monitoring and early detection of deterioration?

## Data Analysis Workflow

The project analyzes more than **150,000 anonymized food-delivery orders** by combining order-level information with operational event timestamps.

The analysis pipeline includes:

* Data loading and integration
* Data quality assessment
* Timestamp validation
* Missing and inconsistent value handling
* Feature engineering
* Exploratory Data Analysis (EDA)
* KPI calculation
* Distribution analysis
* Geographic segmentation
* Temporal analysis
* Process-stage decomposition
* Statistical analysis
* Root Cause Analysis (RCA)
* Threshold calibration
* Early-warning analysis
* Operational recommendations

The goal is to move from raw operational data toward interpretable and actionable insights.

## Exploratory Data Analysis

Exploratory analysis is used to understand the overall structure of BATR performance and identify important patterns in the dataset.

The analysis investigates:

* Overall BATR distribution
* BATR15 performance
* Acceptance Time distribution
* Arrival Time distribution
* Mean, median, P75, P90, and P95 behavior
* Delayed-order severity
* Performance across cities and districts
* Hourly patterns
* Day-of-week patterns
* Monthly trends
* Peak-hour deterioration
* Order-volume effects
* Distance-related behavior
* Offer-status differences
* Vendor-level variation

The analysis also focuses on tail behavior because operational problems are often concentrated among the slowest orders rather than around average performance.

## Core KPIs

Five main KPIs are used to monitor both overall operational performance and its underlying process stages.

### 1. BATR15

**BATR15** represents the percentage of orders where the biker reaches the restaurant within 15 minutes.

It is the main outcome KPI for measuring overall arrival performance.

$$
BATR15 = P(BATR \leq 15)
$$

A reduction in BATR15 indicates deterioration in overall operational performance.

### 2. Average Late Minutes after 15 — ALM15

BATR15 only determines whether an order passed or failed the 15-minute threshold.

It does not measure how severe the delay was.

**ALM15** therefore measures the average number of minutes by which delayed orders exceed the 15-minute target.

This allows the analysis to distinguish between slightly late orders and severe operational delays.

### 3. Acceptance Time P90

**Acceptance Time P90** measures tail performance during the order acceptance stage.

It represents the time within which 90% of orders are accepted.

Using P90 instead of only the mean or median makes the KPI more sensitive to deterioration among slower orders.

### 4. Delayed Offer Rate

**Delayed Offer Rate** measures the proportion of orders entering a delayed state during the offer and acceptance process.

This metric can act as an early-warning signal for potential deterioration in the acceptance stage.

### 5. Post-Acceptance Arrival Time P90

**Arrival Time P90** measures the time within which 90% of bikers reach the restaurant after accepting an order.

This KPI helps identify deterioration occurring after acceptance and separates it from issues originating earlier in the process.

## Process Decomposition

One of the central analytical components of the project is decomposing overall BATR into its underlying process stages.

For sequentially consistent orders:

$$
BATR = AcceptanceTime + ArrivalTime
$$

This makes it possible to identify whether a performance issue is mainly driven by:

* Slow order acceptance
* Slow biker arrival after acceptance
* Both stages simultaneously

This process-level analysis provides more useful information than observing BATR15 alone.

For example, two areas may have similar BATR15 values while having completely different underlying operational problems.

One may suffer primarily from delayed acceptance, while another may experience longer post-acceptance travel times.

## Geographic Analysis

Operational performance is analyzed across multiple geographic levels.

The analysis includes:

* City-level comparison
* District-level comparison
* Vendor-level analysis
* District versus city baseline comparison
* District versus city-hour baseline comparison

This hierarchical approach helps distinguish system-wide deterioration from localized operational problems.

Rather than ranking areas only based on raw BATR values, the analysis also considers how each area performs relative to its surrounding operational environment.

## Temporal Analysis

BATR performance is also analyzed across time.

The project investigates:

* Hour-of-day patterns
* Day-of-week differences
* Monthly changes
* Lunch peak periods
* Dinner peak periods
* City-hour combinations
* Changes in Acceptance and Arrival performance over time

Temporal segmentation helps detect operational issues that may disappear when performance is viewed only at an aggregate level.

## Root Cause Analysis

A major component of the project is a structured **Root Cause Analysis framework**.

The analysis first identifies where the deterioration occurs within the process.

For example:

> Acceptance Time is unusually high in a specific district during a specific period.

This is an evidence-supported observation because it can be directly measured from the available data.

However, deeper operational explanations such as:

* insufficient biker supply,
* inefficient assignment,
* biker positioning,
* traffic conditions,
* rejection behavior,
* biker-to-restaurant distance,

may require additional data.

The RCA therefore separates two categories.

### Evidence-Supported Process Issues

Problems directly observable from the available data.

Examples include:

* unusually high Acceptance Time,
* unusually high Arrival Time,
* high Delayed Offer Rate,
* concentration of poor BATR in specific districts or hours.

### Operational Root-Cause Hypotheses

Potential explanations that are operationally plausible but cannot be fully confirmed using the available dataset.

For each hypothesis, the project identifies additional data that would be required for stronger causal validation.

This approach avoids interpreting correlation as causation.

## Statistical Analysis

The project uses statistical analysis to support exploratory findings and evaluate relationships between operational variables.

The analysis includes:

* Distribution statistics
* Percentile-based metrics
* Correlation analysis
* Healthy-period benchmarking
* Comparative segmentation
* Late-versus-successful order comparison
* Period-over-period decomposition

Both Pearson and Spearman relationships are considered where appropriate.

The statistical layer is used to strengthen operational interpretations rather than relying only on visual inspection.

## KPI Threshold Calibration

Supporting KPI thresholds are not selected arbitrarily.

Instead, thresholds are derived from periods where overall BATR performance is considered healthy.

The general process is:

1. Identify healthy operating periods.
2. Calculate the historical distribution of each supporting KPI.
3. Examine median and upper percentiles.
4. Define operational warning thresholds.
5. Compare performance above and below those thresholds.

This creates a data-driven monitoring framework based on historical system behavior.

## Early-Warning Framework

Monitoring only BATR15 can reveal that performance has already deteriorated, but it may not provide enough information about why.

For this reason, the project combines BATR15 with supporting metrics such as:

* Acceptance P90
* Arrival P90
* Delayed Offer Rate
* ALM15

These indicators help detect deterioration earlier and identify which part of the process requires attention.

The monitoring system is therefore designed to answer two questions:

**Is operational performance deteriorating?**

and

**Which part of the operational process is driving that deterioration?**

## Operations Analytics Framework

The overall project follows the following analytical workflow:

**Operational Data**

↓

**Data Cleaning & Feature Engineering**

↓

**Exploratory Data Analysis**

↓

**KPI Monitoring**

↓

**Geographic & Temporal Segmentation**

↓

**Process Decomposition**

↓

**Root Cause Analysis**

↓

**Early Warning**

↓

**Operational Action**

This structure connects data analysis directly to operational decision-making.

## Operational Decision Support

The purpose of the analysis is not limited to generating charts or descriptive statistics.

Analytical findings are translated into operational insights that can support decisions such as:

* identifying priority regions,
* detecting problematic time windows,
* distinguishing acceptance-related and arrival-related issues,
* prioritizing investigations,
* defining monitoring thresholds,
* detecting emerging deterioration,
* determining which additional operational data should be collected.

The project therefore combines **descriptive analytics, diagnostic analytics, and operational decision support**.

## Technologies

The project uses:

* **Python**
* **Pandas**
* **NumPy**
* **Matplotlib**
* **SciPy**
* **Jupyter Notebook**
* **Power BI**
* **DAX**

Python is used for data preprocessing, exploratory data analysis, statistical analysis, KPI evaluation, RCA, and early-warning experiments.

Power BI provides an interactive monitoring layer for exploring operational KPIs across time and geographic dimensions.

## Repository Structure

The repository contains analytical notebooks covering different stages of the project, including:

* Data preparation and data quality analysis
* BATR and BATR15 calculation
* Geographic performance analysis
* Temporal trend analysis
* Acceptance and Arrival decomposition
* Root Cause Analysis
* Order-characteristics analysis
* Statistical analysis
* KPI monitoring
* Early-warning analysis
* Operational recommendations

Dashboard-related files provide an additional interactive layer for monitoring operational performance.

## Key Takeaway

Operational problems cannot always be understood through a single aggregate KPI.

A drop in BATR15 indicates that performance is deteriorating, but does not explain where the problem originates.

By combining:

* data analysis,
* process decomposition,
* geographic segmentation,
* temporal analysis,
* statistical analysis,
* KPI monitoring,
* and root cause analysis,

this project provides a structured way to move from:

**"Performance is getting worse."**

to:

**"Where is performance deteriorating, when is it happening, which process stage is responsible, what evidence supports that conclusion, and what should be investigated next?"**

