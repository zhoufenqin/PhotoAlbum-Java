# Security Assessment Report

**Generated:** 2026-05-26T06:40:36.0000000Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 45 |
| CVE Vulnerabilities | 41 |
| CWE Vulnerabilities | 4 |
| Total Rules Assessed | 59 |
| Rules Passed | 55 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 41 |
| optional | 2 |
| potential | 2 |

### By Category

| Category | Count |
|----------|-------|
| CVE | 41 |
| Code Quality | 1 |
| Credentials & Secrets | 3 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2016-1000027: Pivotal Spring Framework contains unsafe Java deserialization methods
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2016-1000027](https://github.com/advisories/GHSA-4wrc-f8pq-fpqp) — Pivotal Spring Framework contains unsafe Java deserialization methods

Severity: **critical**.

Affected dependencies:
- `org.springframework:spring-web:5.3.31` — affected range `< 6.0.0`; first patched version `6.0.0`

Description: Pivotal Spring Framework before 6.0.0 suffers from a potential remote code execution (RCE) issue if used for Java deserialization of untrusted data. Depending on how the library is implemented within a product, this issue may or not occur, and authentication may be required.  Maintainers recommend investigating alternative components or a potential mitigating control. Version 4.2.6 and 3.2.17 contain [enhanced documentation](https://github.com/spring-projects/spring-framework/commit/5cbe90b2c...

Fix: Upgrade to a fixed version, for example: `6.0.0`.

### CVE-2025-24813: Apache Tomcat: Potential RCE and/or information disclosure and/or information corruption with partial PUT
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2025-24813](https://github.com/advisories/GHSA-83qj-6fr2-vhqg) — Apache Tomcat: Potential RCE and/or information disclosure and/or information corruption with partial PUT

Severity: **critical**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 8.5.0, <= 8.5.100`; first patched version `No patch version listed`

Description: Path Equivalence: 'file.Name' (Internal Dot) leading to Remote Code Execution and/or Information disclosure and/or malicious content added to uploaded files via write enabled Default Servlet in Apache Tomcat.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.2, from 10.1.0-M1 through 10.1.34, from 9.0.0.M1 through 9.0.98. The following versions were EOL at the time the CVE was created but are known to be affected: 8.5.0 though 8.5.100. Other, older, EOL versions may also be affec...

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2026-40477: Improper restriction of the scope of accessible objects in Thymeleaf expressions
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:40

[CVE-2026-40477](https://github.com/advisories/GHSA-r4v4-5mwr-2fwr) — Improper restriction of the scope of accessible objects in Thymeleaf expressions

Severity: **critical**.

Affected dependencies:
- `org.thymeleaf:thymeleaf-spring5:3.0.15.RELEASE` — affected range `<= 3.1.3.RELEASE`; first patched version `3.1.4.RELEASE`
- `org.thymeleaf:thymeleaf:3.0.15.RELEASE` — affected range `<= 3.1.3.RELEASE`; first patched version `3.1.4.RELEASE`

Description: ### Impact A security bypass vulnerability exists in the expression execution mechanisms of Thymeleaf up to and including 3.1.3.RELEASE. Although the library provides mechanisms to prevent expression injection, it fails to properly restrict the scope of accessible objects, allowing specific potentially sensitive objects to be reached from within a template. If an application developer passes unvalidated user input directly to the template engine, an unauthenticated remote attacker can bypass ...

Fix: Upgrade to a fixed version, for example: `3.1.4.RELEASE`.

### CVE-2026-40478: Improper neutralization of specific syntax patterns for unauthorized expressions in Thymeleaf
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:40

[CVE-2026-40478](https://github.com/advisories/GHSA-xjw8-8c5c-9r79) — Improper neutralization of specific syntax patterns for unauthorized expressions in Thymeleaf

Severity: **critical**.

Affected dependencies:
- `org.thymeleaf:thymeleaf-spring5:3.0.15.RELEASE` — affected range `<= 3.1.3.RELEASE`; first patched version `3.1.4.RELEASE`
- `org.thymeleaf:thymeleaf:3.0.15.RELEASE` — affected range `<= 3.1.3.RELEASE`; first patched version `3.1.4.RELEASE`

Description: ### Impact A security bypass vulnerability exists in the expression execution mechanisms of Thymeleaf up to and including 3.1.3.RELEASE. Although the library provides mechanisms to prevent expression injection, it fails to properly neutralize specific syntax patterns that allow for the execution of unauthorized expressions. If an application developer passes unvalidated user input directly to the template engine, an unauthenticated remote attacker can bypass the library's protections to achie...

Fix: Upgrade to a fixed version, for example: `3.1.4.RELEASE`.

### CVE-2026-41293: Apache Tomcat - HTTP/2 request headers not validated
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-41293](https://github.com/advisories/GHSA-r29c-68gh-xp6x) — Apache Tomcat - HTTP/2 request headers not validated

Severity: **critical**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.22`; first patched version `11.0.22`

Description: Versions Affected: Apache Tomcat 11.0.0-M1 to 11.0.21 Apache Tomcat 10.1.0-M1 to 10.1.54 Apache Tomcat 9.0.0.M1 to 9.0.117 Older, unsupported versions may also be affected  Description: HTTP/2 request headers were not validated which may have triggered unexpected application behaviour if the application (quite reasonably) assumed that header value exposed through the Servlet API would be specification compliant.  Mitigation: Users of the affected versions should apply one of the following mit...

Fix: Upgrade to a fixed version, for example: `11.0.22`.

### CVE-2026-41901: Sandboxed Thymeleaf expressions vulnerable to improper recognition of unauthorized syntax patterns
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:40

[CVE-2026-41901](https://github.com/advisories/GHSA-c9ph-gxww-7744) — Sandboxed Thymeleaf expressions vulnerable to improper recognition of unauthorized syntax patterns

Severity: **critical**.

Affected dependencies:
- `org.thymeleaf:thymeleaf-spring5:3.0.15.RELEASE` — affected range `<= 3.1.4.RELEASE`; first patched version `3.1.5.RELEASE`
- `org.thymeleaf:thymeleaf:3.0.15.RELEASE` — affected range `<= 3.1.4.RELEASE`; first patched version `3.1.5.RELEASE`

Description: ### Impact  A security bypass vulnerability exists in the expression execution mechanisms of Thymeleaf up to and including 3.1.4.RELEASE. Although the library provides mechanisms to avoid the execution of potentially dangerous expressions in some specific sandboxed (restricted) contexts, it fails to properly neutralize specific constructs that allow this kind of expressions to be executed. If an application developer passes to the template engine unsanitized variables that contain such expres...

Fix: Upgrade to a fixed version, for example: `3.1.5.RELEASE`.

### CVE-2026-43512: Apache Tomcat - Digest authenticator will authenticate any unknown user
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-43512](https://github.com/advisories/GHSA-h6fc-48rj-7qqh) — Apache Tomcat - Digest authenticator will authenticate any unknown user

Severity: **critical**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.22`; first patched version `11.0.22`

Description: Versions Affected: Apache Tomcat 11.0.0-M1 to 11.0.21 Apache Tomcat 10.1.0-M1 to 10.1.54 Apache Tomcat 9.0.0.M1 to 9.0.117 Older, unsupported versions may also be affected  Description: When DIGEST authentication was configured, any user not known to the configured Realm would be authenticated if they presented the password "null".  Mitigation: Users of the affected versions should apply one of the following mitigations: - Upgrade to Apache Tomcat 11.0.22 or later - Upgrade to Apache Tomcat 1...

Fix: Upgrade to a fixed version, for example: `11.0.22`.

### CVE-2026-43515: Apache Tomcat - Security constraints not correctly applied
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-43515](https://github.com/advisories/GHSA-5m62-pw8w-7w9f) — Apache Tomcat - Security constraints not correctly applied

Severity: **critical**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.22`; first patched version `11.0.22`

Description: Versions Affected: Apache Tomcat 11.0.0-M1 to 11.0.21 Apache Tomcat 10.1.0-M1 to 10.1.54 Apache Tomcat 9.0.0.M1 to 9.0.117 Older, unsupported versions may also be affected  Description: When multiple security constraints defined an HTTP method constraint for the same extension pattern, only the first method constraint was applied.  Mitigation: Users of the affected versions should apply one of the following mitigations: - Upgrade to Apache Tomcat 11.0.22 or later - Upgrade to Apache Tomcat 10...

Fix: Upgrade to a fixed version, for example: `11.0.22`.

### CVE-2022-1471: SnakeYaml Constructor Deserialization Remote Code Execution
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2022-1471](https://github.com/advisories/GHSA-mjmj-j48q-9wg2) — SnakeYaml Constructor Deserialization Remote Code Execution

Severity: **high**.

Affected dependencies:
- `org.yaml:snakeyaml:1.30` — affected range `<= 1.33`; first patched version `2.0`

Description: ### Summary SnakeYaml's `Constructor` class, which inherits from `SafeConstructor`, allows any type be deserialized given the following line:  new Yaml(new Constructor(TestDataClass.class)).load(yamlContent);  Types do not have to match the types of properties in the target class. A `ConstructorException` is thrown, but only after a malicious payload is deserialized.  ### Severity High, lack of type checks during deserialization allows remote code execution.  ### Proof of Concept Execute `bas...

Fix: Upgrade to a fixed version, for example: `2.0`.

### CVE-2022-25857: Uncontrolled Resource Consumption in snakeyaml
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2022-25857](https://github.com/advisories/GHSA-3mc7-4q67-w48m) — Uncontrolled Resource Consumption in snakeyaml

Severity: **high**.

Affected dependencies:
- `org.yaml:snakeyaml:1.30` — affected range `< 1.31`; first patched version `1.31`

Description: The package org.yaml:snakeyaml from 0 and before 1.31 are vulnerable to Denial of Service (DoS) due missing to nested depth limitation for collections.

Fix: Upgrade to a fixed version, for example: `1.31`.

### CVE-2022-45868: Password exposure in H2 Database 
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:85

[CVE-2022-45868](https://github.com/advisories/GHSA-22wj-vf5f-wrvj) — Password exposure in H2 Database 

Severity: **high**.

Affected dependencies:
- `com.h2database:h2:2.1.214` — affected range `>= 1.4.198, < 2.2.220`; first patched version `2.2.220`

Description: The web-based admin console in H2 Database Engine through 2.1.214 can be started via the CLI with the argument -webAdminPassword, which allows the user to specify the password in cleartext for the web admin console. Consequently, a local user (or an attacker that has obtained local access through some means) would be able to discover the password by listing processes and their arguments. NOTE: the vendor states "This is not a vulnerability of H2 Console ... Passwords should never be passed on...

Fix: Upgrade to a fixed version, for example: `2.2.220`.

### CVE-2023-6378: logback serialization vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2023-6378](https://github.com/advisories/GHSA-vmq6-5m68-f53m) — logback serialization vulnerability

Severity: **high**.

Affected dependencies:
- `ch.qos.logback:logback-classic:1.2.12` — affected range `< 1.2.13`; first patched version `1.2.13`
- `ch.qos.logback:logback-core:1.2.12` — affected range `< 1.2.13`; first patched version `1.2.13`

Description: A serialization vulnerability in logback receiver component part of logback allows an attacker to mount a Denial-Of-Service attack by sending poisoned data.  This is only exploitable if logback receiver component is deployed. See https://logback.qos.ch/manual/receivers.html

Fix: Upgrade to a fixed version, for example: `1.2.13`.

### CVE-2023-6481: Logback is vulnerable to an attacker mounting a Denial-Of-Service attack by sending poisoned data
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2023-6481](https://github.com/advisories/GHSA-gm62-rw4g-vrc4) — Logback is vulnerable to an attacker mounting a Denial-Of-Service attack by sending poisoned data

Severity: **high**.

Affected dependencies:
- `ch.qos.logback:logback-core:1.2.12` — affected range `= 1.2.12`; first patched version `1.2.13`

Description: A serialization vulnerability in logback receiver component part of logback version 1.4.13, 1.3.13 and 1.2.12 allows an attacker to mount a Denial-Of-Service attack by sending poisoned data.

Fix: Upgrade to a fixed version, for example: `1.2.13`.

### CVE-2024-22243: Spring Web vulnerable to Open Redirect or Server Side Request Forgery
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2024-22243](https://github.com/advisories/GHSA-ccgv-vj62-xf9h) — Spring Web vulnerable to Open Redirect or Server Side Request Forgery

Severity: **high**.

Affected dependencies:
- `org.springframework:spring-web:5.3.31` — affected range `<= 5.2.25.RELEASE`; first patched version `No patch version listed`

Description: Applications that use UriComponentsBuilder to parse an externally provided URL (e.g. through a query parameter) AND perform validation checks on the host of the parsed URL may be vulnerable to a  open redirect attack or to a SSRF attack if the URL is used after passing validation checks.

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2024-22259: Spring Framework URL Parsing with Host Validation Vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2024-22259](https://github.com/advisories/GHSA-hgjh-9rj2-g67j) — Spring Framework URL Parsing with Host Validation Vulnerability

Severity: **high**.

Affected dependencies:
- `org.springframework:spring-web:5.3.31` — affected range `< 5.3.33`; first patched version `5.3.33`

Description: Applications that use UriComponentsBuilder in Spring Framework to parse an externally provided URL (e.g. through a query parameter) AND perform validation checks on the host of the parsed URL may be vulnerable to a  open redirect https://cwe.mitre.org/data/definitions/601.html  attack or to a SSRF attack if the URL is used after passing validation checks.  This is the same as  CVE-2024-22243 https://spring.io/security/cve-2024-22243, but with different input.

Fix: Upgrade to a fixed version, for example: `5.3.33`.

### CVE-2024-22262: Spring Framework URL Parsing with Host Validation
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2024-22262](https://github.com/advisories/GHSA-2wrp-6fg6-hmc5) — Spring Framework URL Parsing with Host Validation

Severity: **high**.

Affected dependencies:
- `org.springframework:spring-web:5.3.31` — affected range `>= 6.1.0, < 6.1.6`; first patched version `6.1.6`

Description: Applications that use UriComponentsBuilder to parse an externally provided URL (e.g. through a query parameter) AND perform validation checks on the host of the parsed URL may be vulnerable to a  open redirect https://cwe.mitre.org/data/definitions/601.html  attack or to a SSRF attack if the URL is used after passing validation checks.  This is the same as  CVE-2024-22259 https://spring.io/security/cve-2024-22259  and  CVE-2024-22243 https://spring.io/security/cve-2024-22243 , but with differ...

Fix: Upgrade to a fixed version, for example: `6.1.6`.

### CVE-2024-34750: Apache Tomcat - Denial of Service
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2024-34750](https://github.com/advisories/GHSA-wm9w-rjj3-j356) — Apache Tomcat - Denial of Service

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 8.5.0, <= 8.5.100`; first patched version `No patch version listed`

Description: Improper Handling of Exceptional Conditions, Uncontrolled Resource Consumption vulnerability in Apache Tomcat. When processing an HTTP/2 stream, Tomcat did not handle some cases of excessive HTTP headers correctly. This led to a miscounting of active HTTP/2 streams which in turn led to the use of an incorrect infinite timeout which allowed connections to remain open which should have been closed.   This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.0-M20, from 10.1.0-M1 through 10....

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2024-38816: Path traversal vulnerability in functional web frameworks
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2024-38816](https://github.com/advisories/GHSA-cx7f-g6mp-7hqm) — Path traversal vulnerability in functional web frameworks

Severity: **high**.

Affected dependencies:
- `org.springframework:spring-webmvc:5.3.31` — affected range `>= 5.3.0, <= 5.3.39`; first patched version `No patch version listed`

Description: Applications serving static resources through the functional web frameworks WebMvc.fn or WebFlux.fn are vulnerable to path traversal attacks. An attacker can craft malicious HTTP requests and obtain any file on the file system that is also accessible to the process in which the Spring application is running.  Specifically, an application is vulnerable when both of the following are true:    *  the web application uses RouterFunctions to serve static resources   *  resource handling is explici...

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2024-38819: Spring Framework Path Traversal vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2024-38819](https://github.com/advisories/GHSA-g5vr-rgqm-vf78) — Spring Framework Path Traversal vulnerability

Severity: **high**.

Affected dependencies:
- `org.springframework:spring-webmvc:5.3.31` — affected range `>= 6.0.0, <= 6.0.23`; first patched version `No patch version listed`

Description: Applications serving static resources through the functional web frameworks WebMvc.fn or WebFlux.fn are vulnerable to path traversal attacks. An attacker can craft malicious HTTP requests and obtain any file on the file system that is also accessible to the process in which the Spring application is running.

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2024-47554: Apache Commons IO: Possible denial of service attack on untrusted input to XmlStreamReader
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:65

[CVE-2024-47554](https://github.com/advisories/GHSA-78wr-2p64-hpwj) — Apache Commons IO: Possible denial of service attack on untrusted input to XmlStreamReader

Severity: **high**.

Affected dependencies:
- `commons-io:commons-io:2.11.0` — affected range `>= 2.0, < 2.14.0`; first patched version `2.14.0`

Description: Uncontrolled Resource Consumption vulnerability in Apache Commons IO.  The `org.apache.commons.io.input.XmlStreamReader` class may excessively consume CPU resources when processing maliciously crafted input.   This issue affects Apache Commons IO: from 2.0 before 2.14.0.  Users are recommended to upgrade to version 2.14.0 or later, which fixes the issue.

Fix: Upgrade to a fixed version, for example: `2.14.0`.

### CVE-2024-50379: Apache Tomcat Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2024-50379](https://github.com/advisories/GHSA-5j33-cvvr-w245) — Apache Tomcat Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 8.5.0, <= 8.5.100`; first patched version `No patch version listed`

Description: Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability during JSP compilation in Apache Tomcat permits an RCE on case insensitive file systems when the default servlet is enabled for write (non-default configuration).  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.1, from 10.1.0-M1 through 10.1.33, from 9.0.0.M1 through 9.0.97. The following versions were EOL at the time the CVE was created but are known to be affected: 8.5.0 though 8.5.100. Other, older, EOL versions...

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2024-56337: Apache Tomcat Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2024-56337](https://github.com/advisories/GHSA-27hp-xhwr-wr2m) — Apache Tomcat Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 9.0.0.M1, < 9.0.98`; first patched version `9.0.98`

Description: Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability in Apache Tomcat.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.1, from 10.1.0-M1 through 10.1.33, from 9.0.0.M1 through 9.0.97.  The mitigation for CVE-2024-50379 was incomplete.  Users running Tomcat on a case insensitive file system with the default servlet write enabled (readonly initialisation  parameter set to the non-default value of false) may need additional configuration to fully mitigate CVE-2024-50379...

Fix: Upgrade to a fixed version, for example: `9.0.98`.

### CVE-2025-22235: Spring Boot EndpointRequest.to() creates wrong matcher if actuator endpoint is not exposed
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:92

[CVE-2025-22235](https://github.com/advisories/GHSA-rc42-6c7j-7h5r) — Spring Boot EndpointRequest.to() creates wrong matcher if actuator endpoint is not exposed

Severity: **high**.

Affected dependencies:
- `org.springframework.boot:spring-boot:2.7.18` — affected range `>= 3.4.0, <= 3.4.4`; first patched version `3.4.5`

Description: EndpointRequest.to() creates a matcher for null/** if the actuator endpoint, for which the EndpointRequest has been created, is disabled or not exposed.  Your application may be affected by this if all the following conditions are met:    *  You use Spring Security   *  EndpointRequest.to() has been used in a Spring Security chain configuration   *  The endpoint which EndpointRequest references is disabled or not exposed via web   *  Your application handles requests to /null and this path ne...

Fix: Upgrade to a fixed version, for example: `3.4.5`.

### CVE-2025-41249: Spring Framework annotation detection mechanism may result in improper authorization
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:78

[CVE-2025-41249](https://github.com/advisories/GHSA-jmp9-x22r-554x) — Spring Framework annotation detection mechanism may result in improper authorization

Severity: **high**.

Affected dependencies:
- `org.springframework:spring-core:5.3.31` — affected range `>= 6.2.0, <= 6.2.10`; first patched version `6.2.11`

Description: The Spring Framework annotation detection mechanism may not correctly resolve annotations on methods within type hierarchies with a parameterized super type with unbounded generics. This can be an issue if such annotations are used for authorization decisions.  Your application may be affected by this if you are using Spring Security's @EnableMethodSecurity feature.  You are not affected by this if you are not using @EnableMethodSecurity or if you do not use security annotations on methods in...

Fix: Upgrade to a fixed version, for example: `6.2.11`.

### CVE-2025-48988: Apache Tomcat - DoS in multipart upload
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2025-48988](https://github.com/advisories/GHSA-h3gc-qfqq-6h8f) — Apache Tomcat - DoS in multipart upload

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 8.5.0, <= 8.5.100`; first patched version `No patch version listed`

Description: Allocation of Resources Without Limits or Throttling vulnerability in Apache Tomcat.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.7, from 10.1.0-M1 through 10.1.41, from 9.0.0.M1 through 9.0.105. The following versions were EOL at the time the CVE was created but are known to be affected: 8.5.0 though 8.5.100. Other, older, EOL versions may also be affected.  Users are recommended to upgrade to version 11.0.8, 10.1.42 or 9.0.106, which fix the issue.

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2025-48989: Apache Tomcat Improper Resource Shutdown or Release vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2025-48989](https://github.com/advisories/GHSA-gqp3-2cvr-x8m3) — Apache Tomcat Improper Resource Shutdown or Release vulnerability

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 9.0.0.M1, < 9.0.108`; first patched version `9.0.108`

Description: Improper Resource Shutdown or Release vulnerability in Apache Tomcat made Tomcat vulnerable to the made you reset attack.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.9, from 10.1.0-M1 through 10.1.43 and from 9.0.0.M1 through 9.0.107. Older, EOL versions may also be affected.  Users are recommended to upgrade to one of versions 11.0.10, 10.1.44 or 9.0.108 which fix the issue.

Fix: Upgrade to a fixed version, for example: `9.0.108`.

### CVE-2025-52520: Apache Tomcat Catalina is vulnerable to DoS attack through bypassing of size limits
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2025-52520](https://github.com/advisories/GHSA-wr62-c79q-cv37) — Apache Tomcat Catalina is vulnerable to DoS attack through bypassing of size limits

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 8.5.0, <= 8.5.100`; first patched version `No patch version listed`

Description: For some unlikely configurations of multipart upload, an Integer Overflow vulnerability in Apache Tomcat could lead to a DoS via bypassing of size limits.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.8, from 10.1.0-M1 through 10.1.42, from 9.0.0.M1 through 9.0.106. The following versions were EOL at the time the CVE was created but are known to be affected: 8.5.0 through 8.5.100. Other, older, EOL versions may also be affected.  Users are recommended to upgrade to version 11...

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2025-52999: jackson-core can throw a StackoverflowError when processing deeply nested data
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:72

[CVE-2025-52999](https://github.com/advisories/GHSA-h46c-h94j-95f3) — jackson-core can throw a StackoverflowError when processing deeply nested data

Severity: **high**.

Affected dependencies:
- `com.fasterxml.jackson.core:jackson-core:2.13.5` — affected range `< 2.15.0`; first patched version `2.15.0`

Description: ### Impact With older versions  of jackson-core, if you parse an input file and it has deeply nested data, Jackson could end up throwing a StackoverflowError if the depth is particularly large.  ### Patches jackson-core 2.15.0 contains a configurable limit for how deep Jackson will traverse in an input document, defaulting to an allowable depth of 1000. Change is in https://github.com/FasterXML/jackson-core/pull/943. jackson-core will throw a StreamConstraintsException if the limit is reached...

Fix: Upgrade to a fixed version, for example: `2.15.0`.

### CVE-2025-53506: Apache Tomcat Coyote vulnerable to Denial of Service via excessive HTTP/2 streams
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2025-53506](https://github.com/advisories/GHSA-25xr-qj8w-c4vf) — Apache Tomcat Coyote vulnerable to Denial of Service via excessive HTTP/2 streams

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.9`; first patched version `11.0.9`

Description: Uncontrolled Resource Consumption vulnerability in Apache Tomcat if an HTTP/2 client did not acknowledge the initial settings frame that reduces the maximum permitted concurrent streams.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.8, from 10.1.0-M1 through 10.1.42, from 9.0.0.M1 through 9.0.106. The following versions were EOL at the time the CVE was created but are known to be affected: 8.5.0 through 8.5.100.  Users are recommended to upgrade to version 11.0.9, 10.1.43 or ...

Fix: Upgrade to a fixed version, for example: `11.0.9`.

### CVE-2025-55752: Apache Tomcat Vulnerable to Relative Path Traversal
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2025-55752](https://github.com/advisories/GHSA-wmwf-9ccg-fff5) — Apache Tomcat Vulnerable to Relative Path Traversal

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 8.5.6, <= 8.5.100`; first patched version `No patch version listed`

Description: The fix for bug 60013 introduced a regression where the rewritten URL was normalized before it was decoded. This introduced the possibility that, for rewrite rules that rewrite query parameters to the URL, an attacker could manipulate the request URI to bypass security constraints including the protection for /WEB-INF/ and /META-INF/. If PUT requests were also enabled then malicious files could be uploaded leading to remote code execution. PUT requests are normally limited to trusted users an...

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2026-0603: Hibernate vulnerable to SQL Injection
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:46

[CVE-2026-0603](https://github.com/advisories/GHSA-2p5w-cvg5-gc5c) — Hibernate vulnerable to SQL Injection

Severity: **high**.

Affected dependencies:
- `org.hibernate:hibernate-core:5.6.15.Final` — affected range `>= 5.2.8, <= 5.6.15`; first patched version `No patch version listed`

Description: A flaw was found in Hibernate. A remote attacker with low privileges could exploit a second-order SQL injection vulnerability by providing specially crafted, unsanitized non-alphanumeric characters in the ID column when the InlineIdsOrClauseBuilder is used. This could lead to sensitive information disclosure, such as reading system files, and allow for data manipulation or deletion within the application's database, resulting in an application level denial of service.

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2026-24400: AssertJ has XML External Entity (XXE) vulnerability when parsing untrusted XML via isXmlEqualTo assertion
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:78

[CVE-2026-24400](https://github.com/advisories/GHSA-rqfh-9r24-8c9r) — AssertJ has XML External Entity (XXE) vulnerability when parsing untrusted XML via isXmlEqualTo assertion

Severity: **high**.

Affected dependencies:
- `org.assertj:assertj-core:3.22.0` — affected range `>= 1.4.0, <= 3.27.6`; first patched version `3.27.7`

Description: An XML External Entity (XXE) vulnerability exists in `org.assertj.core.util.xml.XmlStringPrettyFormatter`: the `toXmlDocument(String)` method initializes `DocumentBuilderFactory` with default settings, without disabling DTDs or external entities. This formatter is used by the `isXmlEqualTo(CharSequence)` assertion for `CharSequence` values.  An application is vulnerable only when it uses untrusted XML input with one of the following methods:  - `isXmlEqualTo(CharSequence)` from `org.assertj.c...

Fix: Upgrade to a fixed version, for example: `3.27.7`.

### CVE-2026-24734: Apache Tomcat has an Improper Input Validation vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-24734](https://github.com/advisories/GHSA-mgp5-rv84-w37q) — Apache Tomcat has an Improper Input Validation vulnerability

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 9.0.83, < 9.0.115`; first patched version `9.0.115`

Description: Improper Input Validation vulnerability in Apache Tomcat Native, Apache Tomcat.  When using an OCSP responder, Tomcat Native (and Tomcat's FFM port of the Tomcat Native code) did not complete verification or freshness checks on the OCSP response which could allow certificate revocation to be bypassed.  This issue affects Apache Tomcat Native:  from 1.3.0 through 1.3.4, from 2.0.0 through 2.0.11; Apache Tomcat: from 11.0.0-M1 through 11.0.17, from 10.1.0-M7 through 10.1.51, from 9.0.83 through...

Fix: Upgrade to a fixed version, for example: `9.0.115`.

### CVE-2026-24880: Apache Tomcat has an HTTP Request/Response Smuggling vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-24880](https://github.com/advisories/GHSA-563x-q5rq-57qp) — Apache Tomcat has an HTTP Request/Response Smuggling vulnerability

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, <= 11.0.18`; first patched version `11.0.20`

Description: Inconsistent Interpretation of HTTP Requests ('HTTP Request/Response Smuggling') vulnerability in Apache Tomcat via invalid chunk extension.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.18, from 10.1.0-M1 through 10.1.52, from 9.0.0.M1 through 9.0.115, from 8.5.0 through 8.5.100, from 7.0.0 through 7.0.109. Other, unsupported versions may also be affected.  Users are recommended to upgrade to version 11.0.20, 10.1.52 or 9.0.116, which fix the issue.

Fix: Upgrade to a fixed version, for example: `11.0.20`.

### CVE-2026-34483: Apache Tomcat has an Improper Encoding or Escaping of Output vulnerability in the JsonAccessLogValve
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-34483](https://github.com/advisories/GHSA-rv64-5gf8-9qq8) — Apache Tomcat has an Improper Encoding or Escaping of Output vulnerability in the JsonAccessLogValve

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.21`; first patched version `11.0.21`

Description: Improper Encoding or Escaping of Output vulnerability in the JsonAccessLogValve component of Apache Tomcat.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.20, from 10.1.0-M1 through 10.1.53, from 9.0.40 through 9.0.116.  Users are recommended to upgrade to version 11.0.21, 10.1.54 or 9.0.117 , which fix the issue.

Fix: Upgrade to a fixed version, for example: `11.0.21`.

### CVE-2026-34487: Apache Tomcat vulnerable to Insertion of Sensitive Information into Log File
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-34487](https://github.com/advisories/GHSA-x4m4-345f-5h5g) — Apache Tomcat vulnerable to Insertion of Sensitive Information into Log File

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.21`; first patched version `11.0.21`

Description: Insertion of Sensitive Information into Log File vulnerability in the cloud membership for clustering component of Apache Tomcat exposed the Kubernetes bearer token.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.20, from 10.1.0-M1 through 10.1.53, from 9.0.13 through 9.0.116.  Users are recommended to upgrade to version 11.0.21, 10.1.54 or 9.0.117, which fix the issue.

Fix: Upgrade to a fixed version, for example: `11.0.21`.

### CVE-2026-40972: Spring Boot DevTools remote secret comparison is vulnerable to timing attacks
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:92

[CVE-2026-40972](https://github.com/advisories/GHSA-56v8-86gj-66jp) — Spring Boot DevTools remote secret comparison is vulnerable to timing attacks

Severity: **high**.

Affected dependencies:
- `org.springframework.boot:spring-boot-devtools:2.7.18` — affected range `<= 2.7.32`; first patched version `No patch version listed`

Description: An attacker on the same network as the remote application may be able to utilize a timing attack to discover information about the remote secret. In extreme circumstances this could result in the attacker determining the secret and uploading changed classes, thereby achieving remote code execution in the remote application.  Affected: Spring Boot 4.0.0–4.0.5 (fix 4.0.6), 3.5.0–3.5.13 (fix 3.5.14), 3.4.0–3.4.15 (fix 3.4.16), 3.3.0–3.3.18 (fix 3.3.19), 2.7.0–2.7.32 (fix 2.7.33); DevTools remote...

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2026-40973: Spring Boot accepts predictable temp directory without ownership verification
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:92

[CVE-2026-40973](https://github.com/advisories/GHSA-wwpq-f5c3-7hvx) — Spring Boot accepts predictable temp directory without ownership verification

Severity: **high**.

Affected dependencies:
- `org.springframework.boot:spring-boot:2.7.18` — affected range `<= 2.7.32`; first patched version `No patch version listed`

Description: A local attacker on the same host as the application may be able to take control of the directory used by `ApplicationTemp`. When `server.servlet.session.persistent` is set to `true` and the attack persists across application restarts, this may allow the attacker to read session information and hijack authenticated users or deploy a gadget chain and execute code as the application's user.  Affected: Spring Boot 4.0.0–4.0.5 (fix 4.0.6), 3.5.0–3.5.13 (fix 3.5.14), 3.4.0–3.4.15 (fix 3.4.16), 3.3...

Fix: Upgrade to a fixed version, for example: `No patch version listed`.

### CVE-2026-41284: Apache Tomcat: Unbounded read in WebDAV LOCK and  PROPFIND handling
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-41284](https://github.com/advisories/GHSA-gx5v-xp9w-j4cg) — Apache Tomcat: Unbounded read in WebDAV LOCK and  PROPFIND handling

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.22`; first patched version `11.0.22`

Description: Versions Affected: Apache Tomcat 11.0.0-M1 to 11.0.21 Apache Tomcat 10.1.0-M1 to 10.1.54 Apache Tomcat 9.0.0.M1 to 9.0.117 Older, unsupported versions may also be affected  Description: No limit was enforced on the request body for WebDAV LOCK or PROPFIND requests which were available to unauthenticated users.  Mitigation: Users of the affected versions should apply one of the following mitigations: - Upgrade to Apache Tomcat 11.0.22 or later - Upgrade to Apache Tomcat 10.1.55 or later - Upgr...

Fix: Upgrade to a fixed version, for example: `11.0.22`.

### CVE-2026-42498: Apache Tomcat - WebSocket authentication header exposure
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-42498](https://github.com/advisories/GHSA-fv25-8xcx-gqjc) — Apache Tomcat - WebSocket authentication header exposure

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.22`; first patched version `11.0.22`

Description: Versions Affected: Apache Tomcat 11.0.0-M1 to 11.0.21 Apache Tomcat 10.1.0-M1 to 10.1.54 Apache Tomcat 9.0.2 to 9.0.117 Older, unsupported versions may also be affected  Description: If a WebSocket request was redirected after authentication, Tomcat's WebSocket client would present the most recent authentication header to the redirect target host.  Mitigation: Users of the affected versions should apply one of the following mitigations: - Upgrade to Apache Tomcat 11.0.22 or later - Upgrade to...

Fix: Upgrade to a fixed version, for example: `11.0.22`.

### CVE-2026-43513: Apache Tomcat: LockOutRealm treats user names as case-sensitive
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:34

[CVE-2026-43513](https://github.com/advisories/GHSA-5mp6-jrq3-r938) — Apache Tomcat: LockOutRealm treats user names as case-sensitive

Severity: **high**.

Affected dependencies:
- `org.apache.tomcat.embed:tomcat-embed-core:9.0.83` — affected range `>= 11.0.0-M1, < 11.0.22`; first patched version `11.0.22`

Description: Improper Handling of Case Sensitivity vulnerability in LockOutRealm in Apache Tomcat.  This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.21, from 10.1.0-M1 through 10.1.54, from 9.0.0.M1 through 9.0.117, from 8.5.0 through 8.5.100, from 7.0.0 through 7.0.109. Older unsupported versions may also be affected.  Users are recommended to upgrade to version 11.0.22, 10.1.55 or 9.0.118 which fix the issue.

Fix: Upgrade to a fixed version, for example: `11.0.22`.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-606: Unchecked Input for Loop Condition
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/main/java/com/photoalbum/controller/HomeController.java

HomeController.uploadPhotos() iterates over the user-supplied request parameter `files` with `for (MultipartFile file : files)` at line 68 after only checking that the list is non-empty at line 62, so the loop bound is controlled by client input without any maximum file-count validation.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/main/resources/application.properties

src/main/resources/application.properties line 12 hard-codes the Oracle datasource password as `spring.datasource.password=photoalbum`, embedding a reusable authentication secret directly in application configuration.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/main/java/com/photoalbum/service/impl/PhotoServiceImpl.java

PhotoServiceImpl.uploadPhoto() rejects empty uploads at lines 101-105 by setting an error message and returning immediately when `file.getSize() <= 0`, but this validation failure is not logged, unlike the other upload rejection branches at lines 86-96.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/main/resources/application.properties

src/main/resources/application.properties lines 11-12 hard-code the Oracle datasource username and password as `spring.datasource.username=photoalbum` and `spring.datasource.password=photoalbum`, storing complete database credentials in source-controlled configuration.
