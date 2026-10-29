# Securing a Web System — Individual Report structure

**Student:** [Name]  
**System:** [System name]  
**Elective:** [Course name] — 10 ECTS  
**Date:** [Date]  
**Character count:** [Main report, including spaces]

## Suggested Scope and Length

Target **8–12 normal pages**, where one normal page equals **2,400 characters including spaces**: **19,200–28,800 characters** in total.

Choose an existing web system, one or two of the security themes, and some concrete problems within that/those theme(s). Implement and verify the improvements. Existing controls may also be evaluated, so that the project does not depend on discovering a vulnerability.

The report documents the investigation. Reading, implementation, and testing.

## Possible Themes — Choose One

|| Theme | Example investigation question | Possible evidence |
|| --- | --- | --- |
| 1. | Authorization and data ownership | Can a user access or change another user's resources through the API? | Access-rule table and tests with different users and roles |
| 2. | Authentication and sessions | Does the system enforce its intended login, logout, and session-expiry behaviour? | Relevant configuration, request traces, and lifecycle tests |
| 3. | Input handling | How does one selected input path handle malicious or unexpected input? | Data-flow description, relevant code, and targeted tests |
| 4. | Deployment configuration and secrets | Does the deployment expose information or grant permissions beyond what the application requires? | Redacted configuration, environment comparison, and verification |
| 5. | Dependencies | How are dependency risks identified, assessed, and addressed in this application? | Dependency inventory, assessment of relevant advisories, and regression tests |
| 6. | Security logging | Can selected security-relevant events be detected and investigated without exposing sensitive data? | Event requirements, redacted log examples, and detection tests |

---

# Report content suggestions

## 1. Problem, System, and Scope (1 page)


[Briefly describe the application, its users, relevant data, and web stack. Include a small architecture diagram if useful.]

**Research question:** [Write questions that the investigation can answer.]

Example:

- How effectively does the application enforce ownership of private resources, and how can this be improved and verified?

**In scope:** [Identify the components, endpoints, or workflows investigated.]

**Out of scope:** [State important exclusions and explain them briefly.]

**Test environment:** [Describe the authorized environment, application version, and test accounts.]


## 2. Relevant Concepts and Threat Model (2 pages)

[Explain only the security concepts needed to understand this investigation. Connect each concept to the actual application and cite relevant sources.]

- What assets need protection?
- Who might attempt to misuse the selected functionality, and what access do they have?
- Where does data cross a trust boundary?
- What attacks or misuse scenarios will be investigated?

**Expected security behaviour:** [State what the system should allow and prevent.]


## 3. Investigation Method (1 page)

[Explain how code review, configuration review, manual tests, or selected tools will answer the research question. Justify the choices.]

**Reproduction:** [Identify the setup, relevant versions, and location of detailed instructions.]

**Limitations:** [Explain what the method cannot establish.]


## 4. Findings and Risk (2 pages)

### Finding 1: [Precise title]

**Expected behaviour:** [What should happen?]

**Observed behaviour:** [What actually happens?]

**Evidence:** [Include a short request/response example, code excerpt, or other decisive evidence.]

**Scenario and impact:** [Who could exploit the problem, under what conditions, and with what consequences?]

**Priority:** [Justify the importance in this system's context.]

### Finding 2: [Etc.]

[Repeat the same structure only if a second finding helps answer the research question. Report controls that work correctly where relevant; do not invent a weakness.]

## 5. Improvement and Verification (2–3 pages)

### Design decision

[Describe the selected improvement, why it addresses the finding, and any relevant alternative or trade-off.]

### Implementation

[Explain the important changes. Use short code or configuration excerpts and refer to the repository for the full implementation.]

### Verification

[Define success criteria and report the actual test outcomes. Verify both the security behaviour and legitimate use.]

| Test or scenario | Expected result | Before change | After change |
| --- | --- | --- | --- |
| [Previously problematic scenario] | [Expected security behaviour] | [Observed result] | [Observed result] |
| [Legitimate use] | [Expected functionality] | [Observed result] | [Observed result] |
| [Relevant boundary case] | [Expected result] | [Observed result] | [Observed result] |

[Explain what these results establish and what remains unverified.]

## 6. Discussion and Next Steps (1 page)

- What risks remain within the chosen scope?
- What limitations affect confidence in the results?
- What costs or trade-offs does the solution introduce?
- What should the system owner prioritize next, and why?

[Reflect briefly on how evidence changed your initial assumptions.]

## 7. Conclusion (0.5 page)

[Answer the research question directly. State what was established, what improved, and the most important remaining limitation. Do not introduce new findings.]

## References

[List the sources cited in the main report. Include authoritative documentation and relevant security references, with versions or access dates where useful.]

## Appendices

- Reproduction instructions and test setup.
- Extended test results or redacted tool output.
- Supplementary code and configuration excerpts.
- Repository link and commit identifying the evaluated implementation.


## Reflection Checklist

- Does the conclusion answer the research question?
- Is every important finding supported by evidence?
- Is at least one improvement implemented and verified?
- Are legitimate functionality and relevant boundary cases tested?
- Are assumptions and unverified claims clearly identified?
- Can another student reproduce the key investigation steps?
- Does the main report stand on its own without the appendices?
