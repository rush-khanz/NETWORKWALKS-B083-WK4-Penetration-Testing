# 🔐 NetworkWalks Week 4 — Penetration Testing

## Mediroza General Hospital — Black-Box Penetration Test

This project was completed as part of the **NetworkWalks Cybersecurity Training — Batch B083, Week 4**.

The assessment involved a controlled, authorized black-box penetration test of the simulated **Mediroza General Hospital** web application. The objective was to identify security weaknesses, validate their impact, perform controlled data extraction, and document the findings with appropriate remediation recommendations.

> ⚠️ **Disclaimer:** This assessment was performed only against the authorized training target in a controlled educational environment. No social engineering or denial-of-service testing was performed.

---

## 🎯 Objectives

The assessment focused on:

- Reconnaissance and enumeration of the target web application
- Identifying exposed application paths and resources
- Assessing authentication mechanisms
- Testing input handling and SQL injection
- Validating unauthorized access to protected functionality
- Retrieving the three authorized patient lab-report PDFs
- Performing password-recovery testing on the encrypted PDFs
- Analyzing an exposed database backup
- Identifying staff salary and shareholder information
- Documenting security risks and remediation recommendations

---

## 🛠️ Tools & Technologies

- **Firefox** — Manual web application testing
- **curl** — HTTP request and response analysis
- **Linux CLI utilities** — File and data analysis
- **Python** — Analysis of the exposed database backup
- **NetworkWalks Password Cracker** — PDF password-recovery testing
- **John the Ripper** — Alternative PDF password-recovery testing

---

## 🔎 Methodology

The assessment followed a structured penetration-testing methodology:

```text
Reconnaissance
      ↓
Enumeration
      ↓
Vulnerability Identification
      ↓
Controlled Exploitation
      ↓
Post-Exploitation / Data Analysis
      ↓
Reporting & Remediation
```

---

# 🚨 Key Findings

| ID | Finding | Severity |
|---|---|---|
| F-01 | Sensitive application paths disclosed through `robots.txt` | Low |
| F-02 | Publicly accessible historical database backup | Critical |
| F-03 | SQL injection authentication bypass in Patient Portal | Critical |
| F-04 | Weak password protection on confidential PDF reports | High |

---

## F-01 — Sensitive Paths Disclosed Through robots.txt

The publicly accessible `robots.txt` file disclosed paths including:

```text
/patient/
/staff/
/old/
```

Although `robots.txt` is not an access-control mechanism, exposing sensitive application paths can assist attackers during reconnaissance.

**Severity:** Low

### Recommendation

- Do not rely on `robots.txt` for access control.
- Protect sensitive directories using proper authentication and authorization.
- Avoid unnecessary exposure of administrative or sensitive application paths.

---

## F-02 — Publicly Accessible Database Backup

The `/old/` directory was publicly accessible and exposed a historical SQL database backup:

```text
mediroza_db_backup_2019.sql
```

The backup identified itself as an internal Mediroza General Hospital database backup and contained confidential staff and shareholder records.

Analysis confirmed that the backup contained staff salary information and shareholder ownership information.

The retained analysis identified:

- **29 staff records** in the submitted evidence
- Monthly salary information
- **10 shareholder records**
- Shareholder ownership totaling **100%**

Sensitive personal information from the database is intentionally not reproduced in this repository.

**Severity:** Critical

### Recommendation

- Remove database backups from the public web root.
- Store backups outside web-accessible directories.
- Disable directory listing.
- Apply strict filesystem permissions.
- Encrypt sensitive backups.
- Regularly scan web directories for accidentally exposed backup files.

---

## F-03 — SQL Injection Authentication Bypass

The Patient Portal login endpoint was tested for unsafe input handling.

Controlled testing revealed SQL syntax errors when SQL metacharacters were supplied, indicating unsafe handling of user-controlled input.

A subsequent authorized test using an SQL injection payload successfully bypassed authentication and provided access to the Patient Portal.

The portal exposed three protected patient lab-report downloads.

Sensitive medical information is **not included in this repository**.

**Severity:** Critical

### Recommendation

- Use parameterized queries / prepared statements.
- Never concatenate user input directly into SQL queries.
- Implement secure server-side input validation.
- Return generic authentication errors.
- Implement rate limiting and account protection mechanisms.
- Conduct secure code review and SQL injection testing across application endpoints.

---

## F-04 — Weak Password Protection on Confidential PDFs

The three retrieved lab-report PDFs were protected with passwords.

Password-recovery testing was performed using the **NetworkWalks Password Cracker**.

The first two PDFs were successfully recovered using the NetworkWalks Password Cracker. The third PDF could not be recovered using the same tool, so **John the Ripper** was used as an alternative password-recovery method.

The recovered passwords successfully provided access to the corresponding encrypted PDFs.

For security and privacy reasons, the recovered passwords and medical report contents are **not published in this repository**.

**Severity:** High

### Recommendation

- Do not rely on weak or predictable PDF passwords as the primary access-control mechanism.
- Require proper authentication and authorization before allowing report downloads.
- Use strong, randomly generated passwords when document encryption is required.
- Implement access logging and monitoring for confidential reports.

---

# 📊 Impact Summary

The assessment demonstrated several weaknesses that could affect:

- Confidentiality of employee information
- Confidentiality of shareholder information
- Protection of patient documents
- Authentication controls
- Database backup security
- Overall application security

The combination of an authentication bypass and publicly accessible sensitive backup data significantly increased the potential impact of the identified vulnerabilities.

---

# 🧪 Evidence

The project evidence includes screenshots demonstrating:

1. `robots.txt` path disclosure
2. Public `/old/` directory listing
3. Exposed database backup structure
4. Staff salary analysis
5. Shareholder analysis
6. Patient Portal login
7. Successful authentication bypass
8. NetworkWalks Password Cracker — PDF 1
9. NetworkWalks Password Cracker — PDF 2
10. NetworkWalks Password Cracker / password-recovery evidence for PDF 3

Sensitive patient and employee information has been excluded or redacted where appropriate.

---

# 📑 Report

A complete professional penetration-testing report was prepared containing:

- Executive Summary
- Scope and Objectives
- Methodology
- Tools Used
- Findings
- Evidence
- Risk Ratings
- Business/Security Impact
- Remediation Recommendations
- Limitations
- Conclusion

---

## ⚠️ Ethical & Legal Notice

This project was conducted as part of an **authorized cybersecurity training exercise**.

The techniques demonstrated in this project must only be performed against systems for which explicit authorization has been provided.

Do not reproduce these techniques against real systems without permission.

---

## 👩‍💻 Author

**Rushda Khan**

NetworkWalks Cybersecurity Training  
Batch B083 — Week 4
