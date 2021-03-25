# what is mixed content?

Mixed content occurs when initial HTML is loaded over a secure HTTPS connection, but other resources (such as images, videos, stylesheets scripts) are loaded over an insecure HTTP connection. This is called mixed content because both HTTP and HTTPS content are being loaded to display the same page, and the initial was secure over HTTPS.

Requesting subresources using the insecure HTTP protocol weakens the security of the entire page, as these requests are vulnerable to `on-path attacks`, where an attacker eavesdrop on a network connection and views or modifies the communication between two parties. Using these resources, attackers can track users and replace content on a website, and in the case of active mixed content, take complete control over the page, not just insecure resources.

Although many browsers report mixed content warnings to user, by the time this happens, it is too late: the insecure request have already been performed and the security of this page is compromised.

This is why browsers are increasingly blocking mixed content. If you have mixed content on your site, then fixing it will ensure the content continues to load as browsers become more strict.

