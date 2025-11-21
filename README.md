# Netflix Global Pricing Analysis

A comprehensive data analysis examining how Netflix subscription prices and content availability vary across 128 countries worldwide. This project explores pricing strategies, regional patterns, and value propositions in the global streaming market.

## Project Overview

This analysis investigates Netflix's global pricing structure using 2025 subscription data. The study reveals significant variations in both subscription costs and content library sizes across different countries and regions, offering insights into Netflix's market-specific pricing strategies.

### Key Research Questions

1. How much do Netflix subscription prices vary between countries?
2. Which countries offer the best value for money?
3. How does the ad-supported tier compare to traditional subscription plans?
4. What pricing patterns emerge across different world regions?
5. Is there a correlation between content library size and subscription cost?

## Key Findings

### Global Coverage
- Netflix operates in **128 countries** across six continents
- Europe represents the largest market with the most country coverage
- Pricing strategies vary dramatically based on local economic conditions

### Content Availability
- Library sizes range from **4,883 to 9,765 titles**
- Average library contains approximately **7,100 titles**
- **Iceland** offers the largest content catalog
- Content variation stems from licensing agreements and regional regulations

### Pricing Patterns
- Premium subscription costs vary by approximately **6-7x** between countries
- **Switzerland and Liechtenstein** charge the highest rates (>$7.50/month)
- **Pakistan, Nigeria, and Egypt** offer the lowest prices (<$2.00/month)
- European markets show consistently higher subscription costs

### Value Analysis
- Best value markets combine low prices with large libraries
- Developing economies in Asia and Africa generally provide better value per dollar
- Cost per title varies significantly, from $0.0016 to $0.0096

### Ad-Supported Tier
- Available in **15 select markets**
- Provides **30-60% savings** compared to Basic plan
- Represents Netflix's response to increasing market competition

## Dataset

**Source:** Netflix Subscription Price Dataset 2025

**Coverage:**
- 128 countries across 6 regions
- 4 subscription tiers (Basic with Ads, Basic, Standard, Premium)
- Content library sizes for each market

**Data Fields:**
- Country and regional classification
- Library size (total titles available)
- Subscription costs for all tiers (USD)
- Cost per title metrics

## Project Structure

```
netflix-subscriptions-data-analysis/
├── netflix-2025.csv                          # Raw 2025 dataset
├── netflix dec 2021.csv                      # Historical 2021 dataset
├── netflix-data-analysis-2025.ipynb         # Primary analysis notebook (2025)
├── netflix-data-analysis-2021.ipynb         # Original analysis (archived)
├── docs/
│   ├── index.html                            # Interactive dashboard
│   ├── images/                               # Generated visualizations
│   ├── summary.json                          # Key statistics (auto-generated)
│   └── data.json                             # Processed dataset (auto-generated)
└── README.md                                 # Project documentation
```

## Running the Analysis

### Prerequisites

```bash
# Python 3.9 or higher recommended
pip install pandas numpy matplotlib seaborn pycountry-convert jupyter
```

### Execute the Analysis

1. **Clone the repository:**
   ```bash
   git clone https://github.com/priankr/netflix-subscriptions-data-analysis.git
   cd netflix-subscriptions-data-analysis
   ```

2. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook
   ```

3. **Open the analysis file:**
   - Navigate to `netflix-data-analysis-2025.ipynb`
   - Run all cells to regenerate the complete analysis

4. **View results:**
   - Visualizations are automatically saved to `docs/images/`
   - Summary statistics export to `docs/summary.json`
   - Dashboard opens `docs/index.html` in any web browser

### View the Dashboard

The interactive dashboard can be viewed without running the Python analysis:

1. **Local viewing:**
   - Open `docs/index.html` directly in your web browser
   - All visualizations and data are embedded

2. **GitHub Pages (if deployed):**
   - Visit the live dashboard at your GitHub Pages URL
   - Configure in repository Settings → Pages → Deploy from `/docs` folder

## Analysis Methodology

### Data Processing

1. **Data Cleaning:**
   - Normalize column names and remove empty fields
   - Calculate monthly costs from cost-per-title metrics
   - Handle missing values for markets without certain subscription tiers

2. **Regional Classification:**
   - Map countries to continental regions using ISO standards
   - Handle special cases for territories and disputed regions

3. **Value Metrics:**
   - Calculate cost per title for each tier and market
   - Compare regional averages and identify outliers
   - Analyze price spreads between subscription tiers

### Visualizations

The analysis generates comprehensive visualizations including:

- **Distribution plots:** Library sizes and subscription costs
- **Comparative charts:** Regional pricing patterns and content availability
- **Correlation analysis:** Relationship between library size and cost
- **Value rankings:** Best and worst markets by cost per title
- **Tier analysis:** Ad-supported vs traditional plan savings

### Statistical Methods

- Descriptive statistics (mean, median, standard deviation)
- Correlation analysis between variables
- Regional aggregation and comparison
- Outlier identification and interpretation

## Interactive Dashboard Features

The web dashboard provides:

- **Key metrics summary:** Countries analyzed, average pricing, variation ranges
- **Regional breakdowns:** Coverage and pricing by continent
- **Interactive charts:** Explore pricing extremes and library sizes
- **Value analysis:** Compare cost per title across markets
- **Searchable table:** Filter and explore all 128 countries
- **Responsive design:** Works on desktop and mobile devices

## Insights and Implications

### For Consumers

- **Best value markets:** Pakistan, Nigeria, and India offer the lowest cost per title
- **Largest libraries:** Iceland, Slovenia, and Cyprus provide the most content
- **Budget options:** Ad-supported tier available in select markets with significant savings

### For Market Analysis

- **Pricing strategy:** Netflix adjusts prices based on local purchasing power rather than content volume
- **Regional patterns:** Clear differences in pricing between developed and developing economies
- **Market positioning:** Premium pricing in wealthy nations vs aggressive pricing in emerging markets

### For Streaming Industry

- **Localization:** Subscription services must adapt pricing to local economic conditions
- **Value perception:** Content volume alone does not justify pricing—market factors dominate
- **Competitive response:** Ad-supported tiers address price-sensitive segments

## Historical Context

This analysis updates a previous study conducted in 2021 with 65 countries. The 2025 dataset reflects:

- Nearly **double the country coverage** (65 → 128 countries)
- Introduction of the **ad-supported tier**
- Evolved pricing strategies as Netflix matures in various markets
- Updated content licensing and availability patterns

The original 2021 analysis is preserved in `netflix-data-analysis-2021.ipynb` for comparison.

## Limitations

- Dataset represents a snapshot in time (2025)
- Pricing may not reflect promotional offers or regional discounts
- Content library sizes do not account for content type (movies vs TV shows)
- Quality metrics (HD, 4K availability) not included in analysis
- User engagement and satisfaction data not available

## Future Analysis Opportunities

Potential extensions of this research:

1. **Temporal analysis:** Track pricing changes over time across markets
2. **Content composition:** Analyze movies vs TV shows ratio by country
3. **Competitive comparison:** Include other streaming services (Disney+, Amazon Prime)
4. **User metrics:** Incorporate subscriber counts and engagement data
5. **Economic correlation:** Compare with GDP, purchasing power parity, broadband adoption

## Technologies Used

- **Python 3.9+** - Data analysis and processing
- **Pandas** - Data manipulation and aggregation
- **NumPy** - Numerical computations
- **Matplotlib & Seaborn** - Statistical visualizations
- **pycountry-convert** - Geographic classification
- **Jupyter Notebook** - Interactive analysis environment
- **HTML/CSS/JavaScript** - Interactive dashboard
- **Chart.js** - Dashboard visualizations

## Data Source

This analysis uses publicly available Netflix pricing data compiled from official Netflix websites across different countries. The dataset captures subscription tiers, pricing in USD, and content library sizes as of 2025.

**Original dataset source:** [Kaggle - Netflix Subscription Price Dataset](https://www.kaggle.com/datasets/netflix-subscription-prices)

## Contributing

This project serves as an educational resource for data analysis and visualization. Contributions are welcome:

- **Data updates:** Submit updated pricing or new country data
- **Analysis improvements:** Suggest additional metrics or visualizations
- **Code optimization:** Improve processing efficiency or visualization quality
- **Documentation:** Enhance explanations or add usage examples

## License

This project is available for educational and research purposes. Please attribute appropriately when using or referencing this analysis.

## Author

**Prian Kravichandar**

- GitHub: [@priankr](https://github.com/priankr)
- Original analysis: [Kaggle Notebook](https://www.kaggle.com/code/priankravichandar/netflix-subscriptions-data-analysis)

## Acknowledgments

- Dataset compiled from official Netflix regional websites
- Kaggle community for data sharing and feedback
- Open-source Python data science ecosystem

---

**Last Updated:** January 2025

For questions, suggestions, or collaboration opportunities, please open an issue in the GitHub repository.
