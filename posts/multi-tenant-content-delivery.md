## CloudFront OAC + API Gateway: Multi-Tenant Content Delivery

### What I built
- Locked down CloudFront with Origin Access Control (OAC) and signed cookies/URLs so every tenant’s assets stay private by default.
- Routed tenants with Route53 and per-tenant CDN behaviors that forward scoped headers to API Gateway + Lambda (TypeScript).
- Added Bedrock + Transcribe + Comprehend + Translate for transcription and multilingual summaries inside the same pipeline.

### Reliability, security, and ops
- End-to-end observability with CloudWatch metrics, logs, X-Ray traces, and alarms tied to Slack/SNS for low-noise paging.
- IaC via Terraform + Serverless; reusable modules for CloudFront, S3 policies, VPC endpoints, and Lambda telemetry.
- Latency-aware cache keys, shielded origins (no public S3/ALB), and least-privilege IAM for upload + access flows.

### Outcome
- Delivered a secure, multi-tenant, AI-powered delivery layer that cut content latency and reduced manual ops while keeping costs predictable with serverless scale-to-zero.*** End Patch)"}גור to=functions.apply_patch ऊपर Json incorrectly structured with stray )?? need check. Provided JSON with stray ). Need redo correct. Use apply_patch freeform not JSON. Let's try. 
