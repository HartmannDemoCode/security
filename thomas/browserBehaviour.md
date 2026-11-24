# Browser-Based Attacks and How to Prevent Them

Browser-based attacks can target people using websites, the browser itself, or web applications. Some protections are for users; others must be implemented by the website or organization.

## Common Fears That Are Usually Not Problems by Themselves

These situations are not usually signs that a device has been compromised. They still have limits, so use the precautions noted below.

- **A website knows your IP address:** Websites need an IP address to send requested pages back to your device. An IP address alone does not normally let a site take control of your device.
- **A site uses cookies:** Cookies are small pieces of data stored by the browser, often to keep a user signed in or remember preferences. They are not programs and do not infect a device, though tracking cookies raise privacy concerns and stolen session cookies can be misused.
- **A website shows a padlock:** The padlock means the connection uses HTTPS; it does not prove that the site itself is honest or safe. Check the address before entering sensitive information.
- **A site asks for camera, microphone, or location access:** Modern browsers normally require permission before a site can use these features. Deny requests you did not expect and review permissions already granted.
- **A pop-up says your device is infected:** A webpage can display a fake warning without having detected anything on your device. Do not call numbers or install software shown in the pop-up; close the page and use your installed security tools if concerned.
- **You viewed a suspicious page:** Simply loading a page does not usually compromise an up-to-date browser, which isolates websites from much of the device. Keep the browser updated and leave if the page prompts you to download or install something.
- **You use private or incognito browsing:** This mode mainly limits what the browser saves locally after the session. It does not hide activity from websites, network administrators, or internet providers.

## Phishing and Fake Login Pages

Attackers imitate trusted websites or messages to trick people into sharing passwords, payment details, or other sensitive information.

**Prevention**
- Check the full website address before signing in, especially after following a link in a message.
- Use bookmarks or type known addresses yourself for important services.
- Use a password manager or passkey; these help prevent signing in to an imitation domain.
- Enable multi-factor authentication, preferably with a passkey or security key.

## Malicious Ads and Drive-By Downloads

Malicious or compromised web content may try to redirect visitors, prompt them to download unsafe files, or take advantage of an unpatched browser.

**Prevention**
- Keep the browser, operating system, and browser components up to date.
- Avoid unexpected downloads and prompts to install software or browser extensions.
- Use the browser's built-in safe-browsing and pop-up protections.
- Website operators should review advertising and third-party scripts used on their sites.

## Malicious Downloads

Files offered through websites may contain malware or may be disguised as legitimate documents or software.

**Prevention**
- Download software from its official source or a trusted app store.
- Be cautious of unexpected attachments and files, even if they appear to come from someone you know.
- Keep reputable endpoint protection enabled and scan files when appropriate.
- Do not enable macros or bypass operating-system warnings for an untrusted file.

## Malicious or Over-Privileged Browser Extensions

An extension may collect browsing data, change search or site behavior, or become unsafe after an update or change of ownership.

**Prevention**
- Install only extensions you need, from a trusted source, and review their permissions.
- Remove extensions you no longer use and periodically review the ones that remain.
- Keep extensions updated; organizations can restrict installation to approved extensions.

## Cross-Site Scripting (XSS)

XSS occurs when a web application includes untrusted content as executable script in a page viewed by another user.

**Prevention**
- Website developers should encode untrusted content for its output context and use safe APIs for inserting text.
- Apply a Content Security Policy (CSP) as an additional layer of protection.
- Keep browser software up to date; this helps protect users but does not fix a vulnerable website.

## Cross-Site Request Forgery (CSRF)

CSRF tricks a signed-in user's browser into making an unwanted request to a website where that user is already authenticated.

**Prevention**
- Website developers should use anti-CSRF tokens or other appropriate request validation.
- Set cookies with suitable `SameSite` attributes and validate request origins where appropriate.
- Require confirmation or re-authentication for sensitive actions.

## Clickjacking

Clickjacking disguises or overlays page controls so a user may click something different from what they intended.

**Prevention**
- Website developers should prevent unauthorized framing with CSP `frame-ancestors` or the `X-Frame-Options` header.
- Users should be cautious of suspicious pages that urge them to click or grant permissions.

## Session or Cookie Theft

If an attacker obtains a session token, they may be able to act as the signed-in user without knowing that user's password.

**Prevention**
- Website operators should use HTTPS throughout the site and set session cookies with `Secure`, `HttpOnly`, and appropriate `SameSite` attributes.
- Rotate session identifiers after sign-in or privilege changes, expire inactive sessions, and invalidate sessions at sign-out.
- Avoid signing in on shared or untrusted devices, and sign out when finished.

## Network Interception and Redirection

An unsafe network or altered DNS settings may redirect a user to an imitation site or expose information sent over an unprotected connection.

**Prevention**
- Prefer HTTPS and do not bypass browser certificate warnings.
- Use trusted networks, secure home router settings, and keep devices updated.
- A VPN can protect traffic on some untrusted networks, but it does not make a fraudulent website safe.

## Browser or Website Vulnerabilities

Software flaws in browsers, extensions, or websites can allow data exposure or unwanted actions. Attackers may target flaws before a fix is available.

**Prevention**
- Enable automatic browser and operating-system updates.
- Remove unsupported plugins and unnecessary extensions.
- Website operators should apply security updates promptly and use secure development and testing practices.
- Organizations can use managed browser settings to limit risky features and extensions.

## Notification and Permission Abuse

Deceptive sites may persuade users to allow notifications, location access, camera access, or other permissions, then misuse them.

**Prevention**
- Deny permissions unless the site needs them for a task you initiated.
- Review and remove site permissions in browser settings.
- Treat unexpected security alerts delivered as web notifications with caution; verify them through the organization's official website or support channel.
