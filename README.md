


# ETL for Company List in Nepal

This project extracts data from the Wikipedia page listing companies in Nepal, transforms it into a structured format, and loads it into a PostgreSQL database. The data includes information about the companies' names, industries, sectors, headquarters, founding year, and additional notes.

## Table of Contents

- [Project Overview](#project-overview)
- [Installation](#installation)
- [Data Source](#data-source)
- [ETL Process](#etl-process)
  - [Extraction](#extraction)
  - [Transformation](#transformation)
  - [Loading into CSV](#loading-into-csv)
  - [Loading into PostgreSQL](#loading-into-postgresql)
- [Usage](#usage)
- [PostgreSQL Setup](#postgresql-setup)
- [Dependencies](#dependencies)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

This project automates the process of gathering and organizing data about companies in Nepal from Wikipedia. It uses web scraping techniques to retrieve the data, then transforms and loads it into a PostgreSQL database for further analysis.

## Installation

1. Clone the repository:

    ```bash
    git clone https://github.com/your-repo-name/company-list-nepal.git
    cd company-list-nepal
    ```

2. Install the required dependencies:

    ```bash
    pip install -r requirements.txt
    ```

3. Run the Jupyter Notebook to execute the ETL process.

## Data Source

The data is scraped from the following Wikipedia page:
- [List of Companies of Nepal](https://en.wikipedia.org/wiki/List_of_companies_of_Nepal)

## ETL Process

### Extraction

The extraction process uses the `requests` library to fetch the HTML content of the Wikipedia page. The `BeautifulSoup` library is used to parse the HTML and extract the table containing the company information.

### Transformation

Once the raw HTML table is extracted, the data is cleaned and organized into columns:
- Name
- Industry
- Sector
- Headquarters
- Founded
- Notes

The data is then transformed into a pandas DataFrame for easy manipulation and further processing.

### Loading into CSV

After the data is extracted and transformed, it is saved as a CSV file in the project directory for future use:

```python
df.to_csv('company_list_nepal.csv', index=False)
```

### Loading into PostgreSQL

The transformed data is also loaded into a PostgreSQL database for structured querying and analysis. The process involves:

1. Establishing a connection to the PostgreSQL database using `psycopg2`.
2. Creating a table named `company_list` with the necessary columns.
3. Inserting the transformed data into the PostgreSQL table.

```python
import psycopg2

# Database connection setup
conn = psycopg2.connect(
    dbname="your_db_name",
    user="your_username",
    password="your_password",
    host="localhost",
    port="5432"
)
cursor = conn.cursor()

# Create table if it doesn't exist
cursor.execute('''
    CREATE TABLE IF NOT EXISTS company_list (
        id SERIAL PRIMARY KEY,
        name TEXT,
        industry TEXT,
        sector TEXT,
        headquarters TEXT,
        founded TEXT,
        notes TEXT
    );
''')

# Insert data into the table
for index, row in df.iterrows():
    cursor.execute('''
        INSERT INTO company_list (name, industry, sector, headquarters, founded, notes)
        VALUES (%s, %s, %s, %s, %s, %s)
    ''', (row['Name'], row['Industry'], row['Sector'], row['Headquarters'], row['Founded'], row['Notes']))

# Commit changes and close the connection
conn.commit()
cursor.close()
conn.close()
```

## Usage

To run the ETL process, execute the Jupyter Notebook `company_list_Nepal.ipynb`. The final output will be:
- A CSV file containing the structured list of companies from Nepal.
- The same data loaded into a PostgreSQL database.

## PostgreSQL Setup

To load the data into PostgreSQL, make sure you have PostgreSQL installed and running. Create a new database and update the connection parameters in the script with your PostgreSQL credentials.

1. Install PostgreSQL: [PostgreSQL Installation Guide](https://www.postgresql.org/download/)
2. Create a new database:

    ```bash
    createdb your_db_name
    ```

3. Update the connection details in the script to match your PostgreSQL configuration.

## Dependencies

The project requires the following Python libraries:
- `beautifulsoup4`
- `requests`
- `pandas`
- `psycopg2`

To install the dependencies, run:

```bash
pip install -r requirements.txt
```

## Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the LICENSE file for details.
