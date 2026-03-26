# Loan Decision Engine (Combined Frontend + Backend)

This repository contains both the frontend and backend for the assignment submission.

It combines the original separate repositories:

- Frontend: [loan-frontend](https://github.com/FerayGuluzada/loan-frontend)  
- Backend: [LoanDecisionEngine](https://github.com/FerayGuluzada/LoanDecisionEngine)  

---

## Stack / Technologies Used

- **Backend:** Java, Spring Boot, Lombok
- **Frontend:** React, JavaScript, CSS
- **Communication:** REST API (`/loan/decision`)
- **Data:** Hardcoded user segments for demonstration

---

Decision Engine

The backend determines loan approval and computes the maximum possible loan based on a **user segment**.

### User Segments

Users are assigned segments using a hardcoded repository:

| Personal Code    | Segment     | Credit Modifier |
|-----------------|------------|----------------|
| 49002010965      | DEBT       | 0              |
| 49002010976      | SEGMENT_1  | 100            |
| 49002010987      | SEGMENT_2  | 300            |
| 49002010998      | SEGMENT_3  | 1000           |

> Users not listed default to `SEGMENT_1`.

### Loan Decision Logic

1. **Reject DEBT users** immediately.  
2. **Clamp input values** to allowed ranges:  
   - Amount: €2000–€10000  
   - Period: 12–60 months  
3. **Compute maximum loan per period**:

The formula for maximum loan is derived from the credit score condition:

creditScore = (creditModifier / loanAmount) * loanPeriod ≥ 1

Rewriting for the maximum amount per period:

maxAmountForPeriod = creditModifier * loanPeriod

- The backend loops through all periods (12–60 months) and selects the period that **produces the largest approvable loan**.
- Caps are applied to ensure the result is within allowed limits (€2000–€10000).  

4. **Return result**:
   - If a requested amount is within the computed maximum → approve requested amount  
   - Otherwise → approve **maximum possible amount**
