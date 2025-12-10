# Stock Investment Screener Pipeline

Data engineering portfolio project demonstrating ETL pipeline design, Delta Lake implementation, and data quality practices.

## Project Goal
Build an automated pipeline that analyzes 2,700+ NYSE stocks to identify investment opportunities based on:
- Consistent dividend payments (10+ years)
- Strong price growth (20%+ over 10 years)

## Tech Stack
- **Platform**: Databricks Free Edition (Serverless)
- **Storage**: Delta Lake with Medallion Architecture
- **Data Source**: Yahoo Finance API (yfinance)
- **Language**: Python, SQL

## Architecture

### Bronze Layer (Raw Data)
- `bronze.stock_reference.company_info` - 2,700+ NYSE company metadata
- `bronze.stock_data.price_history` - 10 years weekly prices, dividends, splits (1M+ rows)

### Silver Layer (Calculated Metrics)
- `silver.stock_metrics.dividend_quality` - Dividend payment analysis
- `silver.stock_metrics.price_growth` - 10-year growth calculations

### Gold Layer (Final Output)
- `gold.investment.stock_rankings` - Ranked investment recommendations

## Current Status
- Bronze layer complete  
  - 2,350 stocks successfully loaded (87% success rate)
  - 1,031,022 rows of price/dividend data
  - 10 years historical data (2015-2025)
  - 76,172 dividend payment records
- Silver layer in progress  
- Gold layer planned

## Data Pipeline
<img width="1720" height="420" alt="stock-picker_architecture" src="https://github.com/user-attachments/assets/b5f4ba0c-7a97-477c-b61f-4936d485ce43" />

## Key Features
- Batch processing for 2,700+ stocks
- Delta Lake partitioning by symbol
- Data quality validation queries
- Incremental processing capability

## Sample Queries

**Check dividend-paying stocks:**
```sql
SELECT symbol, years_with_dividends, avg_annual_dividend
FROM silver.stock_metrics.dividend_quality
WHERE passes_dividend_test = TRUE
ORDER BY avg_annual_dividend DESC
LIMIT 10;
```

## Project Structure
```
notebooks/          # Databricks notebooks
data/               # Source CSV files
docs/               # Documentation
results/            # Output files
```

## Running the Pipeline
1. Upload NYSE tickers CSV to `/data`
2. Run `1_bronze_company_info.py`
3. Run `2_bronze_price_history.py` (takes ~30-45 min)
4. Run Silver layer notebooks (in progress)

## Next Steps
- Complete Silver layer metrics
- Implement Gold layer rankings
- Add LLM analysis via HuggingFace API
- Create data quality dashboard
