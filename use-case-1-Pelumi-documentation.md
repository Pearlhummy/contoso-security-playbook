# Use Case 1 – Mitigating Fraudulent Emails at Contoso

**Analyst:** Oluwapelumi Peter  
**Program:** Platform Explorers – Cybersecurity  
**Security Platform:** Microsoft 365 Defender  
**Email Security Solution:** Microsoft Defender for Office 365 Plan 2  
**Date:** 3 May 2026  

---

# 1. Objective of This Exercise

The goal of this exercise is to:

- Configure Microsoft Defender for Office 365 security policies to mitigate fraudulent emails
- Protect VIP users such as the CEO and CFO from phishing and spam attacks
- Configure Anti-Spam, Anti-Phishing, Safe Links, and Safe Attachments policies
- Test the effectiveness of the configurations using a GTUBE spam test email
- Implement policies on a pilot group before organization-wide deployment
- Generate mail flow and threat reports for stakeholders
- Document the implementation process with screenshots for presentation and review

---

# 2. Business Problem Overview

Contoso is experiencing an increase in fraudulent emails reaching users’ mailboxes. These emails include spam, phishing attempts, malicious links, and impersonation attacks targeting executives and employees.

VIP users such as the CEO and CFO are considered high-value targets because attackers often impersonate executives to steal sensitive information, gain unauthorized access, or commit financial fraud.

The organization requires a secure email protection solution that can:

- Detect and block phishing attempts
- Reduce spam emails
- Prevent malicious links and attachments
- Protect executive users from impersonation attacks
- Provide visibility through reporting and monitoring tools

---

# 3. Recommended Microsoft Security Solution

The recommended solution for this environment is:

## Microsoft Defender for Office 365 Plan 2

This plan was selected because it provides advanced email security capabilities including:

- Anti-Spam Protection
- Anti-Phishing Protection
- Safe Links
- Safe Attachments
- Threat Explorer
- Automated Investigation and Response
- Attack Simulation Training
- Advanced Reporting and Threat Visibility

These features help strengthen the organization’s security posture against modern email-based attacks.

---

# 4. Environment Overview

- **Security Platform:** Microsoft 365 Defender  
- **Portal:** https://security.microsoft.com  
- **Policy Deployment Type:** Pilot Deployment  
- **Threat Types:** Spam, Phishing, Malware, Impersonation  
- **Protected Executive Users:**  
  - PradeepG@platexp.onmicrosoft.com (CEO)  
  - AdeleV@platexp.onmicrosoft.com (CFO)  

---

# 5. Pilot Deployment Strategy

Before deploying policies organization-wide, a pilot deployment strategy was implemented.

This approach was chosen to:

- Reduce the risk of disrupting normal business operations
- Identify false positives before full deployment
- Validate policy effectiveness safely
- Ensure executive users receive immediate protection
- Allow administrators to monitor policy behavior before organization-wide rollout

A dedicated security group was created for testing purposes.

---

# 6. Technical Implementation

---

# Step 1: Accessing Microsoft Defender Portal

All configurations were performed within the Microsoft Defender portal.

**Portal:**  
https://security.microsoft.com

**Navigation Path:**  
Email & collaboration → Policies & rules → Threat policies

## Screenshot Evidence – Microsoft Defender Portal

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 01 26 10" src="https://github.com/user-attachments/assets/54ad4029-befd-465e-871e-c9a6a7abb7a1" />


*Figure 1: Microsoft Defender portal used for configuring email security policies.*

---

# Step 2: Creating the Inbound Anti-Spam Policy

A custom inbound anti-spam policy was created to help reduce spam and phishing emails entering the organization.

Inbound filtering was selected because the threats originate from external senders attempting to deliver malicious emails to internal users.

**Policy Name:**  
`Contoso-AntiSpam-VIP`

**Policy Type:**  
Inbound Anti-Spam Policy

**Applied To:**  
`PradeepG@platexp.onmicrosoft.com (CEO) and AdeleV@platexp.onmicrosoft.com (CFO).`

**Navigation Path:**  
Threat Policies → Anti-Spam → Create Policy → Inbound

## Screenshot Evidence – Anti-Spam Policy Creation

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 37 17" src="https://github.com/user-attachments/assets/00064b76-1575-4932-b9f1-97f02f61c154" />


*Figure 2: Creation of inbound anti-spam policy for VIP users.*

---

# Step 3: Configuring Spam Protection Settings

The following anti-spam actions were configured:

| Threat Type | Configured Action |
|---|---|
| Spam | Quarantine |
| High-Confidence Spam | Quarantine |
| Phishing Email | Quarantine |
| Bulk Email Threshold | 5 |

Safety tips and user notifications were also enabled to improve user awareness.

## Screenshot Evidence – Spam Protection Settings

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 45 08" src="https://github.com/user-attachments/assets/54f78c64-d756-4453-948d-77cc345ffb97" />

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 45 15" src="https://github.com/user-attachments/assets/d3eab6d7-f7d9-48cf-aaa2-76f69e720126" />

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 46 56" src="https://github.com/user-attachments/assets/53e35882-a4e4-4170-b092-a361fed0c67a" />


*Figure 3: Spam and phishing protection actions configured within the anti-spam policy.*

---

# Step 4: Creating Anti-Phishing Policy

A dedicated anti-phishing policy was created to protect executive users from impersonation attacks.

This policy helps detect attackers pretending to be trusted users or company domains.

**Policy Name:**  
`Contoso-AntiPhishing-VIP`

**Protected Users:**
- PradeepG@platexp.onmicrosoft.com
- AdeleV@platexp.onmicrosoft.com

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 34 04" src="https://github.com/user-attachments/assets/c05f3877-c202-4d9f-9d71-3897fad0ea1c" />


**Navigation Path:**  
Threat Policies → Anti-Phishing → Create Policy

## Screenshot Evidence – Anti-Phishing Policy

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 33 53" src="https://github.com/user-attachments/assets/4ff5a475-8112-4dea-8b6f-2ae7b057ed46" />


*Figure 4: Anti-phishing policy created to protect executive users from impersonation attacks.*

---

# Step 5: Configuring Impersonation Protection

The following advanced phishing protection settings were enabled:

| Protection Setting | Status |
|---|---|
| User Impersonation Protection | Enabled |
| Domain Impersonation Protection | Enabled |
| Mailbox Intelligence | Enabled |
| Spoof Intelligence | Enabled |

Threat actions for impersonation attempts were configured to quarantine malicious emails.

## Screenshot Evidence – Impersonation Protection Settings

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 34 52" src="https://github.com/user-attachments/assets/1f0375f9-7e2a-449d-b630-37074418109c" />

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 35 12" src="https://github.com/user-attachments/assets/1e96bd18-94b4-47b4-945d-3d74bbc03d50" />

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 35 40" src="https://github.com/user-attachments/assets/a23c19c8-e7b4-4c9b-a341-475b2a1d1bf8" />


*Figure 5: Advanced impersonation protection settings enabled for executive protection.*

---

# Step 6: Configuring Safe Links Policy

Safe Links was configured to protect users from malicious URLs embedded in emails.

Microsoft Defender scans links in real time when users click them.

**Policy Name:**  
`Contoso-SafeLinks-VIP`

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 48 13" src="https://github.com/user-attachments/assets/298da377-175e-4832-894f-7df25ede7e76" />


**Applied To:**  
`PradeepG@platexp.onmicrosoft.com (CEO) and AdeleV@platexp.onmicrosoft.com (CFO).`

**Enabled Features:**
- Real-time URL scanning
- Click tracking
- Email protection

**Navigation Path:**  
Threat Policies → Safe Links

## Screenshot Evidence – Safe Links Configuration

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 54 52" src="https://github.com/user-attachments/assets/b7bf1b8f-7198-48cc-ad46-1c53f7a2ee8d" />


*Figure 6: Safe Links policy configured to scan malicious URLs in emails.*

---

# Step 7: Configuring Safe Attachments Policy

Safe Attachments was configured to scan email attachments for malware and malicious content.

**Policy Name:**  
`Contoso-SafeAttachments-VIP`

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 55 38" src="https://github.com/user-attachments/assets/656560e9-45aa-4dfd-b430-871c047b0d11" />


**Applied To:**  
`PradeepG@platexp.onmicrosoft.com (CEO) and AdeleV@platexp.onmicrosoft.com (CFO).`

**Configured Action:**  
Block detected malware and use Dynamic Delivery

**Navigation Path:**  
Threat Policies → Safe Attachments

## Screenshot Evidence – Safe Attachments Policy

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 57 49" src="https://github.com/user-attachments/assets/8579f55a-d842-4d01-9fa5-cb7473c20536" />

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 58 54" src="https://github.com/user-attachments/assets/394c9b29-f44f-4b39-8d53-e79bbc56acc9" />

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 00 59 04" src="https://github.com/user-attachments/assets/f8c5994a-30b6-4468-b356-bec10eb2756e" />


*Figure 7: Safe Attachments policy configured for malware protection.*

---

# Step 8: Policy Review and Deployment

All configured policies were reviewed and successfully deployed to the pilot security group.

Policies created include:

- Anti-Spam Policy
- Anti-Phishing Policy
- Safe Links Policy
- Safe Attachments Policy

---

# 7. Testing and Validation

---

# Step 9: Attacker Simulation

A personal Gmail account was created to simulate an external attacker attempting to send spam emails into the organization.

This helps validate whether Microsoft Defender policies are functioning correctly.

---

# Step 10: GTUBE Spam Test

A GTUBE spam test email was sent from the external Gmail account to the CEO mailbox.

GTUBE is a standard spam testing method used to validate spam filtering systems.

## Test Email Details

**Sender:**  
gbadebo935@gmail.com

**Recipient:**  
PradeepG@platexp.onmicrosoft.com

**Subject:**  
GTUBE spam filter test

**Body (single line):**

```text
XJS*C4JDBQADN1.NSBN3*2IDNEN*GTUBE-STANDARD-ANTI-UBE-TEST-EMAIL*C.34X
```

## Screenshot Evidence – GTUBE Spam Test

<img width="632" height="1280" alt="WhatsApp Image 2026-05-09 at 01 13 19" src="https://github.com/user-attachments/assets/cc0f2768-ad33-40e1-b668-76f5ba221b5a" />


*Figure 10: GTUBE spam test email sent from external attacker simulation account.*

---

# Step 11: Validation of Policy Effectiveness

After the GTUBE email was sent, Microsoft Defender successfully identified the message as spam.

The message was processed according to the configured anti-spam actions and routed away from the user inbox.

Validation was confirmed through:

- Microsoft Defender Explorer
- Quarantine dashboard
- Message trace monitoring

This confirmed that the configured policies were functioning correctly.

## Screenshot Evidence – Spam Detection Results

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 01 03 14" src="https://github.com/user-attachments/assets/b4adab38-4036-47d5-b4f3-895340bb810a" />


*Figure 11: Microsoft Defender successfully detecting and filtering the GTUBE spam test email.*

---

# 8. Mail Flow Reporting and Monitoring

Microsoft Defender reporting tools were used to monitor email security events and generate reports for stakeholders.

## Reports Reviewed

- Threat Protection Status
- Spam Detections
- Top Targeted Users
- Malware Reports
- URL Protection Reports

**Navigation Path:**  
Email & collaboration → Reports

These reports provide visibility into:
- Spam trends
- Phishing attempts
- Malware detections
- Executive targeting activity
- Email security posture

Reports can be exported in CSV or Excel format for management review.

## Screenshot Evidence – Mail Flow and Threat Reports

<img width="1673" height="926" alt="Screenshot 2026-05-09 at 01 06 01" src="https://github.com/user-attachments/assets/50dbb80d-3e69-42c5-ada1-a5567d320e2c" />


*Figure 12: Threat reporting dashboard used for monitoring and stakeholder reporting.*

---

# 9. Potential Implications of the New Policies

While the implemented policies significantly improve security, there are potential operational considerations:

- Legitimate emails may occasionally be quarantined (false positives)
- Users may require awareness training regarding quarantine notifications
- Administrators must regularly monitor quarantine and threat reports
- Policies may require tuning over time based on business requirements

These considerations reinforce the importance of pilot testing before organization-wide deployment.

---

# 10. Conclusion

This exercise demonstrates the successful implementation of advanced email security protections using Microsoft Defender for Office 365 Plan 2.

By deploying Anti-Spam, Anti-Phishing, Safe Links, and Safe Attachments policies to a pilot group containing VIP users, the organization significantly improved protection against fraudulent emails, phishing attacks, malicious links, malware, and impersonation threats.

The implementation was validated through GTUBE spam testing and monitored using Microsoft Defender reporting tools.

The pilot deployment strategy ensures safe testing and controlled rollout before expanding protections across the entire organization.
