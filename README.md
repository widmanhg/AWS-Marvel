
# 📽️ Marvel Movie Scraper - AWS Lambda

This AWS Lambda function automatically scrapes, processes, and stores data about Marvel Cinematic Universe (MCU) movies and characters **once per day** using an **Amazon EventBridge rule** with the expression `rate(1 day)`.

## 🚀 Overview

The Lambda function performs the following actions:

1. **Scrapes MCU film data** from Wikipedia.
2. **Cleans and formats** the movie dataset.
3. **Enriches film data** with metadata from the OMDB API.
4. **Extracts recurring characters** from a specific Wikipedia table.
5. **Saves the datasets** as CSV files into an S3 bucket.

---

## 🧠 Architecture

- **Trigger**: Scheduled daily via **EventBridge** using `rate(1 day)`.
- **Lambda Layer**: Includes required Python packages such as `pandas`, `beautifulsoup4`, and `requests`.
- **IAM Role**: Grants access to:
  - **Amazon S3**: To upload the CSV files.
  - **AWS Lambda**: For basic execution permissions.

### 🖼️ Diagram
![WhatsApp Image 2025-05-15 at 22 34 53_c2495a47](https://github.com/user-attachments/assets/cc6b94ce-ef60-4edf-992e-48ca617e364a)


---

## 🗂️ S3 Output Structure

Once the Lambda function runs, it stores three CSV files in the specified S3 bucket:

```
s3://marveltest-2209090-widman/
├── movies.csv
├── omdb.csv
└── characters.csv
```


### 🖼️ Example Screenshot
![WhatsApp Image 2025-05-15 at 22 35 29_3ca4d409](https://github.com/user-attachments/assets/14709ae5-06c6-4a23-bd8c-64066d2eff58)


---

## 🧪 Sample Code

This project includes Python functions to:

- Scrape Marvel movies from Wikipedia
- Clean and structure the data
- Pull metadata from OMDB API
- Scrape characters table
- Upload all DataFrames to S3

### Lambda Handler
```python
def lambda_handler(event, context):
    movies_df = scrape_marvel_movies()
    movies_df_cleaned = clean_movie_data(movies_df)

    omdb_results = [fetch_omdb_data(film) for film in movies_df_cleaned['film']]
    omdb_df = pd.DataFrame(omdb_results)

    characters_df = scrape_characters_data()

    upload_to_s3(movies_df_cleaned, 'movies.csv')
    upload_to_s3(omdb_df, 'omdb.csv')
    upload_to_s3(characters_df, 'characters.csv')

    return {
        'statusCode': 200,
        'body': 'Data uploaded successfully!'
    }
```

---

## 🔐 Environment Configuration

These variables are defined directly in the script:
```python
OMDB_API_KEY = "your_omdb_api_key"
S3_BUCKET_NAME = "your_s3_bucket_name"
```

You can move them to **environment variables** in the Lambda console for better security and flexibility.

---

## 📦 Requirements

The Lambda Layer must include:

- `pandas`
- `requests`
- `beautifulsoup4`
- `boto3`

---


