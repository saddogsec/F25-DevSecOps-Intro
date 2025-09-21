## Top 5 Risks
Top 5 risks (countint priority as Severity*100 + Likelihood*10 + Impact) were sorted from risks.json:

| Severity | Category | Asset | Likelihood | Impact | Score |
|---|---|---|---|---|---|
| elevated | unencrypted-communication | user-browser | likely | high | 433 |
| elevated | cross-site-scripting | juice-shop | likely | medium | 432 |
| elevated | unencrypted-communication | reverse-proxy | likely | medium | 432 |
| elevated | missing-authentication | juice-shop | likely | medium | 432 |
| medium | cross-site-request-forgery | juice-shop | very-likely | low | 241 |

## Delta Run
For delta run the following changes were made
* User Browser → communication_links → Direct to App (no proxy): set protocol: https
* Reverse Proxy → communication_links: set protocol: https
* Persistent Storage: set encryption: transparent (to represent disk-level encryption)
And different top risks were sorted from risks.json

| Category | Baseline | Secure | Δ |
|---|---|---|---|
| container-baseimage-backdooring | 1 | 1 | 0 |
| cross-site-request-forgery | 2 | 2 | 0 |
| cross-site-scripting | 1 | 1 | 0 |
| missing-authentication | 1 | 1 | 0 |
| missing-authentication-second-factor | 2 | 2 | 0 |
| missing-build-infrastructure | 1 | 1 | 0 |
| missing-hardening | 2 | 2 | 0 |
| missing-identity-store | 1 | 1 | 0 |
| missing-vault | 1 | 1 | 0 |
| missing-waf | 1 | 1 | 0 |
| server-side-request-forgery | 2 | 2 | 0 |
| unencrypted-asset | 2 | 1 | -1 |
| unencrypted-communication | 2 | 0 | -2 |
| unnecessary-data-transfer | 2 | 2 | 0 |
| unnecessary-technical-asset | 2 | 2 | 0 |

THe only changes are 1 less unencrypted-asset case, and 2 less unencrypted-communication cases, which corresponds to changes made in threagile model. (2 changes from http to https, and transparent encryption on disk)
