# Serverless URL Shortener – AWS Lambda + API Gateway + DynamoDB

## Project Overview
A fully serverless URL shortener service (like bit.ly) built on AWS. No EC2 instances to manage — everything runs on Lambda, API Gateway, and DynamoDB. Costs pennies per month and scales automatically.

**Why serverless:**
- No server management (no patching, no SSH)
- Pay only per request (free tier eligible)
- Scales from 0 to millions automatically
- Perfect for cloud portfolio projects

## Tech Stack (100% Serverless)
| Service | Purpose |
|---------|---------|
| AWS Lambda (Python) | Business logic (create short URL, redirect) |
| API Gateway (REST) | HTTP endpoint for the service |
| DynamoDB | Store URL mappings (short code → long URL) |
| S3 + CloudFront | Static frontend (optional) |
| IAM | Permissions between services |

## Architecture Diagram
> 📸 **[Screenshot 1: Serverless architecture diagram here]**
> *Shows: User → API Gateway → Lambda → DynamoDB flow, plus redirect flow.*

## How It Works
1. User submits a long URL via POST request
2. Lambda generates a short code (e.g., "abc123")
3. DynamoDB stores `{shortCode: "abc123", longUrl: "https://example.com/..."}`
4. User visits `https://your-api-id.execute-api.region.amazonaws.com/abc123`
5. Lambda looks up code and returns 302 redirect to original URL

## API Endpoints
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/shorten` | Create short URL (body: {"url": "https://..."}) |
| GET | `/{shortCode}` | Redirect to original URL |

## Deployment (Terraform)
I provisioned this entire stack using **Terraform** (Infrastructure as Code):

```hcl
# Main resources:
# - AWS Lambda function (Python 3.9)
# - API Gateway REST API
# - DynamoDB table (pay-per-request)
# - IAM role + policy for Lambda
