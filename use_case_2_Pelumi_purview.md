# Use Case 2 – Protecting Finance Data with Microsoft Purview

**Analyst:** Oluwapelumi Peter  
**Program:** Platform Explorers – Cybersecurity  
**Security Platform:** Microsoft Purview  
**Data Protection Solution:** Microsoft Purview Information Protection & DLP 
**Date:** 14 May 2026 

---

## 1. Objective

- Create a **Custom Sensitive Information Type (SIT)** to detect finance identifiers
- Automatically apply **Sensitivity Labels** to emails and files that contain finance data
- Restrict **Microsoft 365 Copilot** from processing sensitive finance information
- Validate detection, labeling, and enforcement using simulation and activity logs
- Document the full implementation for learning and portfolio purposes

---

## 2. Business Problem

The finance department handles highly sensitive information — bank account numbers, personal identifiers, and financial records. Unauthorized sharing of this information through email, documents, or AI tools like Microsoft 365 Copilot creates serious risks: data leakage, compliance violations, and financial loss.

---

## 3. Environment

| Item | Detail |
|---|---|
| Security Platform | Microsoft Purview |
| Features Used | Sensitive Information Types, Sensitivity Labels, Auto-labeling, DLP |
| Protected Department | Finance |
| AI Protection Scope | Microsoft 365 Copilot |

---

## 4. Finance Data Formats Selected

Before building anything in Purview, I first identify what data I need to protect.

**One finance identifiers was selected:**

- **Nigerian Bank Verification Number (BVN)** — exactly 11 digits

---

## 5. Technical Implementation

---

### Step 1 — Access to the Microsoft Purview

<img width="1680" height="924" alt="Screenshot 2026-05-13 at 21 47 12" src="https://github.com/user-attachments/assets/7a619eeb-35b6-437e-8287-5bcb35bd1562" />


*Figure: Microsoft Purview portal home.*

---

### Step 2 — Creating a Custom Sensitive Information Type (SIT)

A **Sensitive Information Type** teaches Purview how to recognize specific data. Think of it like training a detector to spot a particular pattern — similar to how spam filters learn to recognize junk mail.

#### 2.1 — The SIT section

1. In the left sidebar, I clicked **Data classification**
2. Clicked **Sensitive info types**
3. Clicked **+ Create sensitive info type**

---

#### 2.2 — Name and describe the SIT

| Field | What I entered |
|---|---|
| Name | `Finance – Nigerian BVN` |
| Description | Detects Nigerian Bank Verification Numbers used by the finance department |

<img width="1680" height="924" alt="Screenshot 2026-05-14 at 03 34 13" src="https://github.com/user-attachments/assets/d7342f0b-f12e-4698-a90d-34ac2c01c382" />

*Figure: SIT name and description.*

---

#### 2.3 — Added Pattern: Nigerian BVN

**Primary element — Regular expression:**

```
\b[0-9]{11}\b
```

This tells Purview: find any 11-digit number.

**Supporting element — Keyword list:**

```
BVN, Bank Verification Number, BVN Number
```

Set confidence to **Medium confidence**

<img width="1680" height="924" alt="Screenshot 2026-05-14 at 03 43 10" src="https://github.com/user-attachments/assets/59cf3f74-348f-4b6d-8cca-39b3155d84e1" />

<img width="1680" height="924" alt="Screenshot 2026-05-14 at 03 43 22" src="https://github.com/user-attachments/assets/9e71fd4d-4097-429b-bedd-7e133c9eb80d" />

<img width="1680" height="924" alt="Screenshot 2026-05-14 at 03 43 46" src="https://github.com/user-attachments/assets/7fd05614-fa37-4422-9b19-87e86324839b" />

<img width="1680" height="924" alt="Screenshot 2026-05-14 at 03 44 26" src="https://github.com/user-attachments/assets/9de76585-9814-425e-8b66-b157cdc6234a" />

---

#### 2.5 — Reviewed and created the SIT

<img width="1680" height="924" alt="Screenshot 2026-05-14 at 03 44 38" src="https://github.com/user-attachments/assets/5b14d6b9-887a-438c-ae4c-cf99831d27b9" />

<img width="1680" height="924" alt="Screenshot 2026-05-14 at 03 45 49" src="https://github.com/user-attachments/assets/d01e55ca-3473-498e-9188-84c063181865" />

<img width="1680" height="924" alt="Screenshot 2026-05-14 at 03 47 04" src="https://github.com/user-attachments/assets/7c263115-c464-499a-a81a-2ddaf8e3f27f" />


---

### Step 3 — Tested the SIT

Before moving on, I confirmed that Purview actually detects the patterns.

1. Created a plain `finance-test.docx` file on my computer with this content:

```
Employee Financial Record

Name: John Doe
BVN: 22345678901

This document contains sensitive financial information.
```

2. On the SIT page, clicked **Test**
3. And Uploaded the file
4. Purview returned: **Match found**

<img width="812" height="724" alt="Screenshot 2026-05-14 at 03 52 29" src="https://github.com/user-attachments/assets/14af8f31-07b8-499e-a2ae-2cdeda918288" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 03 53 58" src="https://github.com/user-attachments/assets/7d701596-24c3-434e-afb4-a3a121ac9739" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 03 55 08" src="https://github.com/user-attachments/assets/1282ad5f-7aea-4d32-b9c2-637fbf10c7a7" />


---

### Step 4 — Creating a Sensitivity Label

A **Sensitivity Label** is the stamp that gets applied to a file or email. It carries your protection settings — encryption, access restrictions, and visual markings.

#### 4.1 — The Labels section

1. In the left sidebar, I clicked **Information protection**
2. Then Clicked **Labels**
3. And Clicked **+ Create a label**

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 03 58 21" src="https://github.com/user-attachments/assets/b2232640-4665-4324-bd51-62e9b7e52a07" />


---

#### 4.2 — Naming and describing the label

| Field | What I entered |
|---|---|
| Name | `Finance – Confidential` |
| Display name | `Finance – Confidential` |
| Description for users | This document contains sensitive financial data. Do not share outside the finance department. |
| Description for admins | Auto-applied when Croatian IBAN or Nigerian BVN is detected |

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 00 20" src="https://github.com/user-attachments/assets/0cc3fbb3-9679-49bd-a9cd-914f0cf6d90a" />


---

#### 4.3 — Setting the scope

Scope tells Purview where this label can be applied.

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 01 42" src="https://github.com/user-attachments/assets/39de5731-ccfe-4eb8-9714-3ca195259678" />


---

#### 4.4 — Setting encryption and access control

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 10 15" src="https://github.com/user-attachments/assets/c9f967dd-bace-4196-b99f-e06bf9a957b1" />


---

#### 4.5 — Auto Label for files and emails

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 13 48" src="https://github.com/user-attachments/assets/b4860021-7498-4ebe-aed0-373005bdac3d" />


---

#### 4.6 — Reviewed and created the label

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 14 01" src="https://github.com/user-attachments/assets/4757374d-d4c8-4af7-bdd9-2273ad1df6bf" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 14 15" src="https://github.com/user-attachments/assets/35aef87d-72e2-4fd0-a705-7c1483e90445" />


---

### Step 5 — Publish Labeling Policy

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 16 34" src="https://github.com/user-attachments/assets/bb1f53e2-95e2-4d3b-8a11-108f09ef71b6" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 19 42" src="https://github.com/user-attachments/assets/26f025f7-13ac-44ae-9462-546756dea1b4" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 22 13" src="https://github.com/user-attachments/assets/51563109-98cd-4d30-b614-bfd938e7a6de" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 22 24" src="https://github.com/user-attachments/assets/7b3b76b2-4765-4d4a-88ed-b791263bc302" />

---

### Step 6 — Create a DLP Policy to Block M365 Copilot

A **DLP (Data Loss Prevention) policy** is the enforcer. It reads the label on a file or email and decides whether to allow or block an action. Here, the goal is to stop Microsoft 365 Copilot from processing any content that contains finance-sensitive data.

#### 6.1 — Opened the DLP Policies

1. Selected **Custom policy**
2. Nameed it: `Block Finance Data from Copilot`
3. Description: `Prevents M365 Copilot from processing files and emails containing finance-sensitive data`

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 25 03" src="https://github.com/user-attachments/assets/580cd2fa-e0f0-49e0-a7e4-8157599f9700" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 24 36" src="https://github.com/user-attachments/assets/c14b7b34-5fd0-4c87-8937-ef0d807e828f" />


---

#### 6.2 — Set locations

- ✅ Microsoft Copilot

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 34 50" src="https://github.com/user-attachments/assets/b5cf3a0c-3ee1-4387-8a14-d89622f5dc9c" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 35 01" src="https://github.com/user-attachments/assets/86c0cf2f-646c-438a-b577-f584eca5593b" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 36 07" src="https://github.com/user-attachments/assets/4ffca232-12fa-4e05-b950-ed45aeb84a80" />

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 36 28" src="https://github.com/user-attachments/assets/c7263564-e9c0-4ad6-a3cb-15b853e8cb2f" />


---

## 7. Testing and Validation

### Test — Copilot Prompt Test

Opened Microsoft 365 Copilot and typed a prompt that includes a Nigerian BVN. Copilot blocked the response.

<img width="1678" height="927" alt="Screenshot 2026-05-14 at 04 38 51" src="https://github.com/user-attachments/assets/c7ff3334-7426-4813-9bdf-6792f31c3df3" />

*Figure: Test prompt containing a BVN submitted to Copilot.*

---

### Test 2 — Alert Verification

<img width="1678" height="844" alt="Screenshot 2026-05-14 at 05 32 58" src="https://github.com/user-attachments/assets/46ede0b2-46ed-413b-a224-73a12e71f913" />


*Figure: DLP alert triggered by BVN detected in Copilot.*

---

## 8. End-to-End Flow

```
User creates or sends a file/email
            ↓
Purview scans content for BVN or Croatian IBAN
            ↓
Custom SIT detects a match
            ↓
Auto-labeling policy applies "Finance – Confidential" label
            ↓
DLP policy reads the label/SIT match
            ↓
Copilot is blocked from processing the content
Alert is generated and logged
```

---

## 9. Conclusion

This exercise covers a complete finance data protection implementation using Microsoft Purview. A custom SIT was built to detect two specific finance identifiers. A sensitivity label was created and configured to encrypt and restrict access to finance data. An auto-labeling policy connects detection to labeling automatically. A DLP policy blocks Microsoft 365 Copilot from processing any content flagged by the SIT. All policies were validated using simulation mode, and live Copilot testing.
