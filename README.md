# Bitespeed-identity-task

## Table of Contents

- [Overview](#overview)
- [Cases & Logic](#cases--logic)
  - [Case 1: Both Mobile and Email are Unique](#case-1-both-mobile-and-email-are-unique)
  - [Case 2: Both Mobile and Email Exist](#case-2-both-mobile-and-email-exist)
    - [Case 2a: On the Same Contact](#case-2a-on-the-same-contact)
    - [Case 2b: On Different Contacts](#case-2b-on-different-contacts)
      - [Case 2ba: One is Primary](#case-2ba-one-is-primary)
      - [Case 2bb: Both are Primary or Both are Secondary](#case-2bb-both-are-primary-or-both-are-secondary)
  - [Case 3: Either Mobile or Email is Unique](#case-3-either-mobile-or-email-is-unique)
- [Response Format](#response-format)
- [Error Handling](#error-handling)
- [Example Request & Response](#example-request--response)
- [Changelog](#changelog)

---

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

## Cases & Logic

### Case 1: Both Mobile and Email are Unique

- **Condition:** Neither the phone number nor the email exists in the database.
- **Action:** Create a new primary contact with the provided details.
- **Link Precedence:** Primary
- **Response:** Returns the newly created contact's details.

---

### Case 2: Both Mobile and Email Exist

#### Case 2a: On the Same Contact

- **Condition:** Both the phone number and email belong to the same contact.
- **Action:** Retrieve and return the primary contact details.
- **Response:** Details of the primary contact.

#### Case 2b: On Different Contacts

##### Case 2ba: One is Primary

- **Condition:** One contact is primary, the other is secondary.
- **Action:** Link the secondary contact to the primary contact.
- **Response:** Details of the primary contact.

##### Case 2bb: Both are Primary or Both are Secondary

- **Condition:** Both contacts are either primary or both are secondary.
- **Action:**
  - Determine the primary contact based on the earliest `createdAt` date.
  - Update all secondary contacts to link to the new primary contact.
  - Demote the other contact to secondary and link it to the primary.
- **Response:** Details of the primary contact.

---

### Case 3: Either Mobile or Email is Unique

- **Condition:** Only one of the phone numbers or email exists in the database.
- **Action:**
  - Retrieve the existing contact.
  - Create a new secondary contact with the new detail (phone/email), linking it to the primary contact.
- **Link Precedence:** Secondary
- **Response:** Details of the primary contact.

---
