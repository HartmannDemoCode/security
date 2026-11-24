# Incidents
| Danish incident | What happened | What students can learn from it |
|---|---|---|
| **DSB train stoppage, 2022** | A cyberattack affected an IT supplier’s test environment. The supplier shut down servers, and train drivers lost access to software they needed to operate trains. Services were disrupted for several hours. ([Euronews][1]) | **Availability and supplier dependencies.** Ask students which external service their application cannot operate without, and what a safe fallback would look like. |
| **Copenhagen Municipality file permissions, 2021** | After a file migration, roughly **37,500 municipal employees** had access to a drive intended for four developers. It held information about up to **3.7 million people**, including sensitive information about children. The access remained open for nearly two months. Available logs showed authorized access, but covered only part of that period. Datatilsynet published its decision in 2024. ([datatilsynet.dk][2]) | **Least privilege and verification.** Ask students to write a post-deployment test that checks whether an ordinary account is denied access to restricted data. |
| **Mærsk and NotPetya, 2017** | The global malware incident disrupted operations at the Danish shipping company. Mærsk’s annual report put the effect on profitability at **US$250–300 million**. ([A.P. Møller - Mærsk A/S][3]) | **Recovery and business impact.** Ask students what they would restore first if their application server and database became unavailable, and how they would prove their backups could be restored. |
| **Attacks on Danish energy companies, 2023** | SektorCERT reported that **22 companies** operating parts of Denmark’s energy infrastructure were compromised. Attackers reached some industrial control systems, and several companies temporarily operated in isolation. ([sektorcert.dk][4]) | **Patching, network segmentation, and detection.** Ask students to map which parts of their deployment are internet-facing, who owns each update, and what should stay isolated if one component is compromised. |
| **Tureby-Alkestrup waterworks, 2024** | Attackers manipulated water pressure through the utility’s control systems. Pipes burst and households temporarily lost their water supply. ([Danske Vandværker][5]) | **Integrity and physical safety.** Ask students where a safety limit should be enforced if a software command can change something in the physical world. |

[1]: https://www.euronews.com/next/2022/11/03/denmark-cybersecurity?utm_source=chatgpt.com "Danish train standstill on Saturday caused by cyber attack"

[2]: https://www.datatilsynet.dk/presse-og-nyheder/nyhedsarkiv/2024/maj/koebenhavns-kommune-faar-alvorlig-kritik-for-manglende-sikkerhedsforanstaltninger?utm_source=chatgpt.com "Københavns Kommune får alvorlig kritik for manglende sikkerhedsforanstaltninger"

[3]: https://investor.maersk.com/da/news-releases/news-release-details/cyber-attack-update?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com "Cyber attack update"

[4]: https://sektorcert.dk/wp-content/uploads/2023/11/SektorCERT-The-attack-against-Danish-critical-infrastructure-TLP-CLEAR.pdf?utm_source=chatgpt.com "Executive summary"

[5]: https://danskevv.dk/nyheder/tureby-alkestrup-deler-erfaringer-paa-ministerbesoeg/?utm_source=chatgpt.com "Tureby-Alkestrup deler erfaringer på ministerbesøg"