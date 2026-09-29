# 🏋️ Gym Management & Registration Automation

An automated gym membership management workflow built with **n8n**, **Google Sheets**, **Google Forms/Sheets submissions**, **Google Gemini**, **Telegram**, and **Gmail**.

The workflow validates new registrations, creates member and membership records, calculates membership end dates, records payments, sends confirmation messages, and connects members to a Telegram-based attendance flow.

## ✨ Features

- Automated registration intake from Google Sheets
- Basic deterministic validation for:
  - Name
  - Egyptian mobile number
  - Email
  - Date of birth
  - Membership type
  - Start date
  - Payment method
- AI-assisted validation using Google Gemini
- Automatic Member ID generation
- Membership price and duration assignment
- Membership end-date calculation
- Separate member, membership, payment, and registration-log records
- Confirmation email for successful registration
- Telegram notification for failed registration
- Telegram `/start GYM-...` member-account linking
- Stores the member's Telegram chat ID for future gym interactions

## 🔄 Main Registration Workflow

```text
Google Sheets Trigger
        ↓
Extract Registration Fields
        ↓
JavaScript Validation
        ↓
Basic Validation
   ┌────┴────┐
 Invalid     Valid
    ↓          ↓
Telegram    AI Validation
Notification     ↓
             Final Validation
                  ↓
             Generate Member ID
                  ↓
             Membership Pricing
                  ↓
             Calculate End Date
                  ↓
          ┌───────┼────────┐
          ↓       ↓        ↓
       Members Membership Payments
          └───────┼────────┘
                  ↓
            Confirmation Email
```

## 💳 Membership Plans

The workflow currently contains these pricing rules:

| Membership | Duration | Price |
|---|---:|---:|
| 1 Month | 1 month | 500 EGP |
| 3 Months | 3 months | 1,300 EGP |
| 6 Months | 6 months | 2,400 EGP |
| 1 Year | 12 months | 4,500 EGP |

## 🧠 Validation

The JavaScript validation layer performs deterministic checks before the AI stage.

### Name
Checks that the name has at least two words and contains Arabic or English letters.

### Egyptian Phone
Validates the Egyptian mobile pattern:

```text
010xxxxxxxx
011xxxxxxxx
012xxxxxxxx
015xxxxxxxx
```

### Email
Checks for a standard email format.

### Dates
Validates the date of birth and membership start date and rejects invalid calendar dates.

### Membership
Accepts the configured membership plans.

### Payment
Checks the configured payment methods.

The trainer field is optional.

## 🤖 AI Validation

Registrations that pass the basic validation stage are passed to an AI validation agent powered by Google Gemini.

The AI agent evaluates whether the submitted registration information is reasonable and consistent.

It returns fields such as:

- `valid`
- `name_valid`
- `phone_valid`
- `email_valid`
- `date_of_birth_valid`
- `membership_valid`
- `start_date_valid`
- `payment_method_valid`
- `reason`

## 🆔 Member & Membership IDs

The workflow generates unique-style identifiers using the current timestamp:

```text
GYM-YYYYMMDDHHMMSS
MEM-YYYYMMDDHHMMSS
PAY-YYYYMMDDHHMMSS
```

## 📅 Membership End Date

After the membership plan is selected, JavaScript calculates the end date based on the start date and membership duration.

The calculation also handles months with different numbers of days.

## 📊 Google Sheets Data Model

The workflow writes information into separate sheets for:

- Members
- Memberships
- Payments
- Registration Log

This separates the core gym entities and makes the workflow easier to extend.

## 📧 Email Confirmation

Successful registrations receive an email containing:

- Member ID
- Membership type
- Start date
- End date
- Amount paid
- Payment method
- Trainer
- Telegram attendance activation instructions

## 🤖 Telegram Integration

The workflow also includes a Telegram account-linking flow.

A member can start the bot using:

```text
/start GYM-MEMBER_ID
```

The workflow extracts the Member ID, finds the corresponding member record, stores the Telegram chat ID, and can then use that Telegram connection for future gym interactions.

## 🛠️ Tech Stack

- **n8n** — Workflow automation
- **Google Sheets** — Registration and gym database
- **Google Gemini** — AI-assisted registration validation
- **JavaScript** — Validation, ID generation, and date calculations
- **Telegram** — Member account linking and notifications
- **Gmail** — Registration confirmation emails

## 📂 Project Structure

```text
gym-management-n8n/
│
├── workflow/
│   └── gym-management-automation.json
│
├── screenshots/
│   └── workflow.png
│
├── README.md
└── .gitignore
```

## 🚀 Setup

1. Open your n8n instance.
2. Import:

```text
workflow/gym-management-automation.json
```

3. Connect your own:
   - Google Sheets account
   - Google Gemini account
   - Telegram bot
   - Gmail account
4. Configure your Google Sheet.
5. Update the membership pricing if required.
6. Configure your Telegram bot username.
7. Test the registration workflow with sample data.

## 🔐 Security

Do not commit:

- API keys
- OAuth credentials
- Telegram bot tokens
- Private Google Sheet URLs
- Real member data
- Phone numbers
- Personal emails
- Private Telegram chat IDs

The workflow in this repository is sanitized for public GitHub sharing.

## ⚠️ Important Notes

This repository contains an automation prototype. Production deployments should add authentication, stronger database controls, error handling, audit logging, and appropriate protection for member personal information.

## 🔮 Future Improvements

- Automated membership-expiry reminders
- Attendance tracking
- Subscription renewal automation
- QR-code based check-in
- Admin dashboard
- Payment verification
- WhatsApp integration
- Automated trainer assignment
- Membership analytics
- Expiry and renewal notifications

## 👨‍💻 Author

**Mostafa Mohamed**

AI / Machine Learning Developer
