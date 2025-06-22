AWS Cost Optimization and Governance Initiative

## Overview

This project implements a comprehensive AWS cost optimization and governance framework designed to help organizations reduce cloud spending while maintaining operational excellence. The initiative combines automated cost monitoring, resource optimization, and governance policies to ensure efficient cloud resource utilization.

## Project Structure

```
aws-cost-optimization/
├── terraform/
│   ├── modules/
│   │   ├── cost-monitoring/
│   │   ├── resource-tagging/
│   │   ├── budgets-alerts/
│   │   └── governance-policies/
│   ├── environments/
│   │   ├── dev/
│   │   ├── staging/
│   │   └── prod/
│   └── main.tf
├── scripts/
│   ├── cost-analysis/
│   ├── resource-cleanup/
│   └── optimization-reports/
├── lambda-functions/
│   ├── cost-monitor/
│   ├── resource-scheduler/
│   └── unused-resource-detector/
├── policies/
│   ├── iam-policies/
│   ├── service-control-policies/
│   └── cost-allocation-tags/
├── monitoring/
│   ├── cloudwatch-dashboards/
│   ├── cost-anomaly-detection/
│   └── budget-alerts/
├── documentation/
│   ├── architecture/
│   ├── runbooks/
│   └── best-practices/
├── ci-cd/
│   ├── github-actions/
│   ├── jenkins/
│   └── gitlab-ci/
└── README.md
```

## Key Features

### 1. Automated Cost Monitoring
- Real-time cost tracking and alerting
- Custom CloudWatch dashboards for cost visualization
- Automated anomaly detection for unusual spending patterns
- Daily, weekly, and monthly cost reports

### 2. Resource Optimization
- Automated identification of underutilized resources
- EC2 instance right-sizing recommendations
- EBS volume optimization and cleanup
- RDS instance optimization
- S3 storage class recommendations

### 3. Governance and Compliance
- Mandatory resource tagging policies
- Service Control Policies (SCPs) implementation
- Cost allocation and chargeback mechanisms
- Automated compliance reporting

### 4. Automated Resource Management
- Scheduled start/stop of non-production resources
- Automated cleanup of unused resources
- Lifecycle management for temporary resources
- Reserved Instance optimization

## Quick Start

### Prerequisites

- AWS CLI configured with appropriate permissions
- Terraform >= 1.0
- Python 3.8+
- Docker (for Lambda functions)
- Git

### Installation

1. Clone the repository:
```bash
git clone https://github.com/your-org/aws-cost-optimization.git
cd aws-cost-optimization
```

2. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your specific configurations
```

3. Initialize Terraform:
```bash
cd terraform
terraform init
```

4. Deploy the infrastructure:
```bash
terraform plan -var-file="environments/dev/terraform.tfvars"
terraform apply -var-file="environments/dev/terraform.tfvars"
```

5. Deploy Lambda functions:
```bash
cd ../lambda-functions
./deploy.sh
```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `AWS_REGION` | Primary AWS region | `us-east-1` |
| `COST_THRESHOLD` | Monthly cost alert threshold | `1000` |
| `NOTIFICATION_EMAIL` | Email for cost alerts | Required |
| `ENVIRONMENT` | Environment name (dev/staging/prod) | `dev` |
| `TAGGING_ENABLED` | Enable mandatory tagging | `true` |

### Tagging Strategy

All resources must include the following tags:
- `Environment` (dev/staging/prod)
- `Project` (project identifier)
- `Owner` (team or individual responsible)
- `CostCenter` (for chargeback)
- `ExpirationDate` (for temporary resources)

## Core Components

### 1. Cost Monitoring Module

**Location:** `terraform/modules/cost-monitoring/`

Features:
- CloudWatch cost metrics collection
- Custom cost dashboards
- Automated cost anomaly detection
- Integration with AWS Cost Explorer API

### 2. Budget and Alerts Module

**Location:** `terraform/modules/budgets-alerts/`

Features:
- Configurable budget thresholds
- Multi-channel alerting (email, Slack, SNS)
- Forecasting and trend analysis
- Department-level budget allocation

### 3. Resource Scheduler

**Location:** `lambda-functions/resource-scheduler/`

Features:
- Automated EC2 instance scheduling
- RDS instance start/stop automation
- Auto Scaling Group scheduling
- Custom scheduling rules per environment

### 4. Unused Resource Detector

**Location:** `lambda-functions/unused-resource-detector/`

Features:
- Identifies idle EC2 instances
- Detects unattached EBS volumes
- Finds unused Elastic IPs
- Reports on underutilized RDS instances

## Monitoring and Dashboards

### CloudWatch Dashboards

1. **Cost Overview Dashboard**
   - Monthly spend trends
   - Service-wise cost breakdown
   - Budget vs actual spending

2. **Resource Utilization Dashboard**
   - EC2 CPU and memory utilization
   - EBS IOPS and throughput
   - RDS connections and performance

3. **Governance Dashboard**
   - Tagging compliance metrics
   - Policy violation tracking
   - Resource lifecycle status

### Cost Anomaly Detection

Automated detection of:
- Unusual spending spikes
- New services usage
- Region-based cost anomalies
- Account-level spending patterns

## Automation Scripts

### Daily Cost Analysis
```bash
./scripts/cost-analysis/daily-report.py
```
Generates daily cost reports and identifies optimization opportunities.

### Resource Cleanup
```bash
./scripts/resource-cleanup/unused-resources.py
```
Safely removes unused resources based on defined criteria.

### Right-sizing Recommendations
```bash
./scripts/optimization-reports/rightsizing.py
```
Provides EC2 and RDS right-sizing recommendations.

## Governance Policies

### Service Control Policies (SCPs)

1. **Cost Control SCP**
   - Prevents launch of expensive instance types
   - Restricts certain AWS services
   - Enforces region restrictions

2. **Tagging Enforcement SCP**
   - Requires mandatory tags on resource creation
   - Prevents untagged resource launch
   - Enforces tag value formats

### IAM Policies

1. **Cost Optimization Role**
   - Read access to cost and billing APIs
   - Limited resource modification permissions
   - CloudWatch and Lambda execution rights

2. **Developer Restrictions**
   - Prevents costly resource creation
   - Limits instance types and sizes
   - Requires approval for Reserved Instances

## CI/CD Pipeline

### GitHub Actions Workflow

```yaml
name: Cost Optimization Deployment
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
      - name: Terraform Plan
        run: terraform plan
      - name: Terraform Apply
        if: github.ref == 'refs/heads/main'
        run: terraform apply -auto-approve
```

### Deployment Strategy

1. **Development Environment**
   - Automated deployment on feature branch merge
   - Cost simulation and testing
   - Policy validation

2. **Staging Environment**
   - Manual approval required
   - Full integration testing
   - Performance impact assessment

3. **Production Environment**
   - Change management process
   - Rollback procedures
   - Monitoring and alerting validation

## Best Practices

### Cost Optimization
- Implement resource scheduling for non-production environments
- Use Spot Instances for fault-tolerant workloads
- Regular right-sizing reviews
- Implement S3 lifecycle policies
- Monitor and optimize data transfer costs

### Governance
- Enforce consistent tagging across all resources
- Implement approval workflows for high-cost resources
- Regular policy compliance audits
- Cross-functional cost reviews
- Automated policy enforcement

### Monitoring
- Set up proactive cost alerts
- Monitor cost trends and patterns
- Regular cost allocation reviews
- Track optimization initiative ROI
- Continuous improvement processes

## Troubleshooting

### Common Issues

1. **Lambda Function Timeouts**
   - Increase timeout values in `terraform/modules/cost-monitoring/lambda.tf`
   - Optimize function code for performance

2. **Missing Cost Data**
   - Verify Cost Explorer API permissions
   - Check CloudWatch Logs for errors
   - Ensure proper IAM roles are attached

3. **Tag Compliance Issues**
   - Review SCP configurations
   - Validate tag policies
   - Check resource creation logs

### Debug Commands

```bash
# Check Terraform state
terraform show

# Validate Lambda function logs
aws logs tail /aws/lambda/cost-monitor --follow

# Test cost analysis script
python scripts/cost-analysis/daily-report.py --debug
```

## Security Considerations

- All Lambda functions use least-privilege IAM roles
- Sensitive data encrypted in transit and at rest
- Cost data access restricted to authorized personnel
- Regular security audits of cost management components
- Secure handling of cost reports and notifications

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow Terraform best practices
- Include unit tests for Lambda functions
- Update documentation for new features
- Test in development environment first
- Follow semantic versioning

## Cost Savings Metrics

Track the following KPIs:
- Monthly cost reduction percentage
- Resource utilization improvement
- Unused resource elimination
- Right-sizing success rate
- Policy compliance percentage

## Support and Maintenance

### Regular Tasks
- Weekly cost review meetings
- Monthly optimization reports
- Quarterly policy updates
- Annual strategy reviews

### Contact Information
- **Project Lead:** Example  - devops.engineer@example.com
- **DevOps Team:** [devops@example.com]
- **Slack Channel:** #aws-cost-optimization

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Changelog

### v1.0.0 (2024-01-01)
- Initial release
- Basic cost monitoring and alerting
- Resource scheduling functionality
- Governance policy implementation

### v1.1.0 (2024-02-01)
- Added anomaly detection
- Enhanced dashboards
- Improved resource cleanup automation
- Additional governance policies

---

**Note:** This project is continuously evolving. Please check the [CHANGELOG.md](CHANGELOG.md) for the latest updates and features.
