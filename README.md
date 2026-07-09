# NL2SQL Transformer

A custom encoder-decoder Transformer implemented in TensorFlow for Natural Language to SQL (NL→SQL) translation. This project was developed as part of my graduate Natural Language Processing course at Northeastern University with the goal of building and understanding a Transformer architecture rather than relying on existing pretrained models.

---

# Dataset

The dataset is based on the Boston Celtics' 2023–2024 championship season. The original player and game statistics were obtained using the `nba_api` package developed by **swar**, and were used to understand the database schema and relationships between the tables.

Rather than using an existing NL→SQL dataset, I built my own synthetic training dataset. I first designed and wrote 110 SQL queries covering player statistics, team statistics, aggregates, filters, rankings, and game information based on the available schema.

Once the SQL queries were complete, I used Claude to generate synthetic natural language prompts for each query. For every SQL statement, Claude generated eight unique paraphrases ranging from casual fan terminology to more analytical basketball language. The goal was to expose the model to multiple ways of asking the same question. For example, a user might ask who "scored the most points" while someone else might ask who "dropped the most points." Although the wording is different, both questions should generate the same SQL query.

The final dataset contains three columns:

* SQL Query ID
* Natural Language Prompt
* SQL Query

This resulted in an 880-example synthetic NL→SQL training dataset.

---

# Model

For this project I chose to implement an encoder-decoder Transformer with Multi-Head Attention and Cross-Attention using TensorFlow.

The encoder processes the natural language question and learns contextual relationships between the words in the sentence. The decoder then generates the SQL query one token at a time while using cross-attention to reference the encoder's output throughout the generation process.

I chose Multi-Head Attention because it allows the model to learn multiple relationships within the same sentence simultaneously instead of relying on a single attention pattern. This helps the model recognize different ways users may phrase similar questions while still producing the correct SQL query.

---

# Results

The model was evaluated on a random 25-example sample from the generated NL→SQL dataset.

| Metric | Result |
|---|---:|
| Exact Match Accuracy | 84.00% |
| SacreBLEU | 77.6 |

The model performed well on player-specific aggregation queries such as `MIN`, `MAX`, `SUM`, `COUNT`, and `AVG`. The main failure cases were threshold-based filtering queries, such as questions asking for players above or below a certain points or rebounds value. I believe this is likely due to the relatively small training dataset and the limited number of threshold-based query examples available during training. In the future, I plan to expand the dataset with additional threshold-based queries to improve the model's ability to generalize to these patterns.

Overall, the results show that the custom TensorFlow Transformer learned many of the core SQL query patterns in the dataset while still leaving room for improvement on less frequent query structures.
