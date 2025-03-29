Overview

This repository contains code and data-processing scripts for an incremental neural topic modeling project. It builds on a BERTopic pipeline and introduces three new popularity metrics—Relevance, Novelty, and Velocity—to help detect short-lived or semantically important topics that might be missed by simpler frequency-based approaches.
Key Steps in the Pipeline

    Data Filtering and Monthly Slicing

        Load and filter the original dataset, ensuring only sufficiently long and relevant articles are kept (e.g., Business-desk articles).

        Split into train (e.g., June 2021–March 2022) and evaluation (April–June 2022) sets.

        Slice the data by month, cleaning each slice’s text for later processing.

    Running BERTopic per Month

        For each month, a new BERTopic model is trained on that slice’s documents (using UMAP + HDBSCAN for dimensionality reduction and clustering).

        Topic information is collected, including embeddings and assigned documents.

    Incremental Topic Merging

        Consecutive months’ topics are merged if their embedding similarity passes a user-defined threshold (e.g., 0.7).

        Merged topics are aggregated into global topics, each containing the union of documents from all relevant slices.

    Popularity Metric Computation

        Base (Frequency + Decay): Tracks a topic’s mention count over time but decays in months with no new documents.

        Relevance: Measures how semantically central a topic is within its monthly corpus (computed via mean similarity to all documents).

        Novelty: Uses KL divergence between consecutive months’ distributions to quantify how “new” or changed a topic’s textual content is.

        Velocity: Derives from monthly Relevance differences to show how quickly a topic’s semantic importance is rising or declining.

    Evaluation

        Both quantitative (predictive correlation, precision@k, rank correlation, trend consistency) and qualitative (human-refined “ground truth” topics) methods are used to assess the metrics’ effectiveness at detecting significant trends.

Repository Layout

    data/
    Contains the CSV or text files of the raw dataset or pre-filtered slices.

    scripts/
    Holds Python scripts for each pipeline stage: data filtering, monthly slicing, BERTopic training, merging topics, and computing metrics.

    notebooks/
    (Optional) Jupyter notebooks demonstrating usage examples or additional analysis.

    results/
    Stores intermediate outputs (e.g., doc info files, merged topic outputs, metrics CSVs).

    figures/
    Contains all the final graphs and visualizations for the four popularity metrics.

    main.py or main_pipeline.py
    Central entry point to run the entire pipeline from start to finish (data loading → monthly topic modeling → merges → metrics → evaluation).

    requirements.txt
    Lists Python dependencies (e.g., BERTopic, scikit-learn, sentence-transformers, UMAP, HDBSCAN, etc.).

Dependencies

    Python 3.8+

    BERTopic and its dependencies

    UMAP-learn

    HDBSCAN

    sentence-transformers (for Relevance metric)

    pandas, numpy, scipy, scikit-learn

    matplotlib or plotly for visualization
