# Triage Report — OWASP Juice Shop

## Scope & Asset
- Asset: OWASP Juice Shop (local lab instance)
- Image: bkimminich/juice-shop:latest
- Release link/date: https://hub.docker.com/r/bkimminich/juice-shop/ — 05.09.2025

## Environment
- Host OS: Archlinux (Kernel: Linux 6.16.5-arch1-1)
- Docker: 28.3.3

## Deployment Details
- Run command used: `docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop:19.0.0`
- Access URL: http://127.0.0.1:3000
- Network exposure: 127.0.0.1 only [*] Yes  [ ] No

## Health Check
- Page load: attach screenshot of home page (path or embed)
<img width="1280" height="633" alt="image" src="https://github.com/user-attachments/assets/b2b41cf3-5153-40fb-9967-2feda928f912" />

- API check:
```
<html>
  <head>
    \<meta charset='utf-8'> 
    <title>Error: Unexpected path: /rest/products</title>
    <style>* {
```

## Surface Snapshot (Triage)
- Login/Registration visible: [*] Yes  [ ] No
- Product listing/search present: [*] Yes  [ ] No
- Admin or account area discoverable: [*] Yes  [ ] No — notes: admin at /#/administration, account at /#/profile
- Client-side errors in console: [ ] Yes  [*] No

## Risks Observed (Top 3)
1) SQL Injection Vulnerability- Email login field vulnerable to sql injection
2) Simple account recovery - Account recovery only requires security question, which can be brute-forced or found in internet
3) Data erase doesn't work
