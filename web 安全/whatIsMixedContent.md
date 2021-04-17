# what is mixed content?

## brief

Mixed content occurs when initial HTML is loaded over a secure HTTPS connection, but other resources (such as images, videos, stylesheets scripts) are loaded over an insecure HTTP connection. This is called mixed content because both HTTP and HTTPS content are being loaded to display the same page, and the initial was secure over HTTPS.

Requesting subresources using the insecure HTTP protocol weakens the security of the entire page, as these requests are vulnerable to `on-path attacks`, where an attacker eavesdrop on a network connection and views or modifies the communication between two parties. Using these resources, attackers can track users and replace content on a website, and in the case of active mixed content, take complete control over the page, not just insecure resources.

Although many browsers report mixed content warnings to user, by the time this happens, it is too late: the insecure request have already been performed and the security of this page is compromised.

This is why browsers are increasingly blocking mixed content. If you have mixed content on your site, then fixing it will ensure the content continues to load as browsers become more strict.

## the two types of mixed Content

The two types of mixed content are: **active** and **passive**.

**passive mixed content** refers to content that doesn't interact with the reset of the page, and thus a man-in-the-middle attack is restricted to what they can do if they intercept or change that content. Passive mixed content is defined as images, videos, and audio content.

**Active mixed content** interacts with the page as a whole and allows an attacker to do almost anything with the page. Active mixed content includes scripts, stylesheets, iframes, and other code that browser can download and execute.

### passive mixed content

Passive mixed content is seen as less problematic yet still poses a security threat to your site and your users. For example, an attacker can intercept HTTP request for images on your site and swap or replace these images; the attacker can swap the *save* and *delete* button images, causing your users to delete content without intending to; replace your product diagram with lewd or pornographic content, defacing your site; or replace your product pictures with ads for different site or product.

Even if the attacker doesn't alter the content of your site, an attacker can track users via mixed content requests. The attacker can tell which pages a user visit and which product they view base on images or other resources the browser loads.

If passive mixed content is present most browsers will indicate in the URL bar that the page is not secure, even when the page itself was loaded over HTTPS. You can observe this behavior with this 