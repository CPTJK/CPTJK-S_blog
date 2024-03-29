# same-origin policy

## brief

There is no single same-origin policy

## general principal 

Generally speaking ,documents retrieved form distinct origin are isolate from each other. For example, if a document retrieved from `http://example.com/doc.html` tries to access the DOM of a document retrieved from `https://example.com/target.html`, the user agent will disallow access because the origin of the first document,(http,example.com,80) doesn't match the origin of the second document (https, example.com, 80).
The **same-origin policy** is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolated potentially malicious documents, reducing possible attribute vectors.

## definition of an origin

Two URLs have the same origin if the **protocol**,**port**(if specified),and **host** are all the same.

The following table gives examples of origin comparisons with the URL`http://store.company.com/dir/page.html`:

| URL | outcome | reason |
| --- | --- | --- |
| `http://store.company.com/dir2/other.html` | same origin  | only the path differs |
| `http://store.company.com/dir/inner/another.html` | same origin  | only the path differs |
| `https://store.company.com/page.html` | failure  | different protocol |
| `http://store.company.com:81/dir/page.html` | failure | different port |
| `http://news.company.com/dir/page.html` | failure | different host |

### inherited origins

Scripts executed from pages with an `about:blank` or `javascript:` URL inherit the origin of the document containing that URL, since these types of URLs do not contain information about an origin server.

>For example, `about:blank` is often used as a URL of new, empty popup windows into which the parent script writes content (e.g. via the Window.open() mechanism). If this popup also contains JavaScript, that script would inherit the same origin as the script that created it.

### change origin

A page may change its own origin, with some limitations. A script can set the value of `documents.domain` to its current domain or a superdomain of its current domain. If set to a superdomain of the current domain, the shorter superdomain is used for same-origin checks.

For example, assume a script from the document at `http://store.company.com/dir/other.html` executes the following:

```JavaScript
document.domain = 'company.com';
```

Afterwards, the page can pass the same-origin check with `http://company.com/dir/page.html`

