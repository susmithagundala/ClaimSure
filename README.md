ClaimSure -- Insurance Claim Fraud Triage & Evidence Ledger
ClaimSure is a full-stack web application for insurance claim fraud
triage and evidence management. It combines explainable, rule-based
fraud screening with investigator-led review, evidence management, and
analytical reporting.

A fraud flag is an indication for further review, not proof of fraud.
Claims that trigger configured fraud rules are added to the investigator
review queue; claims that are not flagged continue through normal claim
processing and do not enter that manual-review queue. Investigators
review the flagged claims and supporting evidence before recording the
final decision.

Key Features
- Claim submission and claim-status tracking for claimants.
- Entity matching and duplicate-claim detection.
- Configurable, explainable fraud-screening rules.
- Evidence metadata management and supporting-file storage.
- Investigator review queue, investigation notes, final decisions, and
  applicable override reasons.
- Audit and investigation history.
- Analytical reporting through Python/Pandas, Snowflake, and Power BI.
  
Claim Processing Workflow
1. A claimant submits an insurance claim through the React frontend.
2. The Spring Boot backend validates the submission and associates
   relevant customer, policy, and vehicle records.
3. Claim details are stored in PostgreSQL. Supporting evidence files
   are stored in Amazon S3, while evidence metadata is maintained in
   PostgreSQL.
4. The system performs entity matching, duplicate detection, and
   rule-based fraud screening.
5. Claims that trigger configured fraud rules are flagged and added to
   the investigator review queue.
6. Claims that are not flagged continue through normal claim processing
   and do not enter the manual-review queue.
7. Investigators review flagged claims and evidence, record findings
   and notes, and make the final decision.
8. Decision records, applicable override reasons, and investigation
   history are maintained for auditability.
9. Operational and investigation data can be processed through the
   manually executed Python/Pandas ETL pipeline, loaded into Snowflake,
   and visualized through Power BI.
   
Technology Stack
  Area                      Technologies
  Backend                   Java 21, Spring Boot 4.0.9
Frontend                  React.js, JavaScript, Vite, Bootstrap, React Router
Security                  Spring Security, JWT
Operational database      PostgreSQL, Flyway, Hibernate
Database administration   pgAdmin 4
Evidence storage          Amazon S3, AWS IAM
Data processing           Python, Pandas, SQLAlchemy
Data warehouse            Snowflake
Dashboards                Microsoft Power BI
Containerization          Docker, Docker Compose
Cloud hosting             AWS EC2 (Ubuntu), Nginx
CI/CD                     GitHub Actions, AWS OIDC, AWS Systems Manager

Architecture Overview
- React frontend: Provides separate claimant and investigator
  workspaces.
- Spring Boot backend: Handles authentication, claim processing,
  fraud screening, investigation management, and integrations.
- PostgreSQL: Stores operational records such as users, customers,
  policies, claims, evidence metadata, fraud flags, investigations,
  decisions, and audit records.
- Amazon S3: Stores supporting evidence files associated with
  claims.
- Python/Pandas ETL: Extracts operational data from PostgreSQL,
  transforms it for analytics, and loads it into Snowflake. The ETL
  process is executed manually.
- Snowflake and Power BI: Provide analytical storage, queries, and
  dashboards.
  
Power BI Dashboards
- Customer Analytics: Customer and claim metrics, repeat
  claimants, customer risk scores, and monthly claim trends.
- Executive Overview: Total claims, fraud flags, entity matches,
  open investigations, risk-band distribution, and claim trends.
- Fraud Risk Analytics: Configured fraud rules, fraud flags by
  rule, risk-band analysis, average risk scores, and claim status.
  
Deployment and CI/CD
ClaimSure is containerized using Docker and Docker Compose and deployed
to an Ubuntu-based AWS EC2 instance in the Asia Pacific (Mumbai) region.
The frontend is served through Nginx, while the Spring Boot backend and
PostgreSQL communicate through the internal Docker network.
The EC2 instance uses an IAM role for required AWS access; AWS access
keys are not stored in the application configuration. GitHub Actions
supports continuous integration through backend build and test, frontend
build, and ETL Docker image build workflows. Continuous deployment uses
GitHub Actions OpenID Connect (OIDC) authentication with AWS IAM and AWS
Systems Manager Run Command. The ETL image is built in CI, but the ETL
pipeline is not automatically executed during deployment.
