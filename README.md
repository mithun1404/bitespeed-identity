# Bitespeed-identity-task


This project solves the problem of reconciling user identities based on shared contact information (email and/or phone number). It is designed for FluxKart via Bitespeed to unify customer profiles and provide a consistent, personalized experience.

---

## API Overview



## Request Format

You must provide at least one of the following fields:

```json
{
  "phoneNumber": "string | null",
  "email": "string | null"
}

## On success, you will receive a response with the consolidated contact identity:

```json
{
  "contact": {
    "primaryContactId": number,
    "emails": [string],
    "phoneNumbers": [string],
    "secondaryContactIds": [number]
  }
}
