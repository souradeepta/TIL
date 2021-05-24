# What is SSO? | How single sign-on works

Single sign-on (SSO) is an important cloud security technology that reduces all user application logins to one login for greater security and convenience.

## What is single sign-on (SSO)?

Single sign-on (SSO) is a technology which combines several different application login screens into one. With SSO, a user only has to enter their login credentials (username, password, etc.) one time on a single page to access all of their [SaaS](https://www.cloudflare.com/learning/cloud/what-is-saas/) applications. SSO is often used in a business context, when user applications are assigned and managed by an internal IT team. Remote workers who use SaaS applications also benefit from using SSO.

Imagine if customers who had already been admitted to a bar were asked to show their identification card to prove their age each time they attempted to purchase additional alcoholic beverages. Some customers would quickly become frustrated with the continual checks and might even attempt to circumvent these measures by sneaking in their own beverages.

However, most establishments will only check a customer's identification once, and then serve the customer several drinks over the course of an evening. This is somewhat like an SSO system: instead of establishing their identity over and over, a user establishes their identity once and can then access several different services.

SSO is an important aspect of many [identity and access management (IAM)](https://www.cloudflare.com/learning/access-management/what-is-identity-and-access-management/) or [access control](https://www.cloudflare.com/learning/access-management/what-is-access-control/) solutions. User identity verification is crucial for knowing which permissions each user should have. [Cloudflare Access](https://teams.cloudflare.com/access/) is one example of an access control solution that integrates with SSO solutions for managing users' identities.

## What are the advantages of SSO?

In addition to being much simpler and more convenient for users, SSO is widely considered to be more secure. This may seem counterintuitive: how can signing in once with one password, instead of multiple times with multiple passwords, be more secure? Proponents of SSO cite the following reasons:

1. **Stronger passwords:** Since users only have to use one password, SSO makes it easier for them to create, remember, and use stronger passwords.* In practice, this is typically the case: most users do use stronger passwords with SSO.

   **What makes a password "strong"? A strong password is not easily guessed and is random enough that a brute force attack is not likely to succeed. w7:g"5h$G@ is a fairly strong password; password123 is not.*

2. **No repeated passwords:** When users have to remember passwords for several different apps and services, a condition known as "password fatigue" is likely to set in: users will re-use passwords across services. Using the same password across several services is a huge security risk because it means that all services are only as secure as the service with the weakest password protection: if that service's password database is compromised, attackers can use the password to hack all of the user's other services as well. SSO eliminates this scenario by reducing all logins down to one login.

3. **Better password policy enforcement:** With one place for password entry, SSO provides a way for IT teams to easily enforce password security rules. For example, some companies require users to reset their passwords periodically. With SSO, password resets are easier to implement: instead of constant password resets across a number of different apps and services, users only have one password to reset. (While the value of regular password resets [has been called into question](https://docs.microsoft.com/en-us/archive/blogs/secguide/security-baseline-final-for-windows-10-v1903-and-windows-server-v1903), some IT teams still consider them an important part of their security strategy.)

4. **Multi-factor authentication:** Multi-factor authentication, or MFA, refers to the use of more than one identity factor to authenticate a user (see What is MFA?). For example, in addition to entering a username and password, a user might have to connect a USB device or enter a code that appears on their smartphone. Possession of this physical object is a second "factor" that establishes the user is who they say they are. MFA is much more secure than relying on a password alone. SSO makes it possible to activate MFA at a single point instead of having to activate it for three, four, or several dozen apps, which may not be feasible.

5. **Single point for enforcing password re-entry:** Administrators can enforce re-entering credentials after a certain amount of time to make sure that the same user is still active on the signed-in device. With SSO, they have a central place from which to do this for all internal apps, instead of having to enforce it across multiple different apps, which some apps may not support.

6. **Internal credential management instead of external storage:** Usually, user passwords are stored remotely in an unmanaged fashion by applications and services that may or may not follow best security practices. With SSO, however, they are stored internally in an environment that an IT team has more control over.

7. **Less time wasted on password recovery:** In addition to the above security benefits, SSO also cuts down on wasted time for internal teams. IT has to spend less time on helping users recover or reset their passwords for dozens of apps, and users spend less time signing into various apps to perform their jobs. This has the potential to increase business productivity.

## How does an SSO login work?

Whenever a user signs in to an SSO service, the service creates an authentication token that remembers that the user is verified. An authentication token is a piece of digital information stored either in the user's browser or within the SSO service's servers, like a temporary ID card issued to the user. Any app the user accesses will check with the SSO service. The SSO service passes the user's authentication token to the app and the user is allowed in. If, however, the user has not yet signed in, they will be prompted to do so through the SSO service.

An SSO service does not necessarily remember who a user is, since it does not store user identities. Most SSO services work by checking user credentials against a separate identity management service.

Think of SSO as a go-between that can confirm whether a user's login credentials match with their identity in the database, without managing the database themselves — somewhat like when a librarian looks up a book on someone else's behalf based on the title of the book. The librarian does not have the entire library card catalog memorized, but they can access it easily.

## How do SSO authentication tokens work?

The ability to pass an authentication token to external apps and services is crucial in the SSO process. This is what enables identity verification to take place separately from other cloud services, making SSO possible.

Think of an exclusive event that only a few people are allowed into. One way to indicate that the guards at the entrance to the event have checked and approved a guest is to stamp each guest's hand. Event staff can check the stamps of every guest to make sure they are allowed to be there. However, not just any stamp will do; event staff will know the exact shape and color of the stamp used by the guards at the entrance.

Just as each stamp has to look the same, authentication tokens have their own communication standards to ensure that they are correct and legitimate. The main authentication token standard is called SAML (Security Assertion Markup Language). Similar to how webpages are written in HTML (Hypertext Markup Language), authentication tokens are written in SAML.

## How does SSO fit into an access management strategy?

SSO is only one aspect of managing user access. It must be combined with access control, permission control, activity logs, and other measures for tracking and controlling user behavior within an organization's internal systems. SSO is a crucial element of access management, however. If a system does not know who a user is, there is no way to allow or restrict that user's actions.

## Does Cloudflare integrate with SSO solutions?

[Cloudflare Access](https://teams.cloudflare.com/access/) controls and secures user access to applications and websites; it can act as a replacement for most [VPNs](https://www.cloudflare.com/learning/access-management/what-is-a-vpn/). Cloudflare Access integrates with SSO providers in order to identify users and enforce their assigned access permissions. Cloudflare Access is part of the [Cloudflare for Teams](https://teams.cloudflare.com/) product suite.