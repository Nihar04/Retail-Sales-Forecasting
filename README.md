# 🛒 Retail Sales Forecast

A comprehensive data science solution for forecasting retail sales using distributed data processing, machine learning, and time series analysis. This project leverages Apache Spark for scalable data processing and Prophet for accurate sales predictions with confidence intervals.

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Output Files](#-output-files)
- [Key Components](#-key-components)
- [Contributing](#-contributing)
- [License](#-license)

## ✨ Features
- 🔄 **Distributed Data Processing**: Leverages Apache Spark for handling large-scale retail data
- 📈 **Time Series Forecasting**: Uses Facebook's Prophet model for accurate 90-day sales predictions
- 🏪 **Multi-Store Support**: Generates individual forecasts for each store with personalized models
- 📊 **Seasonal Decomposition**: Analyzes trend, seasonality, and residual components
- 💾 **MongoDB Integration**: Stores forecasts and metrics in MongoDB for easy querying
- 📉 **Tableau Integration**: Prepares visualization-ready data for business intelligence dashboards
- 🔍 **Data Quality Validation**: Comprehensive data validation and missing value handling
- 📋 **Store Analytics**: Identifies top-performing stores and calculates performance metrics

## 🛠️ Tech Stack
### Data Processing
- **PySpark 3.5.1** - Distributed data processing framework
- **Pandas** -  Data manipulation and analysis
- **NumPy** - Numerical computing
### Machine Learning
- **Prophet** - Time series forecasting model
- **Statsmodels** - Statistical modeling and seasonal decomposition
- **Scikit-learn** - Machine learning metrics (MAE, RMSE, MAPE)
### Database
- **PyMongo** - MongoDB database connectivity### Visualization
- **Matplotlib** - Static plotting
- **Seaborn** - Statistical visualization
- **Tableau** - Business intelligence dashboards

## 📦 Prerequisites
Before running this project, ensure you have:
- **Python 3.7+**
- **Java 8 or higher** (required for PySpark)
- **MongoDB** (optional, for data persistence)
- **Git** (for cloning the repository)

## 🚀 Installation
### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/retailsalesforecast.git
cd retailsalesforecast
```

### 2. Create a Virtual Environment (Recommended)
```bash
# Using venv
python -m venv venv
# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Install MongoDB (Optional)
If you want to use MongoDB for data persistence:
- **Windows**: Download from [MongoDB Download Center](https://www.mongodb.com/try/download/community)
- **macOS**: `brew install mongodb-community`
- **Linux**: Follow [MongoDB Installation Guide](https://docs.mongodb.com/manual/installation/)

Start MongoDB service:
```
bash# Windows
net start MongoDB
# macOS/Linux
brew services start mongodb-community
# or
sudo systemctl start mongod
```

### 5. Prepare Data
Place your retail sales data in CSV format in the `data/raw/` directory:

```csv
date,store,sales,promo,holiday
2020-01-01,1,5000.0,1,1
2020-01-02,1,4500.0,0,0
...
```
**Expected CSV Schema**:
- `date`: Date in yyyy-MM-dd format
- `store`: Integer (store identifier)
- `sales`: Double (sales amount)
- `promo`: Integer (0 or 1, promotional flag)
- `holiday`: Integer (0 or 1, holiday flag)

## 💻 Usage
### Run the Complete Pipeline
```bash
python main.py
```
The pipeline will:
1. ✅ Load and validate data from `data/raw/store_sales.csv`
2. ✅ Preprocess and clean the data
3. ✅ Add time-based features
4. ✅ Generate forecasts for all stores (90 days ahead)
5. ✅ Perform seasonal decomposition
6. ✅ Save forecasts to MongoDB (if available)
7. ✅ Generate visualization files for Tableau

### Output Files
After execution, you'll find the following files in the `outputs/` directory:
- **forecasts.csv** - Complete forecast dataset with confidence intervals
- **master_forecast_data.csv** - Master dataset for Tableau visualization
- **top_stores.csv** - Top 5 performing stores
- **seasonal_trends.csv** - Monthly average forecast trends
- **store_forecasts.csv** - Store-wise forecast summaries

### Processed Data
Processed data in Parquet format is saved in `data/processed/`:
- `features/` - Feature-engineered data
- `store_aggregated/` - Store-aggregated sales
- `store_stats/` - Store statistics

## 📁 Project Structure
# Using venv
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activateU cores)
- **Driver Memory**: 4GB
- **Executor Memory**: 4GB
- **Shuffle Partitions**: 8

### MongoDB Configuration

MongoDB connection settings in `src/mongodb_handler.py`:
- **Default URI**: `mongodb://localhost:27017/`
- **Database**: `retail_sales`
- **Collections**: `forecasts`, `store_metrics`

To change MongoDB connection:
```python
mongo_handler = MongoDBHandler(
    uri="mongodb://your-host:27017/",
    db_name="your_database"
)
```

### Forecast Parameters

Modify forecast horizon in `src/forecasting.py`:
```python
self.forecast_horizon = 90  # Change to desired days
```

## 📊 Key Components
### 1. DataLoader
- Loads CSV files into Spark DataFrames
- Validates data schema and quality
- Reports missing values and data statistics

### 2. DataPreprocessor
- Removes duplicates and handles missing values
- Adds temporal features (year, month, day, dayofweek, quarter, week)
- Aggregates sales by store and date
- Calculates store-level statistics

### 3. TimeSeriesForecaster
- Implements Prophet forecasting model
- Generates 90-day forecasts with confidence intervals
- Performs seasonal decomposition (trend, seasonality, residual)
- Calculates forecast accuracy metrics (MAE, RMSE, MAPE)

### 4. MongoDBHandler
- Manages MongoDB connections
- Stores forecasts and metrics
- Queries top-performing stores

### 5. VisualizationPrep
- Prepares data for Tableau dashboards
- Generates top stores rankings
- Creates seasonal trend summaries

## 📈 Model Details
### Prophet Model Configuration

- **Yearly Seasonality**: Enabled
- **Weekly Seasonality**: Enabled
- **Daily Seasonality**: Disabled
- **Confidence Interval**: 95%
- **Forecast Horizon**: 90 days
- **External Regressors**: Promotional data (if available)

### Forecast Output Schema

| Column | Description |
|--------|-------------|
| `date` | Forecast date |
| `forecast_sales` | Predicted sales value |
| `lower_bound` | Lower 95% confidence bound |
| `upper_bound` | Upper 95% confidence bound |
| `store` | Store identifier |
| `forecast_date` | Timestamp when forecast was generated |

## 🔧 Troubleshooting
### PySpark Issues

**Error**: "Missing Python executable 'python3'"
- **Solution**: PySpark is configured to use the current Python interpreter automatically

**Error**: Hadoop winutils warnings on Windows
- **Solution**: These warnings are benign in local mode and can be ignored

### MongoDB Issues
**Error**: "MongoDB connection failed"
- **Solution**: Ensure MongoDB is running. The project will continue with file-based outputs if MongoDB is unavailable.

### Memory Issues
If you encounter memory errors:
- Reduce Spark driver/executor memory in `config/spark_config.py`
- Process data in smaller batches
- Use Spark's distributed mode for larger datasets

## 🤝 Contributing
Contributions are welcome! Please follow these steps:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines

- Follow PEP 8 style guidelines
- Add docstrings to new functions and classes
- Include unit tests for new features
- Update documentation as needed

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
## 🙏 Acknowledgments

- Facebook for the Prophet forecasting library
- Apache Spark community for the distributed computing framework
- Contributors and open-source community
## 📚 Additional Resources

- [Prophet Documentation](https://facebook.github.io/prophet/)
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [MongoDB Documentation](https://docs.mongodb.com/)

---

📁 Project Structure

retailsalesforecast/
├── main.py                      # Main execution script
├── requirements.txt            # Python dependencies
├── README.md                   # Project documentation
├── PROJECT_REPORT.md           # Detailed project report
│
├── config/                      # Configuration files
│   ├── spark_config.py         # Spark session configuration
│   └── mongodb_config.py       # MongoDB connection settings
│
├── src/                         # Source code modules
│   ├── data_loader.py          # Data loading and validation
│   ├── preprocessing.py        # Data cleaning and feature engineering
│   ├── forecasting.py         # Time series forecasting models
│   ├── mongodb_handler.py      # Database operations
│   └── visualization_prep.py   # Visualization data preparation
│
├── data/                        # Data storage
│   ├── raw/                    # Raw input data (CSV)
│   └── processed/              # Processed data (Parquet)
│       ├── features/
│       ├── store_aggregated/
│       └── store_stats/
│
├── outputs/                     # Generated outputs
│   ├── forecasts.csv
│   ├── master_forecast_data.csv
│   ├── top_stores.csv
│   ├── seasonal_trends.csv
│   └── store_forecasts.csv
│
└── notebooks/                   # Jupyter notebooks
    └── analysis.ipynb          # Data analysis notebook


⭐ If you find this project helpful, please give it a star on GitHub!
