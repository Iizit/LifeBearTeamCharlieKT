![Designer](Designer.jpeg)

# LifeBearTeamCharlieKT
[LifeBear Valid Data](https://cynatglobal.sharepoint.com/:x:/s/AIAnalyst/EWSky9WkKDdElkLuisg5nF0BaUy0jyFTF06VXfefuZ6OfA?e=ox1Vr8)

# Japan User Database Processing Script

# Introduction/Overview

This script processes a large dataset containing user information from [invalid URL removed]. It performs the following tasks:

Reads the CSV file using pandas
Removes duplicate entries based on mail_address and login_id
Validates email addresses using a regular expression
Converts created_at column to datetime format
Formats datetime strings for better readability
Splits the data into chunks for efficient processing
Analyzes each chunk and exports a preview
Exports valid data to a new CSV file
Exports invalid emails to a separate CSV file
Requirements

Python 3.x
pandas library

# Instructions

Install pandas: pip install pandas
Download the CSV file (3.6M-Japan-lifebear.com-Largest-Notebook-App-UsersDB-csv-2019.csv) and place it in the same directory as this script.
Run the script.
Output

valid_data.csv: Contains the cleaned and processed user data.
dump.csv: Contains invalid email addresses extracted from the original data.
Chunks directory (if not already existing): Stores temporary CSV files for analysis. These can be deleted after processing is complete.

# Code Snippets

# Reading CSV and Removing Duplicates

'''
import pandas as pd

df = pd.read_csv('/content/3.6M-Japan-lifebear.com-Largest-Notebook-App-UsersDB-csv-2019.csv', sep=';', low_memory=True)
df.drop_duplicates(subset=['mail_address', 'login_id'], keep='first', inplace=True)
Use code with caution.
'''

# Validating Emails

'''
email_regex = r"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$"
invalid_emails_df = pd.DataFrame(columns=df.columns)

for index, row in df.iterrows():
    email = row['mail_address']
    if not re.match(email_regex, str(email)):
        invalid_emails_df = pd.concat([invalid_emails_df, pd.DataFrame([row])], ignore_index=True)
        df.drop(index, inplace=True)
'''

# Processing Created_at Column

'''
df['created_at'] = pd.to_datetime(df['created_at'], errors='coerce')
df['created_at'] = df['created_at'].dt.strftime('%A, %B %d, %Y %I:%M %p')
'''

# Chunking and Analysis

'''chunk_size = len(df) // 7

for i in range(7):
    start_index = i * chunk_size
    end_index = (i + 1) * chunk_size if i < 6 else len(df)
    chunk = df[start_index:end_index]

    print(f"Chunk {i+1}:")
    print(chunk.head(5))  # Print first 5 rows
    print("...")
    print(chunk.tail(5))  # Print last 5 rows
    print("-" * 20)
'''

# Exporting Results

'''
invalid_emails_df.to_csv('/content/dump.csv', index=False)
chunks.to_csv(f'/content/Chunks/chunk_{i+1}.csv', index=False)
merged_df.to_csv('/content/valid_data.csv', index=False)

print("Merged data exported to 'valid_data.csv'")

'''

# Additional Notes

The script uses regular expressions for email validation, which may not cover every edge case. You might want to consider a more sophisticated email validation library for stricter checks.
Error handling can be added to the script to make it more robust.
