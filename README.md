# Barbershop Scores Parser

A Python tool for parsing barbershop competition scoresheets from PDF format into CSV and JSON formats. This tool can handle both single-round and multi-round competition scoresheets.

## Installation

1. Clone this repository:

bash
git clone https://github.com/mblac056/barbershop-scores-parser.git
cd barbershop-scores-parser

2. Install dependencies:

bash
pip install -r requirements.txt

## Usage

Basic usage:
bash
python -m oss_parser.parse_oss -i path/to/scoresheet.pdf

This will generate three files in the same directory as your input PDF:
- `[filename].csv`: Raw data extracted from the PDF
- `[filename].json`: Structured data with rounds and songs
- `[filename]_pivot.csv`: Data formatted for pivot table analysis

### Output Formats

#### JSON Structure
The JSON output is structured as follows:

json
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

#### Pivot CSV Structure
The pivot CSV contains the following columns:
- Group
- Round
- Song
- Category
- Score

This format is ideal for creating pivot tables and data analysis in spreadsheet software.

## Requirements
- Python 3.6+
- tabula-py
- pandas

## License
[MIT License](LICENSE)