# PortSwigger Lab Report: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

**Date:** October 16, 2026
**Target:** PortSwigger Web Security Academy (Authorized Lab Environment)
**Tools Used:** Web Browser

## 1. Executive Summary
The application is vulnerable to SQL Injection in the `category` parameter of the product filter. By manipulating this parameter, an attacker can alter the backend SQL query to bypass filtering logic and retrieve data that is not intended to be displayed, such as unreleased or hidden products.

## 2. Root Cause
The application fails to properly sanitize or parameterize user input before including it in a SQL query. The backend query likely resembles:
`SELECT * FROM products WHERE category = 'USER_INPUT' AND released = 1`
By injecting a single quote (`'`) to break out of the string, followed by `OR 1=1--`, the attacker forces the condition to always be true and comments out the `AND released = 1` restriction.

## 3. Steps to Reproduce
1. Navigate to the lab and click on a product category (e.g., "Pets").
2. Observe the request to `/filter?category=Pets`.
3. Modify the `category` parameter to inject the payload: `' OR 1=1--`
4. The full URL becomes: `/filter?category=' OR 1=1--`
5. Send the request.
6. Observe that the response now contains all products, including those that were previously hidden.

## 4. Evidence
*(Note: Insert a sanitized screenshot of the browser showing the hidden products here. Ensure no session cookies or personal tokens are visible).*

**Payload Used:**
`' OR 1=1--`

**Observed Behavior:**
The application returned hidden products that were not supposed to be displayed.

## 5. Impact
An attacker can bypass application logic to retrieve unauthorized data from the database. Depending on the query, this could expose sensitive information, confidential records, or administrative data.

## 6. Remediation Advice
To prevent this vulnerability, the application should use **parameterized queries (prepared statements)**. This ensures that user input is treated strictly as data and not as executable SQL code. Additionally, input validation and the principle of least privilege for database accounts should be implemented.
