# Threat Modeling

## Risks Observed (Top 5)
| Severity | Category | Asset | Likelihood | Impact | Score |
|---|---|---|---|---|---|
| elevated | unencrypted-communication | user-browser | likely | high | 433 |
| elevated | unencrypted-communication | user-browser | likely | high | 433 |
| elevated | cross-site-scripting | juice-shop | likely | medium | 432 |
| elevated | unencrypted-communication | reverse-proxy | likely | medium | 432 |
| elevated | missing-authentication | juice-shop | likely | medium | 432 |

## Delta Run
Changing http in reverse proxy, to https resulted in removal of a single unencrypted-communication 433-scored risk
| Severity | Category | Asset | Likelihood | Impact | Score |
|---|---|---|---|---|---|
| elevated | unencrypted-communication | user-browser | likely | high | 433 |
| elevated | cross-site-scripting | juice-shop | likely | medium | 432 |
| elevated | unencrypted-communication | reverse-proxy | likely | medium | 432 |
| elevated | missing-authentication | juice-shop | likely | medium | 432 |
| medium | cross-site-request-forgery | juice-shop | very-likely | low | 241 |
