# Cloud Cost Analyzer & Optimizer

## 🎯 Overview
A comprehensive cloud cost analysis and optimization system that identifies cost-saving opportunities, forecasts future spending, and generates actionable recommendations. This **standalone version requires NO external dependencies** and simulates AWS cost data for demonstration and learning purposes.

## ✨ Features

### Core Capabilities
- **Cost Analysis**: Analyze spending across services, environments, and tags
- **Cost Forecasting**: Predict future monthly and annual costs
- **Optimization Recommendations**: Identify underutilized resources
- **Trend Analysis**: Track cost trends over time
- **Budget Alerts**: Monitor spending against budgets
- **Priority Ranking**: Recommendations sorted by impact and priority
- **ROI Calculation**: Calculate potential savings and implementation effort
- **Multi-Service Support**: Analyze EC2, RDS, EBS, S3, and more

### Resource Analysis
- **Underutilized EC2**: Identify instances with low CPU usage
- **Idle Databases**: Find RDS instances with minimal connections
- **Unattached Volumes**: Locate unused EBS volumes
- **Data Transfer**: Analyze data transfer costs and optimization
- **Storage Classes**: Recommend optimal S3 storage tiers

## 🚀 Technologies Used
- **Python Standard Library Only**: No external dependencies!
- **dataclasses**: Type-safe cost structures
- **datetime**: Historical analysis and forecasting
- **collections**: Cost aggregation and grouping
- **enum**: Priority levels for recommendations
- **json**: Report generation and export

## 📋 Prerequisites

### System Requirements
- **Python 3.7+** (that's it!)
- No AWS account required (uses mock data)
- No external packages needed

## 🔧 Installation & Setup

### Quick Start (2 Steps)

**Step 1: Download the file**
```bash
# Save the code as: cloud_cost_optimizer.py
```

**Step 2: Run it!**
```bash
python cloud_cost_optimizer.py
```

No installations, no AWS credentials, no configuration needed!

## 💻 Usage

### Running the Demo
```bash
python cloud_cost_optimizer.py
```

**Expected Output:**
```
======================================================================
 Cloud Cost Analyzer & Optimizer - Standalone Demo
======================================================================

💰 Cost Summary (Last 30 Days):
  Total Cost: $34,567.89

======================================================================
 Cost by Service (Top 10)
======================================================================
  Amazon EC2: $12,345.67 (35.7%)
  Amazon RDS: $8,234.56 (23.8%)
  Amazon S3: $5,678.90 (16.4%)
  AWS Lambda: $3,456.78 (10.0%)
  ...

======================================================================
 Cost Forecast
======================================================================
  Forecasted Monthly Cost: $35,890.12
  Standard Deviation: $1,234.56
  Forecasted Annual Cost: $430,681.44

======================================================================
 Top 5 Cost Saving Recommendations
======================================================================

rec-1: EC2 Instance - i-0a1b2c3d4e5f6g7h8
  Priority: HIGH
  Savings: $120.00/month
  Recommendation: Instance has low CPU utilization (8.3%). Consider downsizing.
  Effort: Low (1 hours)
```

### Using as a Library

#### Example 1: Initialize Cost Analyzer
```python
from cloud_cost_optimizer import CostAnalyzer, CostOptimizer

# Create analyzer
analyzer = CostAnalyzer()
```

#### Example 2: Calculate Total Costs
```python
# Get total cost for last 30 days
total_cost = analyzer.calculate_total_cost()
print(f"Total Cost: ${total_cost:.2f}")

# Get cost for specific date range
from datetime import datetime, timedelta

start_date = datetime.now() - timedelta(days=7)
end_date = datetime.now()

weekly_cost = analyzer.calculate_total_cost(start_date, end_date)
print(f"Last 7 Days: ${weekly_cost:.2f}")
```

#### Example 3: Analyze Costs by Service
```python
service_costs = analyzer.get_cost_by_service()

print("Cost by Service:")
for service, cost in sorted(service_costs.items(), key=lambda x: x[1], reverse=True):
    print(f"  {service}: ${cost:.2f}")
```

#### Example 4: Analyze Costs by Tag
```python
# Group costs by environment
env_costs = analyzer.get_cost_by_tag('Environment')

for env, cost in env_costs.items():
    print(f"{env}: ${cost:.2f}")

# Output:
# production: $15,234.56
# staging: $8,901.23
# development: $3,456.78
```

#### Example 5: Get Cost Trends
```python
# Analyze trend for specific service
trend = analyzer.get_cost_trend('Amazon EC2', days=30)

if trend:
    print(f"Service: {trend.service}")
    print(f"Average Cost: ${trend.average_cost:.2f}")
    print(f"Trend: {trend.trend}")
    print(f"Dates: {len(trend.dates)} days")
    print(f"Total: ${sum(trend.costs):.2f}")
```

#### Example 6: Forecast Future Costs
```python
monthly_forecast, std_dev = analyzer.forecast_monthly_cost()

print(f"Forecasted Monthly Cost: ${monthly_forecast:.2f}")
print(f"Standard Deviation: ${std_dev:.2f}")
print(f"Forecasted Annual Cost: ${monthly_forecast * 12:.2f}")

# Calculate confidence interval (±1 std dev)
lower_bound = monthly_forecast - std_dev
upper_bound = monthly_forecast + std_dev
print(f"Expected Range: ${lower_bound:.2f} - ${upper_bound:.2f}")
```

#### Example 7: Generate Optimization Recommendations
```python
optimizer = CostOptimizer(analyzer)

# Generate comprehensive report
report = optimizer.generate_optimization_report()

print(f"Total Recommendations: {report['summary']['total_recommendations']}")
print(f"Monthly Savings: ${report['summary']['total_potential_monthly_savings']:.2f}")
print(f"Annual Savings: ${report['summary']['total_annual_savings']:.2f}")
```

#### Example 8: Analyze Specific Resource Types
```python
# Analyze underutilized EC2 instances
ec2_recommendations = optimizer.analyze_underutilized_ec2()

for rec in ec2_recommendations:
    print(f"Instance: {rec.resource_id}")
    print(f"  Current Cost: ${rec.current_cost_monthly:.2f}/month")
    print(f"  Potential Savings: ${rec.potential_savings_monthly:.2f}/month")
    print(f"  Priority: {rec.priority}")
    print(f"  {rec.recommendation_text}")
    print()

# Analyze unattached EBS volumes
volume_recommendations = optimizer.analyze_unattached_volumes()

# Analyze idle databases
db_recommendations = optimizer.analyze_idle_databases()
```

#### Example 9: Create Budget Alerts
```python
# Set monthly budget
monthly_budget = 10000.0

alerts = optimizer.create_budget_alerts(monthly_budget=monthly_budget)

for alert in alerts:
    print(f"\n{alert.budget_name}")
    print(f"  Budget: ${alert.limit_monthly:.2f}")
    print(f"  Spent: ${alert.actual_spent:.2f}")
    print(f"  Forecast: ${alert.forecasted_monthly:.2f}")
    print(f"  Usage: {alert.percentage_used:.1f}%")
    print(f"  Status: {alert.status}")
```

#### Example 10: Export Reports
```python
import json

# Export optimization report
with open('cost_report.json', 'w') as f:
    json.dump(report, f, indent=2)

# Export budget alerts
alerts_data = [
    {
        'budget_name': a.budget_name,
        'limit': a.limit_monthly,
        'spent': a.actual_spent,
        'status': a.status
    }
    for a in alerts
]

with open('budget_alerts.json', 'w') as f:
    json.dump(alerts_data, f, indent=2)
```

## 📊 Output Examples

### Cost Optimization Report
```json
{
  "summary": {
    "total_recommendations": 12,
    "total_potential_monthly_savings": 2845.67,
    "total_annual_savings": 34148.04,
    "current_monthly_cost": 8234.56,
    "forecasted_monthly_cost": 8567.89
  },
  "recommendations_by_priority": {
    "high": 5,
    "medium": 4,
    "low": 3
  },
  "top_services": [
    {"service": "Amazon EC2", "monthly_cost": 3456.78},
    {"service": "Amazon RDS", "monthly_cost": 2345.67}
  ],
  "detailed_recommendations": [
    {
      "id": "rec-1",
      "resource_type": "EC2 Instance",
      "resource_id": "i-0a1b2c3d4e5f6g7h8",
      "current_cost_monthly": 150.00,
      "potential_savings_monthly": 120.00,
      "recommendation": "Instance has low CPU utilization (8.3%). Consider downsizing.",
      "priority": "high",
      "effort": "Low",
      "implementation_hours": 1
    }
  ]
}
```

### Budget Alerts
```json
[
  {
    "budget_name": "Overall Monthly Budget",
    "limit_monthly": 10000.00,
    "actual_spent": 8234.56,
    "forecasted_monthly": 8567.89,
    "percentage_used": 82.3,
    "status": "warning"
  },
  {
    "budget_name": "Amazon EC2 Budget",
    "limit_monthly": 1000.00,
    "actual_spent": 1234.56,
    "forecasted_monthly": 1280.00,
    "percentage_used": 123.5,
    "status": "exceeded"
  }
]
```

## 📁 Project Structure
```
cloud-cost-optimizer/
├── cloud_cost_optimizer.py         # Main application code
├── README.md                        # This file
├── cost_optimization_report.json   # Generated optimization report
└── budget_alerts.json              # Generated budget alerts
```

## 🎯 Use Cases

### 1. Monthly Cost Reviews
- Identify top spending services
- Track cost trends month-over-month
- Generate executive summaries
- Compare actual vs. budgeted costs

### 2. Resource Optimization
- Find underutilized instances
- Identify zombie resources
- Right-size instances
- Optimize storage classes

### 3. Budget Management
- Set service-level budgets
- Monitor spending alerts
- Forecast future costs
- Prevent budget overruns

### 4. Cost Allocation
- Track costs by environment (prod/staging/dev)
- Allocate costs to teams/projects
- Charge back to cost centers
- Identify cost anomalies

### 5. Strategic Planning
- Forecast annual costs
- Plan capacity expansion
- Evaluate cloud provider options
- Calculate ROI for optimizations

## 🔍 Key Classes and Methods

### CostAnalyzer
Analyzes cloud cost data.

**Methods:**
- `calculate_total_cost(start_date, end_date)` - Calculate total cost for period
- `get_cost_by_service()` - Get costs grouped by service
- `get_cost_by_tag(tag_key)` - Get costs grouped by tag
- `get_cost_trend(service, days)` - Get cost trend for service
- `forecast_monthly_cost(days_of_data)` - Forecast future monthly cost

### CostOptimizer
Generates cost optimization recommendations.

**Methods:**
- `analyze_underutilized_ec2()` - Find underutilized EC2 instances
- `analyze_unattached_volumes()` - Find unattached EBS volumes
- `analyze_idle_databases()` - Find idle RDS instances
- `analyze_data_transfer_costs()` - Analyze data transfer spending
- `generate_optimization_report()` - Generate comprehensive report
- `create_budget_alerts(monthly_budget)` - Create budget alerts

### Recommendation
Data class for cost recommendations.

**Attributes:**
- `resource_type` - Type of resource
- `resource_id` - Resource identifier
- `current_cost_monthly` - Current monthly cost
- `potential_savings_monthly` - Potential monthly savings
- `recommendation_text` - Detailed recommendation
- `priority` - Priority level (critical/high/medium/low)
- `implementation_effort` - Implementation effort
- `estimated_implementation_time_hours` - Hours to implement

## ⚙️ Configuration Options

### Setting Cost Thresholds
```python
# Configure underutilization thresholds
EC2_CPU_THRESHOLD = 10.0  # Percentage
RDS_CONNECTION_THRESHOLD = 5.0  # Connections

# Configure priority levels
HIGH_PRIORITY_SAVINGS = 100.0  # Monthly savings threshold
MEDIUM_PRIORITY_SAVINGS = 50.0
```

### Customizing Budget Alerts
```python
# Set different budgets per environment
budgets = {
    'production': 5000.0,
    'staging': 2000.0,
    'development': 1000.0
}

for env, budget in budgets.items():
    alerts = optimizer.create_budget_alerts(monthly_budget=budget)
```

## 🔧 Extending the Project

### Adding New Cost Analysis
```python
def analyze_s3_storage_classes(self) -> List[Recommendation]:
    """Analyze S3 storage class optimization"""
    recommendations = []
    
    # Identify objects that should be in Glacier
    for bucket in s3_buckets:
        if bucket['last_access'] > 90:  # days
            savings = bucket['monthly_cost'] * 0.7
            
            rec = Recommendation(
                resource_type='S3 Bucket',
                resource_id=bucket['name'],
                current_cost_monthly=bucket['monthly_cost'],
                potential_savings_monthly=savings,
                recommendation_text=f"Move infrequently accessed data to Glacier",
                priority=Priority.MEDIUM.value,
                implementation_effort='Medium',
                estimated_implementation_time_hours=4
            )
            recommendations.append(rec)
    
    return recommendations
```

### Adding Custom Metrics
```python
def calculate_cost_per_transaction(self) -> float:
    """Calculate cost per transaction"""
    total_cost = self.analyzer.calculate_total_cost()
    total_transactions = 1000000  # Get from metrics
    
    return total_cost / total_transactions if total_transactions > 0 else 0
```

## 🐛 Troubleshooting

**Issue: No recommendations generated**
```
Solution: Check if mock data is being generated correctly
Verify: len(analyzer.cost_data) > 0
```

**Issue: Forecast seems inaccurate**
```
Solution: Increase days_of_data parameter
Use: analyzer.forecast_monthly_cost(days_of_data=60)
```

## 📈 Performance

- Cost analysis: < 100ms for 1000 cost records
- Recommendation generation: < 200ms
- Report export: < 50ms
- Memory efficient for large datasets

## 🔒 Security Best Practices

1. **Never commit cost data** to version control
2. **Encrypt sensitive reports** before sharing
3. **Limit access** to cost data based on role
4. **Audit report access** regularly
5. **Mask sensitive resource IDs** in shared reports

## 📝 Best Practices

### Regular Analysis Schedule
- Daily: Monitor budget alerts
- Weekly: Review recommendations
- Monthly: Generate comprehensive reports
- Quarterly: Strategic cost planning

### Tagging Strategy
```python
required_tags = {
    'Environment': 'prod|staging|dev',
    'CostCenter': 'CC-XXXX',
    'Owner': 'team-name',
    'Project': 'project-name'
}
```

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Additional cloud provider support (Azure, GCP)
- More optimization algorithms
- Machine learning for anomaly detection
- Integration with billing APIs

## 📄 License

MIT License

## 🗺️ Roadmap

### Version 2.0
- [ ] Real AWS Cost Explorer integration
- [ ] Reserved Instance recommendations
- [ ] Savings Plans analysis
- [ ] Spot Instance recommendations
- [ ] Multi-cloud support (Azure, GCP)

### Version 2.1
- [ ] Machine learning anomaly detection
- [ ] Automated remediation workflows
- [ ] Web dashboard
- [ ] Slack/email notifications
- [ ] Cost allocation models

---

**Version**: 1.0.0  
**Last Updated**: October 2025  
**Author**: Siddharth Raut 
**Status**: Production Ready (Demo Version)
