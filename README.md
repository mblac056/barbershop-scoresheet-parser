# Barbershop Scores Parser

A Python tool for parsing barbershop competition scoresheets from PDF format into CSV and JSON formats. This tool supports both single-round and multi-round scoresheets and is optimized for deployment as an [AWS Lambda](https://aws.amazon.com/lambda/) container image.

## ✨ Features

- Extracts tables from PDF scoresheets using [Tabula](https://tabula.technology/) via `tabula-py`
- Parses results into structured JSON
- Generates a pivot-friendly CSV for further analysis
- Deployable via container to AWS Lambda for low-cost, on-demand use
- Also usable locally via CLI

---

## 🚀 Cloud Deployment (Lambda + Docker)

This project is containerized for AWS Lambda, allowing you to run the parser on-demand via an API Gateway endpoint. See `Dockerfile` and `app.py` for the Lambda-compatible entry point.

### 1. Build the Docker image

```bash
docker build -t barbershop-parser .
```

### 2. Push to Amazon ECR

```bash
aws ecr create-repository --repository-name barbershop-parser
aws ecr get-login-password | docker login --username AWS --password-stdin <your-ecr-url>

docker tag barbershop-parser:latest <your-ecr-url>/barbershop-parser:latest
docker push <your-ecr-url>/barbershop-parser:latest
```

### 3. Create the Lambda function

```bash
aws lambda create-function \
  --function-name parseBarbershopScoresheet \
  --package-type Image \
  --code ImageUri=<your-ecr-url>/barbershop-parser:latest \
  --role arn:aws:iam::<your-account-id>:role/your-execution-role
```

### 4. Optional: Connect via API Gateway

Set up an HTTP API to call this function from the web.

---

## 🖥️ Local Usage (CLI)

If you'd rather run it locally:

### 1. Clone the repository

```bash
git clone https://github.com/mblac056/barbershop-scores-parser.git
cd barbershop-scores-parser
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the parser

```bash
python -m parser.scoresheet_parser -i path/to/scoresheet.pdf
```

This will generate three files:

- `[filename].csv`: Raw table data extracted from the PDF
- `[filename].json`: Structured data with rounds and song scores
- `[filename]_pivot.csv`: Pivot-format data for spreadsheets

---

## 📁 Output Formats

### JSON Example

```json
[
  {
    "placement": 1,
    "group": "Group Name",
    "combined_total_scores": {
      "MUS": 94.5,
      "PER": 93.8,
      "SNG": 95.0,
      "Total": 94.4
    },
    "rounds": {
      "Finals": {
        "scores": {
          "MUS": 95.8,
          "PER": 94.9,
          "SNG": 96.0,
          "Total": 95.5
        },
        "songs": [
          {
            "title": "Song Title",
            "scores": {
              "MUS": 96.0,
              "PER": 95.3,
              "SNG": 96.5,
              "Total": 95.9
            }
          }
        ]
      }
    }
  }
]
```

### Pivot CSV Columns

- Group
- Round
- Song
- Category
- Score

---

## 📦 Requirements

- Python 3.6+
- Java 11+
- tabula-py
- pandas

For Lambda, all requirements are pre-installed via the Docker container.

---

## 📄 License

MIT License