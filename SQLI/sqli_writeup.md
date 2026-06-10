# SQL Injection (SQLi) Security Assessment Report

---

## 1. Cover Page

### SQL Injection Vulnerability Assessment Report

**Assessment Type:** Web Application Penetration Testing

**Vulnerability Category:** SQL Injection (SQLi)

**Prepared By:** [Enme.Benson Ngugi](https://github.com/bensonngugi1)

**Certification:** Certified Ethical Hacker (CEH)

**Date:** 07/06/2026

**Target Environment:** [Portswigger labs](portswigger.net) and [Altoro Mutual](demo.testfire.net)

**Classification:** Educational / Authorized Security Testing

---

## 2. Executive Summary (Non-Technical)

### 2.1 Overview

This report presents the findings from a security assessment focused on SQL Injection (SQLi) vulnerabilities within multiple web application scenarios provided by the PortSwigger Web Security Academy. The objective was to identify weaknesses that could allow unauthorized access to sensitive information, authentication bypass, database enumeration, and data extraction.

SQL Injection remains one of the most critical web application vulnerabilities due to its potential to compromise the confidentiality, integrity, and availability of organizational data.

---

### 2.2 Key Findings

The assessment identified multiple SQL Injection vulnerabilities including:

- SQL Injection in WHERE clauses
- Authentication bypass via SQL Injection
- Database version disclosure
- Database schema enumeration
- UNION-based SQL Injection
- Error-based SQL Injection
- Blind SQL Injection
- Time-based Blind SQL Injection
- Out-of-Band (OAST) SQL Injection
- Filter bypass through XML encoding

---

### 2.3 Risk Summary

| Risk Level | Number of Findings |
| ---------- | ------------------ |
| Critical | Multiple |
| High | Multiple |
| Medium | Multiple |
| Low | None |

#### Potential Business Impact

- Unauthorized access to sensitive data
- Exposure of customer information
- Credential compromise
- Application takeover
- Reputational damage
- Regulatory compliance violations

---

### 2.4 Recommendations

- Implement parameterized queries and prepared statements.
- Validate and sanitize all user input.
- Enforce least privilege on database accounts.
- Disable verbose database error messages.
- Deploy a Web Application Firewall (WAF).
- Conduct regular penetration testing and code reviews.

---

## 3. Scope and Objectives

### 3.1 Scope

The assessment covered SQL Injection vulnerabilities demonstrated within the PortSwigger Web Security Academy SQL Injection learning environment.

#### Included Components

- Authentication functionality
- Search features
- Product filtering mechanisms
- Database-driven pages
- Error handling functions

---

### 3.2 Objectives

The objectives of this assessment were:

- Identify SQL Injection vulnerabilities.
- Validate exploitability.
- Assess potential impact.
- Demonstrate attack scenarios.
- Recommend mitigation strategies.

---

### 3.3 Rules of Engagement

- Testing was conducted only against authorized lab environments.
- No testing was performed against production systems.
- No persistence mechanisms were established.
- Findings were documented responsibly.

---

## 4. Methodology

The assessment followed a structured penetration testing methodology.

---

### 4.1 Reconnaissance

Activities performed:

- Application mapping
- Input field identification
- Endpoint discovery
- Parameter analysis

#### Evidence

![Input field where a payload can be added](/SQLI/User_input_payload_used_here.png)

---

### 4.2 Scanning and Enumeration

Activities performed:

- Parameter testing
- Database fingerprinting
- Response analysis
- Error identification

#### EvidencE

![Identifying errors returned by database](/SQLI/Error_by_DB.png)

[Source:Altoro Mutual testing page](https://demo.testfire.net/login.jsp)

---

### 4.3 Vulnerability Analysis

Activities performed:

- SQL syntax validation
- Error message analysis
- Boolean testing
- UNION query validation

### Evidence

![False condition i.e 1=2](/SQLI/Boolean_testing.png)


![True condition i.e 1=1](/SQLI/After_SQLI.png)

---

## 4.4 Exploitation

Activities performed:

- Authentication bypass
- Data extraction
- Schema enumeration
- Blind SQL exploitation
- OAST exploitation

### Evidence

![Authetication bypass](/SQLI/Login_bypass.png)

**Payload used**

```
administrator'--

```

---

## 4.5 Reporting

All findings were documented with:

- Severity ratings
- Impact analysis
- Supporting evidence
- Remediation guidance

---

# 5. Tools Used

| Tool | Purpose |
|--------|---------|
| Burp Suite | Proxy, interception, and testing |
| Browser Developer Tools | Request analysis |
| PortSwigger Labs | Vulnerable test environment |
| Burp Collaborator | OAST testing |
| SQL Payloads | Injection testing |

---

# 6. Detailed Findings

The following sections document all identified SQL Injection vulnerabilities.

---

# 7. Vulnerability Findings

---

# 7.1 SQL Injection in WHERE Clause Allowing Retrieval of Hidden Data

## Type

Error-Based SQL Injection

## Description

The application improperly handled user-supplied input within SQL WHERE clauses. By manipulating SQL logic, hidden records could be retrieved from the database.

## Affected Assets

- Product Category Filter
- Product Listing Functionality

## Severity

**Critical**

## Impact

- Unauthorized data access
- Disclosure of hidden records
- Potential information leakage

## Evidence

### Screenshot 1: Initial Application State

![SQLi Hidden Data Before Exploitation](/SQLI/Before_SQLi.png)

### Screenshot 2: Successful Exploitation

![Showing all the data after exploiting](/SQLI/After_SQLI.png)

**Payload used**

```
'OR 1=1--

```

## Root Cause

User input was concatenated directly into SQL statements without validation.

## Remedy

- Use prepared statements.
- Implement parameterized queries.
- Validate user input.

## References

- OWASP SQL Injection Prevention Cheat Sheet

---

# 7.2 SQL Injection Allowing Login Bypass

## Type

Authentication Bypass

## Description

The login functionality failed to sanitize authentication inputs, allowing attackers to bypass login controls.

## Affected Assets

- Authentication Portal

## Severity

**Critical** because an attacker may have the ability to log an administrator.

## Impact

- Unauthorized access
- Privilege escalation

## Evidence

![Login Bypass Screenshot](/SQLI/Login_bypass.png)

```
administrator'--

```

## Root Cause

Improper input handling in authentication queries.

## Remedy

- Parameterized queries
- Secure authentication logic

---

# 7.3 SQL Injection Querying Database Type and Version

## Type

Information Disclosure

## Description

SQL Injection enabled extraction of database type and version information.

## Severity

**High**

## Impact

- Facilitates targeted attacks

## Evidence

![Database Version of Oracle Enumeration](/SQLI/Obtaining_Oracle_version.png)

**Payload Used**

```
' UNION SELECT 'TEST', NULL FROM dual--
```

## Remedy

- Suppress database error output
- Parameterized queries

---

# 7.4 SQL Injection Listing Database Contents (Non-Oracle)

## Type

Database Enumeration

## Severity

**Critical**

## Impact

- Schema disclosure
- Table discovery

## Evidence

![Non Oracle Enumeration](/SQLI/non_oracles.png)

**Payload Used**

```
' UNION SELECT NULL,@@version#

```

---

# 7.5 SQL Injection Listing Database Contents (Oracle)

## Type

Database Enumeration

## Severity

**Critical**

## Impact

- Schema disclosure
- Table discovery

## Evidence

![Oracle Enumeration](/SQLI/Displaying_on_Oracle.png)

---

# 7.6 UNION Attack Determining Number of Columns

## Type

UNION-Based SQL Injection

## Severity

**High**

## Impact

- Enables further exploitation

## Evidence

![Column Enumeration](/SQLI/Determining_number_of_columns.png)

---

# 7.7 UNION Attack Finding a Column Containing Text

## Type

UNION-Based SQL Injection

## Severity

**High**

## Impact

- Identifies injectable columns

## Evidence

![Finding Text Columns](/SQLI/Finding_column_with_text.png)

---

# 7.8 UNION Attack Retrieving Multiple Values in a Single Column

## Type

UNION-Based SQL Injection

## Severity

**Critical**

## Impact

- Sensitive data extraction

## Evidence

![Multiple Values Extraction](/SQLI/Retrieving_multiple_values.png)

---

# 7.9 UNION Attack Retrieving Data from Other Tables

## Type

UNION-Based SQL Injection

## Severity

**Critical**

## Impact

- Cross-table data access

## Evidence

![Cross Table Data Extraction](/SQLI/retrieving_data_from_other_tables.png)

---

# 7.10 Blind SQL Injection with Conditional Responses

## Type

Boolean-Based Blind SQL Injection

## Severity

**High**

## Impact

- Data extraction through application responses

## Evidence



![Conditional Responses ](/SQLI/Blind_sqli_with_conditional_responses.png)

---

# 7.11 Blind SQL Injection with Conditional Errors

## Type

Error-Based Blind SQL Injection

## Severity

**High**

## Impact

- Information disclosure through errors

## Evidence


![Conditional Errors ](/SQLI/Blind_sqli_with_cond_err.png)

---

# 7.12 Visible Error-Based SQL Injection

## Type

Error-Based SQL Injection

## Severity

**High**

## Impact

- Direct database information disclosure

## Evidence

![Visible Error SQLi](images/error-based.png)

---

# 7.13 Blind SQL Injection with Time Delays

## Type

Time-Based Blind SQL Injection

## Severity

**Critical**

## Impact

- Database inference attacks

## Evidence

![Time Delay SQLi](images/time-delay.png)

---

# 7.14 Blind SQL Injection with Time Delays and Information Retrieval

## Type

Time-Based Data Extraction

## Severity

**Critical**

## Impact

- Extraction of sensitive information

## Evidence

![Time Delay Data Extraction](images/time-delay-data.png)

---

# 7.15 Blind SQL Injection with Out-of-Band Interaction

## Type

Out-of-Band SQL Injection

## Severity

**Critical**

## Impact

- External interaction from database server

## Evidence

![OAST Interaction](images/oast-interaction.png)

---

# 7.16 Blind SQL Injection with Out-of-Band Data Exfiltration

## Type

Out-of-Band Data Exfiltration

## Severity

**Critical**

## Impact

- Sensitive data exfiltration

## Evidence

![OAST Exfiltration](images/oast-exfiltration.png)

---

# 7.17 SQL Injection with Filter Bypass via XML Encoding

## Type

Filter Bypass SQL Injection

## Severity

**High**

## Impact

- Circumvention of security controls

## Evidence

![XML Encoding Bypass](images/xml-bypass.png)

---

# 9. Summary

The assessment successfully identified and exploited multiple SQL Injection vulnerabilities across various attack scenarios. The findings demonstrate the severe risks associated with improper input validation and insecure database interactions.

### Key Outcomes

- Authentication bypass achieved
- Database enumeration performed
- Sensitive information extracted
- Blind SQL Injection demonstrated
- Out-of-Band exfiltration validated

---

# 10. Recommendations

## Immediate Actions

- Implement prepared statements.
- Remove dynamic SQL construction.
- Validate all user inputs.
- Restrict database privileges.

## Long-Term Security Improvements

- Adopt Secure SDLC practices.
- Conduct code reviews.
- Provide developer security awareness training.
- Perform regular penetration testing.

---

# 11. Conclusion

The assessment demonstrated that SQL Injection vulnerabilities can result in complete compromise of application data and authentication mechanisms. Implementing secure coding practices and database security controls is essential to mitigate these risks.

---

# 12. Appendix

## Testing Environment

| Component | Description |
|------------|-------------|
| Platform | PortSwigger Academy |
| Browser | [Browser Name] |
| Proxy Tool | Burp Suite |
| Assessment Date | [Date] |

---

# 13. Technical Details

## SQL Injection Categories Covered

1. Error-Based SQL Injection
2. UNION-Based SQL Injection
3. Boolean-Based Blind SQL Injection
4. Time-Based Blind SQL Injection
5. Out-of-Band SQL Injection
6. Authentication Bypass
7. Database Enumeration
8. XML Encoding Filter Bypass

---

# 14. Glossary

| Term | Definition |
|--------|-----------|
| SQLi | SQL Injection |
| DBMS | Database Management System |
| OAST | Out-of-Band Application Security Testing |
| UNION | SQL operator used to combine query results |
| Blind SQLi | SQL Injection without visible output |
| WAF | Web Application Firewall |
| RDBMS | Relational Database Management System |
| CVSS | Common Vulnerability Scoring System |

---

**End of Report**
