graph TD

subgraph Frontend
    A1[React/Next.js Dashboard]
end

subgraph Gateway Service (Go)
    B1[Rate Limiting (Redis)]
    B2[JWT/Auth Verification]
    B3[Quarantine Triggers]
    B4[API Key Middleware]
end

subgraph Vault Service (Go)
    C1[API Key Generation]
    C2[Encryption via AWS KMS]
    C3[Token Rotation Engine]
    C4[RLS Token/Key DB Access]
end

subgraph AI Service (Python)
    D1[Log Embedding Generator]
    D2[GPT Summary Generator]
    D3[Risk Classification]
end

subgraph Data Layer
    E1[(PostgreSQL - RDS)]
    E2[(Redis - Elasticache)]
    E3[(S3 Bucket Storage)]
    E4[(KMS - Key Encryption)]
end

subgraph AWS Infra
    F1[EC2 / Fargate]
    F2[CloudWatch Logs]
    F3[IAM Scoped Roles]
end

A1 -->|REST API| B1
B1 --> B2 --> B3 --> B4 --> E1
B4 --> C1
C1 --> C2 --> C3 --> E1
C1 --> E4

B3 --> D1 --> D2 --> D3 --> E3
D1 --> E2
D2 --> E3

A1 -->|GET Logs| E3
A1 -->|GET Reports| E1

Gateway Service --> F1
Vault Service --> F1
AI Service --> F1
E1 --> F2
E3 --> F2
E4 --> F3
