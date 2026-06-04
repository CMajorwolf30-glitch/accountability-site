# Mid-Life Accountability Partner Platform

A zero-overhead landing page for managing a high-touch habit accountability service.

## Relational Database Schema Design
For managing client performance and check-in pacing over time, use this relational setup:

```sql
CREATE TABLE pricing_plans (
    plan_id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    price_monthly NUMERIC(5,2) NOT NULL,
    check_ins_per_week INT NOT NULL
);

CREATE TABLE clients (
    client_id SERIAL PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    phone_number VARCHAR(20) NOT NULL UNIQUE,
    preferred_platform VARCHAR(30) DEFAULT 'SMS',
    plan_id INT REFERENCES pricing_plans(plan_id),
    signup_date DATE DEFAULT CURRENT_DATE,
    status VARCHAR(20) DEFAULT 'active'
);

CREATE TABLE client_habits (
    habit_id SERIAL PRIMARY KEY,
    client_id INT REFERENCES clients(client_id) ON DELETE CASCADE,
    habit_name VARCHAR(100) NOT NULL,
    weekly_frequency_target INT NOT NULL
);

CREATE TABLE daily_logs (
    log_id SERIAL PRIMARY KEY,
    client_id INT REFERENCES clients(client_id) ON DELETE CASCADE,
    habit_id INT REFERENCES client_habits(habit_id) ON DELETE CASCADE,
    log_date DATE DEFAULT CURRENT_DATE,
    status_achieved BOOLEAN NOT NULL DEFAULT FALSE,
    client_feedback_snippet TEXT
);