# Healthcare Agent Demo

## Inspiration
This project was born from seeing how overwhelmed care teams can be with streams of patient vitals. I wanted to simplify: ingest, triage, summarize — with full observability and error safety. I aimed to build something reusable, modular, and demonstrable using Google Cloud.

## What it does
- Receives patient vitals + symptoms via the **Orchestrator** service  
- Publishes to `vitals.events` → **triage** service  
- Triage emits to `summary.notes` → **summarization** service  
- Uses **Pub/Sub push subscriptions** with **OIDC authentication**  
- Utilizes **dead-letter queues (DLQs)** to prevent message loss  
- Integrates **Logging & Monitoring** with alerts (DLQ > 0, Cloud Run 5xx)  

## How I built it
1. Defined topics & subscriptions (Pub/Sub) with push → Cloud Run  
2. Configured IAM: service accounts, invoker roles, token creator  
3. Built Flask services for orchestrator, triage, summarization  
4. Tested end-to-end flow via Console UI (Publish message → Logs)  
5. Added DLQs and alert policies for robustness  

## Challenges
- Aligning audience strings for OIDC tokens  
- Debugging subscription authentication failures  
- Configuring monitoring alerts for asynchronous flows  

## Technologies
- **Google Cloud Run**, **Cloud Pub/Sub**, **Cloud Logging/Monitoring**  
- **Python / Flask** services  
- **gcloud CLI ** scripts for infra setup  
- **GitHub** for version control & submission  

## Architecture
![Architecture](diagrams/architecture.png)

## Demo Instructions
1. In GCP Console → Pub/Sub → Topics → `vitals.events` → *Publish message*
   ```json
   {
     "patient_id": "ptest",
     "vitals": { "hr": 120, "spo2": 97, "temp_c": 37.0 },
     "symptoms": ["dizziness"]
   }
