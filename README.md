# Healthcare Financial Data Generator

A comprehensive Python tool for generating realistic healthcare financial data including capitation payments, claims data, and provider financial metrics. Perfect for healthcare analytics, financial modeling, testing, and educational purposes.

## 🎯 Overview

This tool generates a complete healthcare financial ecosystem with:
- **Patient Demographics**: Realistic patient profiles with age, sex, and health plan assignments
- **Capitation Data**: Monthly risk-adjusted payments based on RAF scores
- **Claims Data**: Fee-for-service claims with realistic diagnosis-procedure pairs
- **Provider Analytics**: Financial performance metrics and revenue analysis

## 🚀 Features

### 📊 Realistic Data Generation
- **Clinically Matched Codes**: ICD-10 diagnoses paired with appropriate CPT procedures
- **Risk Adjustment**: RAF scores that increase with age and medical complexity
- **Payer Mix**: Multiple health plans with different reimbursement rates
- **Financial Logic**: Realistic copays, allowable amounts, and payment calculations

### 🏥 Provider Network Simulation
- **Multi-clinic Environment**: 4 clinics with 6 providers each
- **Patient Panels**: 150 patients per provider with monthly variations (±3%)
- **Revenue Streams**: Both capitation and fee-for-service modeling
- **Performance Metrics**: Provider-level financial analysis

### 💰 Healthcare Economics
- **Capitation Payments**: Monthly PMPM with risk adjustment
- **Claims Processing**: Realistic claim statuses and payment logic
- **Self-Pay Patients**: Office-only restrictions and different payment patterns
- **Revenue Cycle**: Complete financial workflow simulation

## 📁 Output Files

The generator creates four comprehensive datasets:

| File | Description | Records |
|------|-------------|---------|
| `healthcare_capitation_data.csv` | Monthly capitation payments | ~40,000+ |
| `healthcare_claims_data.csv` | Individual claims with diagnoses | ~15,000+ |
| `healthcare_patients_registry.csv` | Master patient demographics | 3,600 |
| `provider_financial_summary.csv` | Provider performance metrics | 24 |

## 🛠️ Installation

### Requirements
```bash
pip install pandas numpy faker
```

### For Databricks
```python
%pip install pandas faker
```

## 💻 Usage

### Basic Usage
```python
# Configuration
clinics = ['North Clinic', 'South Clinic', 'East Clinic', 'West Clinic']
providers_per_clinic = 6
patients_per_provider = 150
months_back = 12

# Generate data
df_capitation, df_claims, df_patients = generate_combined_healthcare_data(
    clinics, providers_per_clinic, patients_per_provider, months_back
)

# Generate financial summary
summary = generate_financial_summary(df_capitation, df_claims, df_patients)
```

### Customization Options
- **Scale**: Adjust `patients_per_provider` (50-500 recommended)
- **Time Period**: Modify `months_back` (6-24 months)
- **Clinics**: Add/remove clinic names
- **Health Plans**: Customize payer mix and reimbursement rates

## 📋 Data Schema

### Capitation Data
| Field | Type | Description |
|-------|------|-------------|
| PatientID | String | Unique patient identifier (PAT000001) |
| PatientName | String | Realistic patient name (Faker generated) |
| HealthPlan | String | Insurance plan (Universal Health, BlueDot Health, etc.) |
| Provider | String | Assigned provider name |
| MonthYear | String | Payment month (YYYY-MM) |
| RiskScore_RAF | Decimal | Risk Adjustment Factor (0.5-3.5) |
| CapitationAmount | Decimal | Monthly payment amount |

### Claims Data
| Field | Type | Description |
|-------|------|-------------|
| ClaimID | String | Unique claim identifier (CLM00000001) |
| PatientID | String | Links to patient registry |
| PrimaryDiagnosis | String | ICD-10 diagnosis code |
| PrimaryProcedure | String | CPT procedure code |
| PlaceOfService | String | Service location code |
| ChargeAmount | Decimal | Provider charges |
| PaidAmount | Decimal | Insurance payment |
| ClaimStatus | String | Paid/Denied/Pending/Partially Paid |

### Patient Registry
| Field | Type | Description |
|-------|------|-------------|
| patient_id_str | String | Unique identifier |
| patient_name | String | Full name |
| age | Integer | Patient age (18-85) |
| sex | String | M/F |
| health_plan | String | Insurance plan |
| base_raf | Decimal | Baseline risk score (rounded to 2 decimals) |
| base_cap | Decimal | Base capitation rate (rounded to 2 decimals) |

## 🎨 Use Cases

### Healthcare Analytics
- **Revenue Cycle Analysis**: Track collection rates and payment patterns
- **Provider Performance**: Compare productivity and financial metrics
- **Payer Analysis**: Understand reimbursement by health plan
- **Risk Adjustment**: Model RAF score impact on payments

### Financial Modeling
- **Budget Planning**: Forecast revenue based on patient mix
- **Contract Negotiation**: Model different reimbursement scenarios
- **Cost Analysis**: Understand revenue per patient/visit
- **Profitability**: Calculate margins by service line

### Testing & Development
- **Healthcare Software**: Test claims processing systems
- **Analytics Platforms**: Validate healthcare dashboards
- **Machine Learning**: Train models on realistic data
- **Education**: Teach healthcare finance concepts

## 📊 Sample Analytics

### Provider Revenue Analysis
```python
# Top performing providers
top_providers = summary['provider_summary'].nlargest(5, 'TotalRevenue')

# Revenue mix analysis
capitation_pct = summary['total_capitation_revenue'] / summary['total_revenue'] * 100
ffs_pct = summary['total_claims_paid'] / summary['total_revenue'] * 100

# Collection rate
collection_rate = summary['total_claims_paid'] / summary['total_claims_charges'] * 100
```

### Patient Analytics
```python
# Risk score distribution
df_patients['base_raf'].describe()

# Age demographics
df_patients.groupby('age').size().plot(kind='hist')

# Payer mix
df_patients['health_plan'].value_counts()
```

## 🔧 Advanced Configuration

### Custom Health Plans
```python
health_plan_products = {
    'Custom Plan': {
        'CUSTOM-BASIC': {'copay': 15, 'weight': 80},
        'CUSTOM-PREMIUM': {'copay': 35, 'weight': 20}
    }
}
```

### Custom Diagnosis-Procedure Pairs
```python
custom_dx_proc = {
    'Z00.00': ['99213', '99214', '99396'],  # Annual exam
    'I10': ['99213', '99214', '93000'],     # Hypertension
}
```

## 📈 Financial Metrics

The generator produces realistic healthcare financial patterns:

- **Capitation Revenue**: $400-600 PMPM average
- **Claims Volume**: 3-5 claims per patient annually
- **Collection Rate**: 70-80% of charges
- **Revenue Mix**: 60% Capitation, 40% Fee-for-Service
- **RAF Scores**: 0.5-3.5 range (higher for older patients)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/enhancement`)
3. Commit changes (`git commit -am 'Add new feature'`)
4. Push to branch (`git push origin feature/enhancement`)
5. Create a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Jason Votaw** - *Healthcare Data Architect*
- Specializes in healthcare analytics and financial modeling
- Focus on realistic data generation for healthcare systems

## 🆘 Support

For questions or issues:
1. Check existing [Issues](../../issues)
2. Create a new issue with detailed description
3. Include sample code and error messages

## 🏷️ Version History

- **v1.0.0** - Initial release with combined capitation and claims generation
- **v1.1.0** - Added provider financial summaries and analytics
- **v1.2.0** - Enhanced RAF scoring and risk adjustment logic

## 🔗 Related Projects

- [Healthcare Quality Metrics Generator](../quality-metrics) - Provider-level quality data
- [MMR Flat File Parser](../mmr-parser) - Medicare payment file processing
- [Healthcare Claims Analyzer](../claims-analytics) - Advanced claims analytics

---

*Built for healthcare professionals, data scientists, and developers working with healthcare financial data.*
