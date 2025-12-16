# Graph-Knowledge-Pipeline
Graph Knowledge pipeline project. Including : Web Scrapping, Text Preprocessing, Name Entity Recognition (NER), Relation Extraction, RDF Construction, Data Augmentation, Evaluation, Vizualisation

\documentclass[11pt]{article}
\usepackage[utf8]{inputenc}
\usepackage{geometry}
\usepackage{graphicx}
\usepackage{booktabs}
\usepackage{hyperref}
\usepackage{amsmath}
\usepackage{float}
\usepackage{titlesec}
\usepackage{enumitem}
\geometry{margin=1in}

\titleformat{\section}{\large\bfseries}{\thesection}{1em}{}
\titleformat{\subsection}{\normalsize\bfseries}{\thesubsection}{1em}{}

\title{\textbf{Web Data Mining \& Semantics}\\Final Project Report}
\author{Jérémie Formey de Saint Louvent, Alexis Ducroux\\\small Class: DIA 3}
\date{}

\begin{document}

\maketitle

\section{Project Overview and Methodology}

This final project for the course \textit{Web Data Mining \& Semantics} was divided into two integrated parts:

\begin{itemize}
    \item \textbf{Part 1: Web Scraping and Knowledge Base Construction}
    \item \textbf{Part 2: Knowledge Graph Embedding}
\end{itemize}

The goal was to extract structured knowledge from web articles and encode it into a knowledge graph (KG). The graph was then analyzed through embedding models to reveal semantic structure and support reasoning tasks like similarity and link prediction.

\section{Methodology}

\subsection*{Part 1: Web Scraping and Knowledge Base Construction}
\begin{itemize}
    \item \textbf{Web Scraping:} Articles were collected from CNN’s technology section using Selenium with headless Chrome.
    \item \textbf{Text Preprocessing:} Lowercasing, punctuation and stopword removal, lemmatization, tokenization.
    \item \textbf{Named Entity Recognition (NER):} Compared a CRF model (trained on CoNLL-2003) and SpaCy's pre-trained \texttt{en\_ner\_conll03}.
    \item \textbf{Relation Extraction:} Used SpaCy \texttt{en\_core\_web\_sm} with 3+ rule-based syntactic patterns.
    \item \textbf{RDF Construction:} Converted triples to RDF using RDFLib.
\end{itemize}

\subsection*{Part 2: Knowledge Graph Embedding}
\begin{itemize}
    \item \textbf{Data Augmentation:} Retrieved additional facts from DBpedia via SPARQL.
    \item \textbf{Model Training:} Trained TransE and DistMult using PyKEEN on both the original and augmented graphs.
    \item \textbf{Evaluation:} Assessed Mean Rank, MRR, Hits@1/3/10.
    \item \textbf{Similarity and Prediction:} Used cosine similarity and link prediction routines.
    \item \textbf{Visualization:} Applied t-SNE to entity embeddings to evaluate clustering.
\end{itemize}

\section{Quantitative Results}

\subsection*{Named Entity Recognition}

\begin{table}[H]
\centering
\begin{tabular}{lcc}
\toprule
\textbf{Metric} & \textbf{CRF Model} & \textbf{SpaCy Pretrained} \\
\midrule
Accuracy        & 97.0\%     & 95.0\% \\
F1-score (avg)  & 0.98       & 0.96  \\
Precision       & 0.98       & 0.96  \\
Recall          & 0.97       & 0.95  \\
\bottomrule
\end{tabular}
\caption{NER performance comparison}
\end{table}

These results show that the CRF model slightly outperformed SpaCy's pre-trained model, particularly in terms of F1-score and recall. This suggests better consistency and robustness on rarer entity types.

\subsection*{Relation Extraction}
Three rule-based syntactic patterns were used: active voice (nsubj), passive voice (nsubjpass with agent), and compound noun handling. The extracted relations were validated against expected results in the articles.

\subsection*{Knowledge Graph Statistics}

\begin{table}[H]
\centering
\begin{tabular}{lcc}
\toprule
\textbf{Metric} & \textbf{Original Graph} & \textbf{Augmented Graph} \\
\midrule
Number of triples         & 875   & 2360  \\
Unique entities           & 917   & 2051  \\
Unique relations          & 300   & 338   \\
\bottomrule
\end{tabular}
\caption{Knowledge graph statistics before and after augmentation}
\end{table}

\textit{The augmented graph is significantly denser and more diverse, with over twice as many entities and relations. This provides a richer structure for learning embeddings.}

\subsection*{Embedding Evaluation}

\begin{table}[H]
\centering
\begin{tabular}{lcccc}
\toprule
\textbf{Metric} & \textbf{TransE (orig)} & \textbf{DistMult (orig)} & \textbf{TransE (aug)} & \textbf{DistMult (aug)} \\
\midrule
Mean Rank              & 337.36 & 475.25 & 385.59 & 502.89 \\
MRR                    & 0.0030 & 0.0021 & 0.0026 & 0.0020 \\
Hits@1                 & 0.0625 & 0.0114 & 0.0593 & 0.1292 \\
Hits@3                 & 0.0852 & 0.0114 & 0.1335 & 0.2034 \\
Hits@10                & 0.1534 & 0.0114 & 0.2394 & 0.2860 \\
\bottomrule
\end{tabular}
\caption{Knowledge graph embedding results}
\end{table}

\textit{Although the scores remain low overall due to the complexity and noise of the data, augmentation clearly improved Hits@3 and Hits@10 for both models, especially for DistMult. This suggests better generalization and link structure in the augmented graph. TransE remained more sensitive to noise and performed less consistently.}

\subsection*{Entity Similarity Example}
Cosine similarity was computed between entity embeddings. Results allow identifying semantically close nodes. Exact values depend on entity indexing and were analyzed during notebook execution.

\subsection*{Link Prediction Example}
The model scores tail candidates for a fixed head and relation. Examples were executed in the notebook and revealed that semantic relevance improved after augmentation.

\section{Challenges and Solutions}

\begin{itemize}
    \item \textbf{Sparse Initial Graph:} Augmentation via DBpedia added connectivity and diversity.
    \item \textbf{Relation Extraction Errors:} Refinement of SpaCy rule-based patterns improved precision.
    \item \textbf{Entity Linking Heuristics:} Used surface label heuristics and language filters to reduce errors.
\end{itemize}

\section{Examples of Extracted Knowledge}

\textbf{From articles:}
\begin{itemize}
    \item (Apple, founded\_by, Steve Jobs)
    \item (Apple, located\_in, California)
\end{itemize}

\textbf{From DBpedia:}
\begin{itemize}
    \item (Steve Jobs, dbo:birthPlace, San Francisco)
    \item (Apple Inc., dbo:industry, Consumer electronics)
\end{itemize}

\section{Visualizations of the Knowledge Graph}

\subsection*{Before Data Augmentation}

\begin{figure}[H]
\centering
\includegraphics[width=0.25\textwidth]{output1.png}
\caption{TransE embeddings before data augmentation}
\end{figure}

\begin{figure}[H]
\centering
\includegraphics[width=0.25\textwidth]{output_2.png}
\caption{DistMult embeddings before data augmentation}
\end{figure}

\subsection*{After Data Augmentation}

\begin{figure}[H]
\centering
\includegraphics[width=0.25\textwidth]{output_3.png}
\caption{TransE embeddings after data augmentation}
\end{figure}

\begin{figure}[H]
\centering
\includegraphics[width=0.25\textwidth]{output_4.png}
\caption{DistMult embeddings after data augmentation}
\end{figure}

\textit{The t-SNE plots show that the augmented graphs yield more structured and interpretable embeddings, with improved separation between clusters.}

\section{Comparison Between Simple and Augmented Data}

Data augmentation yielded mixed results. While DistMult showed clear improvements in Hits@k metrics, TransE's gains were less consistent. However, similarity analysis and visualizations confirmed better semantic grouping post-augmentation. The enriched graph enabled the models to better learn entity types and relations, despite the increased complexity.

\section{Conclusion}

This project demonstrated a full pipeline from web scraping to knowledge graph embedding. Although initial data was sparse and noisy, augmentation via DBpedia significantly improved the structure and utility of the graph. Embedding models captured semantic patterns, and the visualizations confirmed improved interpretability. Future work could explore deeper NLU, ontology alignment, and more robust entity disambiguation.

\end{document}
