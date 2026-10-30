# Web Security Elective: Report Format

## Purpose

The study group investigates how to secure a specific system built on a web stack. Students identify risks, implement selected improvements, and evaluate whether those improvements work.

The shared question is:

- How can we make this system more secure, and what evidence supports our decisions?

## Possible Report Formats

|No| Format | Main focus | Best suited for |
| -- |--- | --- | --- |
| 1. | Security assessment | Identify weaknesses and propose prioritized improvements | Groups with limited access to modify the system |
| 2. | Security improvement case study | Investigate, implement improvements, and compare before and after | Groups working with their own application |
| 3. | Security consultancy report | Explain risks and recommend a realistic action plan for a system owner | Groups practising professional communication |

For 4th semester students, the recommended format is a **security improvement case study**, with a short management summary at the beginning.

## Suggested Report Structure

### 1. Summary for the System Owner

Provide a one-page explanation of the most important risks, the improvements made, and the issues that remain. Write for someone who understands the business but does not necessarily understand security.

### 2. System Description and Scope

- What does the system do, and who uses it?
- What technologies, services, and infrastructure does it depend on?
- What sensitive data and important functionality does it contain?
- Which parts will the you investigate, and which are outside the scope?

Include an architecture diagram showing components, data flows, and trust boundaries.

### 3. Security Goals and Threat Model

Explain what needs protection and from whom. Identify plausible attackers, entry points, and misuse scenarios.

Describe a small number of concrete scenarios, such as:

> A logged-in user attempts to access another user's private data.

### 4. Investigation Method

Describe how you investigated the system: code review, configuration review, manual testing, and automated tools. Explain why each method was selected and its limitations.

Document the authorized test environment so the work is reproducible.

### 5. Findings and Risk Assessment

Use the same structure for every finding:

| Element | What to document |
| --- | --- |
| Finding | A precise description of the weakness |
| Evidence | Relevant requests, code, screenshots, or configuration |
| Attack scenario | How someone could exploit it |
| Impact | What could happen to users or the system |
| Priority | How urgently it should be addressed, with justification |
| Proposed improvement | A concrete change and why it should help |

### 6. Implemented Improvements

For selected findings, explain:

- The original behaviour.
- The change made and why it was chosen.
- Relevant trade-offs, such as usability, complexity, performance, or maintenance.

### 7. Verification and Remaining Risks

Show whether the improvements worked. Include before-and-after evidence and tests that check both the security control and legitimate functionality.

Distinguish between verified results, assumptions, and issues you could not resolve.

### 8. Prioritized Action Plan

Recommend what the system owner should do next:

- Immediate fixes.
- Later improvements.
- Ongoing maintenance.

Include rough effort estimates and dependencies.

### 9. Discussion and Conclusion

Answer the original question. Discuss limitations, alternative solutions, and how your understanding changed during the investigation.

### Appendices

Place detailed tool output, test instructions, and longer code examples in appendices. Keep the evidence needed to understand the conclusions in the main report.

## Milestones

Build the report through short milestones:

1. **System and scope review:** Present the architecture and agree on a manageable investigation.
2. **Threat-model workshop:** Another student challenges the proposed threats and assumptions.
3. **Findings review:** Present evidence and defend the prioritization.
4. **Improvement demonstration:** Demonstrate one attack scenario before and after a fix.
5. **Final report and oral defence:** Explain conclusions and justify decisions.

## Suggested Scope

Investigate **three to five security problems in depth**, with selected improvements implemented and verified. This encourages depth and leaves room for students to choose different security topics.

## Assessment Principle

- Reward the quality of the reasoning, evidence, and evaluation—not the number of vulnerabilities or tools used.
