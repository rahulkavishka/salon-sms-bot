# Salon SMS Bot V4.1

A high-performance, AI-driven SMS chatbot for hair salons built on **n8n**. This bot automates the entire customer journey—from answering general inquiries and checking service prices to handling complex multi-step bookings and proactive customer retention.

![system](https://res.cloudinary.com/dagj9begy/image/upload/v1777435970/Screenshot_2026-04-29_094109_ecce0f.png)

## 🚀 Core Features

### 1. AI-Powered Conversational Interface
*   **Intelligent NLP**: Uses **Groq (Llama 3.3 70B)** to understand natural language, greeting customers, and answering salon-specific questions (hours, location, services).
*   **Input Normalization**: Automatically extracts names, dates, and intent from raw SMS messages.
*   **Smart Routing**: Intelligently switches between general chat and the booking flow based on user intent.

### 2. Automated Square Booking Flow
*   **Step-by-Step Guidance**: Leads customers through service selection, name entry, date picking, and time slot selection.
*   **Real-time Availability**: Direct integration with the **Square API** to fetch live slots.
*   **Relative Date Parsing**: Supports inputs like "next Tuesday", "tomorrow", or "21st April".
*   **Alternative Suggestions**: If a requested date is fully booked, the bot automatically searches and suggests slots for the next 3 available days.
*   **Instant Confirmation**: Automatically creates customer profiles and bookings in Square and sends a confirmation SMS with a reference number.

### 3. Proactive Customer Retention
*   **Cadence Analysis**: Daily scheduled triggers analyze the booking history (last 90 days) for every customer.
*   **Personalized Nudges**: Calculates a customer's typical return interval and sends a "reminder" SMS at 75% of that cycle.
*   **Win-Back Logic**: Sends a "We Miss You" message to single-visit customers after 30 days of inactivity.
*   **Duplicate Prevention**: Logs every retention message in Supabase to ensure customers aren't over-messaged.

### 4. Enterprise-Grade Management
*   **Session Persistence**: Uses **Supabase** to manage booking sessions, ensuring customers can pick up where they left off.
*   **Session Cleanup**: Automatically notifies users when a booking session has timed out due to inactivity.
*   **Daily Analytics**: Rollup system that tracks total messages, bookings confirmed, tokens used, and SMS cap hits.
*   **Safety Limits**: Configurable daily SMS caps per user to prevent spam and control costs.

---

## 🛠 Tech Stack

*   **Automation**: [n8n](https://n8n.io/)
*   **AI/LLM**: [Groq](https://groq.com/) (Llama-3.3-70b-versatile)
*   **SMS Gateway**: [ClickSend](https://www.clicksend.com/)
*   **Backend Database**: [Supabase](https://supabase.com/) (PostgreSQL)
*   **Salon Management**: [Square](https://squareup.com/) (Catalog, Bookings, Customers)

---

## ⚙️ Setup & Installation

### Prerequisites
*   An active **n8n** instance.
*   **Groq API Key** for AI processing.
*   **Square Account** (Sandbox or Production) with Location ID.
*   **Supabase Project** with the following tables:
    *   `booking_sessions` (Session state)
    *   `message_logs` (Conversation history)
    *   `daily_analytics` (Performance tracking)
    *   `retention_sms_log` (Retention tracking)
*   **ClickSend Account** for SMS delivery.

### Installation Steps
1.  **Import Workflow**: Download the `Salon SMS Bot V4_1.json` file and import it into your n8n instance.
2.  **Configure Credentials**:
    *   Set up **HTTP Basic Auth** for ClickSend.
    *   Update the **Header Parameters** in Square and Supabase nodes with your respective API keys.
3.  **Set Environment Variables**:
    *   `CLICKSEND_FROM_NUMBER`: Your dedicated SMS number.
4.  **Update Node Constants**:
    *   Open the `Build Groq Prompt` and `Check Staff Availability` nodes to update salon details (Name, Phone, Staff Names, Location IDs).
5.  **Activate**: Turn the workflow on and point your ClickSend inbound webhook to the **Webhook SMS Inbound** URL.

---

## 📊 Database Schema (Supabase)

To support the bot, ensure your Supabase instance has the following structure:

| Table | Purpose |
| :--- | :--- |
| `booking_sessions` | Tracks active booking stages, user phone, and selected service/date/time. |
| `message_logs` | Stores all IN/OUT messages, intent, tokens used, and timestamps. |
| `daily_analytics` | Aggregates daily performance (runs via `Schedule Analytics`). |
| `retention_sms_log` | Tracks automated nudges to prevent duplicate messaging. |

---

## 📝 License
This project is for demonstration purposes. Ensure you comply with local SMS marketing laws (e.g., TCPA/ACMA) before deploying to production.
