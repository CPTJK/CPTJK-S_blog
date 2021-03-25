# same-origin policy

## brief

There is no single same-origin policy

## general principal 

Generally speaking ,documents retrieved form distinct origin are isolate from each other. For example, if a document retrieved from `http://example.com/doc.html` tries to access the DOM of a document retrieved from `https://example.com/target.html`, the user agent will disallow access because the origin of the first document,(http,example.com,80) doesn't match the origin of the second document (https, example.com, 80).
