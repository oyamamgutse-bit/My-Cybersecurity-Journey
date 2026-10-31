# My-Cybersecurity-Journey
A documentation repository tracking my reconnaissance and infrastructure fingerprinting workflows on live targets using Kali Linux tools.


**key Phases of the assessment

**Subdomain Brute-Forcing via `dnsmap`**
    *   **Purpose:** Identifying hidden infrastructure and subdomains by running a targeted brute-force attack using a custom wordlist.
    *   **Command Executed:** 
        ```bash
        dnsmap procompare.co.za -w /home/lukhanyo/YERR/subdomains.txt
        ```
    *   **Discoveries:** 
        *   `www.procompare.co.za` ---> `195.201.125.154` (Main website)
        *   `api.procompare.co.za` ---> `128.140.47.36` (Backend API endpoint)
        *   `app.procompare.co.za` ---> `138.201.82.27` (User application environment)

*   **Infrastructure Fingerprinting via `curl`**
    *   **Purpose:** Inspecting HTTP response headers of the target to map out server infrastructure and assess security posture.
    *   **Command Executed:** 
        ```bash
        curl -IL https://procompare.co.za
        ```
    *   **Analysis & Findings:**
        *   **Server Stack:** The target handles web traffic utilizing an **Nginx** server engine.
        *   **Redirection Matrix:** Returns an initial `HTTP/2 301` moving permanently to `https://www.procompare.co.za/` before completing an `HTTP/2 200 OK` handshake.
        *   **Security Controls Implemented:** Defensive header evaluation confirms active mitigations against standard browser vulnerabilities:
            *   `x-frame-options: SAMEORIGIN` (Clickjacking defense)
            *   `x-content-type-options: nosniff` (MIME-sniffing protection)
            *   `referrer-policy: strict-origin-when-cross-origin` (Information disclosure mitigation)

*   **Key Takeaways & Lessons Learned**
    *   **High-Value Targets:** Both `api` and `app` subdomains represent high-value targets for an external penetration test since they interact directly with backend infrastructure and databases.
    *   **Hardened Configuration:** The target shows a strong baseline security posture by correctly implementing defensive HTTP security headers on their primary Nginx web infrastructure.
