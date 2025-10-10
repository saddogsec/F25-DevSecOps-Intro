### Task 1 — Static Application Security Testing with Semgrep

- **SAST Tool Effectiveness** - Semgrep's detection capabilities and coverage
Semgrep detected total of 25 flaws. 4 Of which are "ERROR" critical. It has broad coverage and comprehensive explanations why something should be fixed.

- **Critical Vulnerability Analysis** - 5 key SAST findings with file locations and severity levels

| Severity | File                                                       | Line | Rule ID                                                                                     | Message                                                                                                                                                                                                                                                                    |
|----------|------------------------------------------------------------|------|---------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR    | /src/data/static/codefixes/dbSchemaChallenge_1.ts          | 5    | javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection | Detected a sequelize statement that is tainted by user-input. This could lead to SQL injection if the variable is user-controlled and is not properly sanitized. In order to prevent SQL injection, it is recommended to use parameterized queries or prepared statements. |
| ERROR    | /src/data/static/codefixes/dbSchemaChallenge_3.ts          | 11   | javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection | Detected a sequelize statement that is tainted by user-input. This could lead to SQL injection if the variable is user-controlled and is not properly sanitized. In order to prevent SQL injection, it is recommended to use parameterized queries or prepared statements. |
| ERROR    | /src/data/static/codefixes/unionSqlInjectionChallenge_1.ts | 6    | javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection | Detected a sequelize statement that is tainted by user-input. This could lead to SQL injection if the variable is user-controlled and is not properly sanitized. In order to prevent SQL injection, it is recommended to use parameterized queries or prepared statements. |
| ERROR    | /src/data/static/codefixes/unionSqlInjectionChallenge_3.ts | 10   | javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection | Detected a sequelize statement that is tainted by user-input. This could lead to SQL injection if the variable is user-controlled and is not properly sanitized. In order to prevent SQL injection, it is recommended to use parameterized queries or prepared statements. |
| WARNING  | /src/frontend/src/app/navbar/navbar.component.html         | 17   | generic.html-templates.security.unquoted-attribute-var.unquoted-attribute-var               | Detected a unquoted template variable as an attribute. If unquoted, a malicious actor could inject custom JavaScript handlers. To fix this, add quotes around the template expression, like this: "{{ expr }}".                                                            |


### Task 2 — Dynamic Application Security Testing with Multiple Tools

- **Tool Comparison and strengths** - effectiveness comparison of ZAP vs Nuclei vs Nikto vs SQLmap, what each tool excels at detecting in practice
  * Zap has great web app scanning for broad range of vulnerabilities, good output with categorization.
  * Nuclei is very fast detection of known CVE's and misconfigurations.
  * Nikto focuses on web server misconfiguration and file exposure.
  * SQLma: automatically detects and exploits SQL Injections found.
All those tools works better in some specific cases. So should be used alongside each other.

- **DAST Findings** - explain at least 1 significant finding from each tool

    ZAP found cross-domain-misconfiguration, cross-domain js souce inclusion
    Nuclei found DNS rebinding, public swagger api, many missing security headers
    Nikto found some uncommon headers, exposure of /css /ftp /public in robots.txt
    SQLmap managed to exploit SQl with boolean-based-blind and time-based-blind at `GET /rest/products/search?q=apple`

#### 3.1: SAST/DAST Correlation
- **SAST vs DAST Findings** - unique discoveries from each approach with clear differences explained
Both SAST and DAST detected SQL Injections, disclosure risks, some exposure and configuration weaknesses.
Sast warned about some dangerous patterns like use of eval, but this is not directly exploitable.

Overall SAST detects a lot more than DAST, due to access to source, and also it warns about some non-exploitable parts, which are undetected by DAST.

- **Integrated Security Recommendations** - how to use both approaches effectively in a DevSecOps pipeline
SAST works on source code, and can be used before any deployments to find all potential vulnerabilities in code. Use it each PR.
DAST needs deployed version, so should be used depending on project (if it can be deployed in isolate environment, test it here, if not test - on canary deploy (in anyway the worst solution is not to test it at all)
