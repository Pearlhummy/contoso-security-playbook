# Use Case 2 – Protecting Finance Data with Microsoft Purview

**Analyst:** [Your Full Name]
**Program:** Platform Explorers – Cybersecurity
**Security Platform:** Microsoft Purview
**Data Protection Solution:** Microsoft Purview Information Protection & DLP
**Date:** [Date]

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

Before building anything in Purview, identify what data you need to protect.

**Two finance identifiers were selected:**

- **Croatian Bank Account Number** — IBAN format starting with `HR` followed by 19 digits
- **Nigerian Bank Verification Number (BVN)** — exactly 11 digits

> These two formats represent the kind of structured financial data that Purview can be taught to recognize using patterns (regex).

![Finance identifiers selected for classification](doc/02_detection-formats_notes.png)
*Figure: Finance identifiers selected for classification.*

---

## 5. Technical Implementation

---

### Step 1 — Access Microsoft Purview

1. Open your browser and go to [https://purview.microsoft.com](https://purview.microsoft.com)
2. Sign in with your Microsoft 365 admin account
3. You'll land on the Purview portal home page

![Microsoft Purview portal landing page](doc/01_purview_portal-home.png)
*Figure: Microsoft Purview portal home.*

---

### Step 2 — Create a Custom Sensitive Information Type (SIT)

A **Sensitive Information Type** teaches Purview how to recognize specific data. Think of it like training a detector to spot a particular pattern — similar to how spam filters learn to recognize junk mail.

#### 2.1 — Open the SIT section

1. In the left sidebar, click **Data classification**
2. Click **Sensitive info types**
3. Click **+ Create sensitive info type**

![List of sensitive info types](doc/03_sit_sensitive-info-types_list.png)
*Figure: Sensitive info types list.*

---

#### 2.2 — Name and describe the SIT

| Field | What to enter |
|---|---|
| Name | `Finance – Croatian IBAN & Nigerian BVN` |
| Description | Detects Croatian IBAN and Nigerian BVN identifiers used by the finance department |

Click **Next**.

![SIT name and description](doc/04_sit_create_name-description.png)
*Figure: SIT name and description.*

---

#### 2.3 — Add Pattern 1: Croatian IBAN

Click **+ Create pattern**, then:

**Primary element — Regular expression:**

```
\bHR[0-9]{19}\b
```

This tells Purview: find any string that starts with `HR` followed by exactly 19 digits.

**Supporting element — Keyword list:**

Add these keywords so Purview only flags the number when these words appear nearby (reduces false positives):

```
IBAN, Croatian IBAN, HR account, bank account
```

Set confidence to **Medium confidence**, then click **Done**.

![Croatian IBAN regex](doc/05_sit_regex_croatian-iban.png)
*Figure: Croatian IBAN regex pattern.*

![Croatian IBAN keyword list](doc/06_sit_keywordlist_croatian-iban.png)
*Figure: Croatian IBAN supporting keywords.*

![Croatian IBAN pattern complete](doc/07_sit_supportin-elements_attached_croatian-iban.png)
*Figure: Croatian IBAN pattern with supporting elements attached.*

---

#### 2.4 — Add Pattern 2: Nigerian BVN

Click **+ Create pattern** again, then:

**Primary element — Regular expression:**

```
\b[0-9]{11}\b
```

This tells Purview: find any 11-digit number.

**Supporting element — Keyword list:**

```
BVN, Bank Verification Number, BVN Number
```

Set confidence to **Medium confidence**, then click **Done**.

![Nigerian BVN regex](doc/09_sit_regex_primary_nigerian-bvn.png)
*Figure: Nigerian BVN regex pattern.*

![Nigerian BVN keyword list](doc/10_sit_keywordlist_nigerian-bvn.png)
*Figure: Nigerian BVN supporting keywords.*

![Both patterns created](doc/12_sit_patterns_page_pattern2-created_nigerian-bvn.png)
*Figure: Both patterns created on the patterns page.*

---

#### 2.5 — Review and create the SIT

Review your settings on the summary page, then click **Create**.

![SIT review page](doc/13_sit_review_page_before-finish.png)
*Figure: SIT review before finishing.*

![SIT created confirmation](doc/14_sit_creation_page.png)
*Figure: SIT successfully created.*

![SIT visible in list](doc/15_sit_list_custom-sit-visible.png)
*Figure: Custom SIT visible in the sensitive info types list.*

---

### Step 3 — Test the SIT

Before moving on, confirm that Purview actually detects your patterns.

1. Create a plain `.txt` file on your computer with this content:

```
Employee BVN: 12345678901
Croatian IBAN: HR1210010051863000160
```

2. On the SIT page, click **Test**
3. Upload the file
4. Purview should return: **Match found**

If it doesn't match, go back and check your regex — a single typo breaks the pattern.

![Test file content](doc/16_testfile_content_hr-iban_bvn.png)
*Figure: Sample test file containing both identifiers.*

![Upload test file](doc/17_sit_test_upload_file.png)
*Figure: Uploading the test file into Purview.*

![SIT test results — success](doc/18_sit_test_results_success.png)
*Figure: SIT test returned a successful match.*

---

### Step 4 — Create a Sensitivity Label

A **Sensitivity Label** is the stamp that gets applied to a file or email. It carries your protection settings — encryption, access restrictions, and visual markings.

#### 4.1 — Open the Labels section

1. In the left sidebar, click **Information protection**
2. Click **Labels**
3. Click **+ Create a label**

![Sensitivity labels page](doc/19_label_sensitivity-labels_page.png)
*Figure: Sensitivity labels list.*

---

#### 4.2 — Name and describe the label

| Field | What to enter |
|---|---|
| Name | `Finance – Confidential` |
| Display name | `Finance – Confidential` |
| Description for users | This document contains sensitive financial data. Do not share outside the finance department. |
| Description for admins | Auto-applied when Croatian IBAN or Nigerian BVN is detected |

Click **Next**.

![Label basics](doc/20_label_basics_confidential-finance.png)
*Figure: Label name and description.*

---

#### 4.3 — Set the scope

Scope tells Purview where this label can be applied.

Select:
- ✅ Files & emails

Click **Next**.

![Label scope](doc/22_label_scope_files-and-emails.png)
*Figure: Label scope set to files and emails.*

---

#### 4.4 — Set encryption and access control

1. Check **Encrypt files and emails**
2. Under **Assign permissions now or let users decide**, select **Assign permissions now**
3. Click **+ Add or remove users**
4. Add the finance department group (e.g., `finance@yourcompany.com`)
5. Set their permission to **Co-Owner**
6. Do not add anyone else — that means no access for everyone outside finance

![Access control settings](doc/25_label_access-control_final-permissions.png)
*Figure: Encryption and access control configured for finance group only.*

---

#### 4.5 — Add content marking

Content marking adds a visible header to labeled documents so users know the file is protected.

1. Turn on **Content marking**
2. Select **Add a header**
3. Header text: `CONFIDENTIAL – FINANCE ONLY`

![Header marking configured](doc/26_label_content-marking_header-configured.png)
*Figure: Header content marking configured.*

---

#### 4.6 — Review and create the label

Review all settings, then click **Create label**.

> ⚠️ Do not publish the label yet. Publishing happens through the auto-labeling policy in the next step.

![Label review](doc/29_label_review_final-summary.png)
*Figure: Final label review before creation.*

![Label created](doc/30_label_created_success_dont-publish-yet.png)
*Figure: Label created successfully — not yet published.*

---

### Step 5 — Configure an Auto-Labeling Policy

An **auto-labeling policy** connects your SIT to your label. It tells Purview: "when you detect the BVN or Croatian IBAN, apply the Finance – Confidential label automatically — without the user doing anything."

#### 5.1 — Open auto-labeling

1. Click **Information protection** in the left sidebar
2. Click **Auto-labeling**
3. Click **+ Create auto-labeling policy**

![Auto-labeling page](doc/32_autolabeling_policy_page.png)
*Figure: Auto-labeling policies page.*

---

#### 5.2 — Choose policy type and info to label

1. Select **Apply label only** as the policy type
2. Select **Custom** under info to label

![Policy type](doc/33_autolabeling_policy_type_apply-only.png)
*Figure: Policy type set to apply-only.*

![Custom info type selected](doc/34_autolabeling_info-to-label_custom.png)
*Figure: Custom info type selected.*

---

#### 5.3 — Configure the rule

1. Click **+ Add rule**
2. Under **Conditions**, click **+ Add condition**
3. Select **Content contains** → **Sensitive info types**
4. Search for and select `Finance – Croatian IBAN & Nigerian BVN`
5. Set the label to apply: `Finance – Confidential`

![Rule condition with custom SIT selected](doc/41_autolabeling_rule_condition_custom-sit-selected.png)
*Figure: Rule condition using the custom SIT.*

---

#### 5.4 — Set policy mode to Simulation

> **Why simulation?** Running in simulation mode first lets you see what Purview *would* have labeled — without affecting any real files yet. This is how you confirm the policy works before going live.

Select **Simulation mode**, then click **Next** and create the policy.

![Simulation mode selected](doc/45_autolabeling_policy-mode_simulation.png)
*Figure: Policy mode set to simulation.*

![Auto-labeling policy created](doc/47_autolabeling_policy_created_success.png)
*Figure: Auto-labeling policy created successfully.*

---

### Step 6 — Create a DLP Policy to Block M365 Copilot

A **DLP (Data Loss Prevention) policy** is the enforcer. It reads the label on a file or email and decides whether to allow or block an action. Here, the goal is to stop Microsoft 365 Copilot from processing any content that contains finance-sensitive data.

#### 6.1 — Open DLP Policies

1. In the left sidebar, click **Data loss prevention**
2. Click **Policies**
3. Click **+ Create policy**
4. Select **Custom policy**
5. Name it: `Block Finance Data from Copilot`
6. Description: `Prevents M365 Copilot from processing files and emails containing finance-sensitive data`

---

#### 6.2 — Set locations

Turn **on** these locations:

- ✅ Exchange email
- ✅ SharePoint sites
- ✅ OneDrive accounts
- ✅ Microsoft Copilot

---

#### 6.3 — Create the rule

1. Click **+ Create rule**
2. Name: `Finance SIT detected – Block Copilot`
3. Under **Conditions**, add:
   - **Content contains** → Sensitive info types → `Finance – Croatian IBAN & Nigerian BVN`

---

#### 6.4 — Set the action

Under **Actions**:

- Select **Block Microsoft 365 Copilot from processing this content**

![DLP rule action — block Copilot](doc/59_dlp_rule_action_block_processing-prompts.png)
*Figure: DLP rule action set to block Copilot from processing prompts.*

![DLP rule overview](doc/61_dlp_rule_overview_block-copilot-confirmed.png)
*Figure: DLP rule overview confirming block action.*

---

#### 6.5 — Set policy mode

Start in **Simulation mode**, same reason as before — verify before enforcing.

![DLP policy mode — simulation](doc/62_dlp_policy_mode_simulation_confirmed.png)
*Figure: DLP policy mode set to simulation.*

![DLP policy created](doc/64_dlp_policy_created_success.png)
*Figure: DLP policy created successfully.*

---

## 6. Testing and Validation

### Test 1 — Copilot Prompt Test

Open Microsoft 365 Copilot and type a prompt that includes a Nigerian BVN or Croatian IBAN. Copilot should either block the response or log the interaction as a policy match.

![Copilot prompt test with BVN](doc/66_test_copilot_prompt_with-bvn.png)
*Figure: Test prompt containing a BVN submitted to Copilot.*

---

### Test 2 — Activity Explorer

1. Go to **Data classification** → **Activity explorer**
2. Filter by your DLP policy
3. Confirm that Purview logged the detection event

![Activity Explorer showing Copilot SIT detection](doc/67_dlp_activity-explorer_copilot_sit-detected.png)
*Figure: Activity Explorer showing the SIT was detected in a Copilot interaction.*

---

### Test 3 — Alert Verification

1. Go to **Data loss prevention** → **Alerts**
2. Confirm an alert was generated for the BVN detection

![DLP alert for Copilot BVN](doc/70_dlp_alert_copilot_bvn.png)
*Figure: DLP alert triggered by BVN detected in Copilot.*

---

## 7. End-to-End Flow

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
Alert is generated and logged in Activity Explorer
```

---

## 8. Conclusion

This exercise covers a complete finance data protection implementation using Microsoft Purview. A custom SIT was built to detect two specific finance identifiers. A sensitivity label was created and configured to encrypt and restrict access to finance data. An auto-labeling policy connects detection to labeling automatically. A DLP policy blocks Microsoft 365 Copilot from processing any content flagged by the SIT. All policies were validated using simulation mode, Activity Explorer, and live Copilot testing.
