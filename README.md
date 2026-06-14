# SQL Injection (SQLi) Writeup

---

## Overview

-This writeup documents the identification and analysis of a SQL Injection vulnerability discovered in a controlled lab environment. The goal of this exercise was to understand how improper input handling can affect database security.

## Scope

**Target**: Portswigger labs

**Category**: SQL Injection

**Status**: Verified in controlled environment only

## Methodology

The assessment followed a basic web application testing approach:

-Input parameter analysis

-Error behavior observation

-Response comparison under different inputs

-Manual payload testing

## Findings

The application was found to improperly handle user-supplied input in database queries, leading to potential SQL Injection vulnerability.

Observed indicators:
-Unexpected query behavior

-Input reflection affecting backend responses

-Error-based anomalies (where applicable)

## Impact

If exploited in a real-world scenario, this vulnerability could lead to:

-Unauthorized data access

-Database enumeration

-Authentication bypass

-Potential full database compromise (depending on privileges)

## Recommendations

To mitigate SQL Injection risks:

-Use parameterized queries / prepared statements

-Avoid dynamic SQL string concatenation

-Validate and sanitize all user inputs

-Implement least privilege database accounts

-Disable detailed database error messages in production

## Tools Used

-Browser / Developer Tools

-Burp Suite

-Manual testing techniques

## References

OWASP SQL Injection Guide

Web Application Security Testing Methodologies

[Portswigger](https://portswigger.net)

## Author
GitHub: [Benson Ngugi](https://github.com/bensonngugi1)

Writeup Type: Educational / CTF Lab Report

## Disclaimer

This project was conducted in a controlled environment for educational purposes only. No unauthorized testing was performed against real-world systems.
