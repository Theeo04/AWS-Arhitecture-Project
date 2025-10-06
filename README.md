# Hybrid AWS and on-premise Kubernetes Arhitecture Project
Hybrid application on AWS and on-premise Kubernetes Cluster implementing multiple services

## Introduction
Scopul proiectului este crearea unei platforme simple care permite utilizatorilor să încarce fișiere CSV (de exemplu, date de la senzori) și să vizualizeze rezultatele procesării într-o interfață web.

## Arhitectura sistemului
![descriere imagine](diagrama-v1.png)

### Componente AWS
- Route 53 – gestionarea domeniilor și rutarea traficului.
- Application Load Balancer (ALB) – distribuirea traficului către instanțele EC2.
- EC2 + Docker – găzduirea containerelor cu aplicația web (frontend și backend API).
- Cognito – autentificare utilizatori.
- S3 – stocare fișiere CSV brute și rezultate procesate.
- SQS – coadă pentru gestionarea joburilor de procesare.
- Lambda – functii serverless care primesc rezultatele de la worker si le prelucreaza.
- SNS – trimiterea notificărilor către utilizatori.
- CloudWatch / Managed Prometheus & Grafana – monitorizare, loguri și dashboard-uri.

### Componente on-prem (cluster K3s) creat din 3 VM-uri Local
- Worker Deployment – procesează fișierele CSV primite din SQS.
- External Secrets Operator – sincronizează secretele din AWS Secrets Manager.
- Prometheus agent + CloudWatch agent – colectează metrici și loguri locale.
- Conectivitate securizată prin VPN (sau WireGuard) către VPC-ul AWS.

## Tehnologii și instrumente folosite
- Containerizare & orchestrare: Docker, K3s.
- Cloud AWS: EC2, ALB, S3, Cognito, SQS, RDS/Aurora, SNS, CloudWatch.
- Infra-as-Code: Terraform (pentru AWS) și Ansible (pentru setup on-prem).
- CI/CD: GitHub/GitLab Actions – build imagini Docker (ECR) + deploy automat (Helm/Terraform).
- Observabilitate: Prometheus + Grafana, CloudWatch.
- Securitate: TLS, IAM, External Secrets, WAF pe ALB.

## Nota:

 - Proiectul poate suferi modificari pe parcursul dezvoltarii.
 - Pentru procesarea fisierelor CSV se poate integra un modul suplimentar de inteligenta artificiala, fie printr-un model LLM, fie utilizand bibliotecile oferite de fastai.

## Cursuri:
 - https://1and1.udemy.com/course/aws-certified-cloud-practitioner-new/learn/ 
 - https://www.fast.ai/ 
 - https://www.udemy.com/course/ultimate-aws-certified-sysops-administrator-associate/?couponCode=2021PM20#instructor-1