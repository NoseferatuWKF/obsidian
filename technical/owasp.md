[Secure Product Design Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Product_Design_Cheat_Sheet.html)
1. The principle of Least Privilege and Separation of Duties
2. The principle of Defense-in-Depth
3. The principle of Zero Trust
4. The principle of Security-in-the-Open

[How to Prevent - Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)
- Establish and use of secure development lifecycle with AppSec professionals to help evaluate and design security and privacy-related controls
- Establish and use a library of secure design patterns or paved road ready to use components
- Use threat modeling for critical authentication, access control, business logic, and key flows
- Integrate security language and controls into user stories
- Integrate plausibility checks at each tier of your application (from frontend to backend)
- Write unit and integration tests to validate that all critical flows are resistant to the threat model. Compile use-cases and misuse-cases for each tier of your application
- Segregate tier layers on the system and network layers depending on the exposure and protection needs
- Segregate tenants robustly by design throughout all tiers
- Limit resource consumption by user or service

[How to Prevent - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- Except for public resources, deny by default
- Implement access control mechanisms once and re-use them throughout the application, including minimizing Cross-Origin Resource Sharing (CORS) usage.
- Model access controls should enforce record ownership rather than accepting that the user can create, read, update, or delete any record
- Unique application business limit requirements should be enforced by domain models
- Diable web server directory listing and ensure file metadata (e.g., .git) and backup files are not present within web roots.
- Log accdess control failures, alert admins when appropriate (e.g., repeated failures).
- Rate limit API and controller access to minimize the harm from automated attack tooling
- Stateful session identifiers should be invalidated on the server after logout. Stateless JWT tokens should rather be short-lived so that the window of opportunity for an attacker is minimized. For longer lived JWTs it's highly recommended to follow the OAuth standards to revoke access.