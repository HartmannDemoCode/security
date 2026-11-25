# OWASP (Open Worldwide Application Security Project) TOP 10:2025
1. A01:2025 - Broken Access Control 
  - 3.73% of applications tested had one or more of the 40 Common Weakness Enumerations (CWEs) in this category. As indicated by the dashed line in the above figure, Server-Side Request Forgery (SSRF) has been rolled into this category.
  - 100% of the applications tested were found to have some form of broken access control. Notable CWEs included are CWE-200: Exposure of Sensitive Information to an Unauthorized Actor, CWE-201: Exposure of Sensitive Information Through Sent Data, CWE-918 Server-Side Request Forgery (SSRF), and CWE-352: Cross-Site Request Forgery (CSRF). This category has the highest number of occurrences in the contributed data, and second highest number of related CVEs.
  
2. A02:2025 - Security Misconfiguration
  - 3.00% of the applications tested had one or more of the 16 CWEs in this category. This is not surprising, as software engineering is continuing to increase the amount of an application’s behavior that is based on configurations.
  - 
3. A03:2025 - Software Supply Chain Vulnerabilities
  - This category has 5 CWEs and a limited presence in the collected data, but we believe this is due to challenges in testing and hope that testing catches up in this area. This category has the fewest occurrences in the data, but also the highest average exploit and impact scores from CVEs.
4. A04:2025 - Cryptographic Failures
  -  3.80% of applications have one or more of the 32 CWEs in this category. This category often leads to sensitive data exposure or system compromise.
5. A05:2025 - Injection 
  - Has the greatest number of CVEs associated with the 38 CWEs in this category. Injection includes a range of issues from Cross-site Scripting (high frequency/low impact) to SQL Injection (low frequency/high impact) vulnerabilities.
6. A06:2025 - Insecure Design 
  - we have seen noticeable improvements in the industry related to threat modeling and a greater emphasis on secure design.
7. A07:2025 - Authentication Failures 
  - There are 36 CWEs in this category. This category remains important, but the increased use of standardized frameworks for authentication appears to be having beneficial effects on the occurrences of authentication failures.
8. A08:2025 - Software or Data Integrity Failures 
  - This category is focused on the failure to maintain trust boundaries and verify the integrity of software, code, and data artifacts at a lower level than Software Supply Chain Failures.
9. A09:2025 - Security Logging & Alerting Failures
  - Great logging with no alerting is of minimal value in identifying security incidents. This category will always be underrepresented in the data, and was again voted into a position in the list from the community survey participants.
10. A10:2025 - Mishandling of Exceptional Conditions 
-  is a new category for 2025. This category contains 24 CWEs focusing on improper error handling, logical errors, failing open, and other related scenarios stemming from abnormal conditions that systems may encounter.