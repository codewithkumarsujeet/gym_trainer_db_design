# 🏋️ Gym Management Database Design – README

## Overview

This database schema is designed for a **Gym / Fitness Training Management System** that supports trainers, clients, programs (plans), sessions, subscriptions, payments, progress tracking, and trainer feedback.

The schema follows normalized relational database design principles and supports real-world requirements such as:

* Multiple trainers per client
* Multiple program subscriptions per client over time
* Payment tracking per subscription
* Session tracking
* Progress tracking history
* Trainer feedback storage
* Separation of sessions and check-ins

This design is scalable and suitable for **backend applications, SaaS fitness platforms, or learning-level production-ready systems**.

---

# 🧱 Core Entities

## 1️⃣ Trainers

Stores trainer information.

**Fields:**

* trainer_id (PK)
* name
* email (unique)
* phone
* created_at

**Relationships:**

* Many-to-many with Clients (via `trainer_client`)
* One-to-many with Sessions
* One-to-many with Trainer Notes

---

## 2️⃣ Clients

Stores client/member details.

**Fields:**

* client_id (PK)
* name
* email
* phone
* status

**Relationships:**

* Many-to-many with Trainers
* Many-to-many with Programs (via `client_programs`)
* One-to-many with Sessions
* One-to-many with Payments
* One-to-many with Progress
* One-to-many with Check-ins
* One-to-many with Trainer Notes

---

## 3️⃣ Programs (Plans)

Stores available gym subscription plans.

**Fields:**

* program_id (PK)
* name
* price
* validity_days
* description
* created_at

**Relationships:**

* Many-to-many with Clients (via `client_programs`)
* One-to-many with Sessions

---

# 🔗 Relationship Tables

## 4️⃣ Trainer_Client (Junction Table)

Maps trainers to clients.

**Purpose:**
Supports many-to-many relationship between trainers and clients.

**Fields:**

* trainer_id (FK)
* client_id (FK)

**Primary Key:**
Composite Primary Key (trainer_id, client_id)

---

## 5️⃣ Client_Programs (Subscription Table)

Stores program subscription history for each client.

**Purpose:**
Allows clients to:

* buy multiple programs
* renew subscriptions
* upgrade plans
* maintain history

**Fields:**

* id (PK)
* client_id (FK)
* program_id (FK)
* start_date
* end_date
* status

---

# 💰 Transaction Tables

## 6️⃣ Payments

Stores payment transactions for program subscriptions.

**Fields:**

* payment_id (PK)
* client_program_id (FK)
* amount
* payment_method
* payment_status
* payment_date

**Relationship:**
Each payment belongs to one client subscription.

---

## 7️⃣ Sessions

Stores trainer-client workout sessions.

**Fields:**

* session_id (PK)
* trainer_id (FK)
* client_id (FK)
* program_id (FK)
* session_date
* start_time
* end_time

**Purpose:**
Tracks actual coaching sessions conducted.

---

# 📊 Tracking Tables

## 8️⃣ Progress

Stores client body progress over time.

**Fields:**

* progress_id (PK)
* client_id (FK)
* weight
* body_fat
* muscle_mass
* recorded_at

**Purpose:**
Maintains historical progress tracking instead of storing only latest values.

---

## 9️⃣ Check-ins

Stores periodic check-in updates separate from sessions.

**Fields:**

* checkin_id (PK)
* client_id (FK)
* weight
* notes
* checkin_date

**Purpose:**
Tracks attendance or periodic updates.

---

## 🔟 Trainer_Notes

Stores trainer feedback for clients.

**Fields:**

* note_id (PK)
* trainer_id (FK)
* client_id (FK)
* note
* created_at

**Purpose:**
Maintains coaching remarks and improvement suggestions.

---

# 📐 Relationship Summary

```
Trainers ↔ Clients (trainer_client)
Clients ↔ Programs (client_programs)
Client_Programs → Payments

Trainers → Sessions
Clients → Sessions
Programs → Sessions

Clients → Progress
Clients → Check-ins

Trainers → Trainer Notes
Clients → Trainer Notes
```

---

# 🧠 Design Decisions Explained

### Why Trainer_Client table exists?

Supports many-to-many trainer-client relationships.

### Why Client_Programs table exists?

Allows multiple subscriptions per client over time.

### Why Payments connect to Client_Programs?

Payments belong to subscriptions, not directly to programs.

### Why Progress stored separately?

Progress changes frequently and requires history tracking.

### Why Sessions and Check-ins separated?

Sessions represent trainer interaction.
Check-ins represent progress/attendance updates.

---

# 🚀 Benefits of This Schema

✔ Normalized structure (3NF)

✔ Supports subscription history

✔ Supports trainer assignments

✔ Tracks fitness progress

✔ Tracks payments properly

✔ Scalable for production systems

✔ Suitable for backend learning projects

✔ Interview-ready database design

