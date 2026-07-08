# NL2SQL Transformer
A custom encoder-decoder Transformer implemented in TensorFlow for natural language to SQL translation. Developed as part of a graduate NLP research project.

# Data
Data and scheme was based upon the 2023-2024 NBA Championship season of the Boston Celtics taking the initial data set from: **https://github.com/swar/nba_api**

Pulled data into a Dataframe so as to figure out schema being used, from here I used that scheme to model my SQL queries. I proceeded to create 110 individual SQL queries based on the schema.

Once I had my base SQL queries created I generated synthetic natural language prompts utilizing Claude. I prompted Claude to produce 8 unique natural language sentences for each SQL query using vocabulary ranging from a casual fan to a professional analyst. The reasoning for this is so that we can get different vocabulary that means the same thing so that when we go to test the data we can accurately create SQL prompts even if the wordage is not quite the same; e.g. "scored" vs "dropped".

From here I created a .CSV file formated as follows:
Column 1 - SQL Query #
Column 2 - NL Prompt
Column 3 - SQL Query Code

# Model
My choice of model for this project was a Cross-Attention Transformer with Multi-Headed Attention.

The reasoning behind this is that we Cross-Attention allows for the Encoder to break down the initial input and feed the information back into the Decoder as it creates the output we are looking for. Multi-Headed Attention is used because it allows our model to compare each word in the dataset to multiple different relationships and patterns in the data.

