# Architecture 1: Post-Purchase Order Modification Framework

This document outlines the state-machine transition flow governing an automated customer request to modify an active order. It maps out user intent recognition, backend CRM validations, and conditional branching to eliminate LLM hallucinations.

## 🔄 State Transition Mapping

```mermaid
graph TD
    A[Start: User Greeting/Intent] --> B{Intent Classifier: Dialogueflow CX}
    B -->|Intent: Modify Order| C[State: Await Order Credentials]
    B -->|Intent: General FAQ| D[State: Dynamic GenAI Grounding]
    
    C --> E[User Provides: Order ID & Email]
    E --> F[Webhook Trigger: Query Shopify Backend]
    
    F --> G{Conditional Loop: Order Shipped?}
    G -->|Yes| H[State: Deny Modification / Offer Return Policy]
    G -->|No| I[State: Execute Field Amendment API]
    
    I --> J[Webhook Trigger: Push Update to CRM]
    J --> K[End: Confirm Modification via Rich Media/Email]
