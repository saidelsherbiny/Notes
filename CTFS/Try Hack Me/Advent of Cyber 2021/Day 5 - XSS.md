# Day 5 - Web - XSS

## What is a XSS vulnerability?

- Cross-Site Scripting, is an injection attack where malicious JavaScript gets injected into a web application with the intention of being executed by other users.

## Types of XSS

### DOM
- DOM stands for Document Object Model and is a programming interface for HTML and XML documents.
- It represents the page so that programs can change the document structure, style and content.

- DOM Based XSS is where the JavaScript execution happens directly in the browser without any new pages being loaded or data submitted to backend code.
- An example of this could be a website's JavaScript code getting the contents from the `window.location.hash` parameter and then write that onto the page in the currently being viewed section. 
	- The contents of the hash aren't checked for malicious code, allowing an attacker to inject JavaScript of their choosing onto the webpage.

### Reflected 
- Reflected XSS happens when user-supplied data in an HTTP request is included in the webpage source without any validation.

### Stored
- The XSS payload is stored on the web application (in a database, for example) and then gets run when other users visit the site or web page.

### Blind
- Blind XSS is similar to a stored XSS in that your payload gets stored on the website for another user to view, but in this instance, you can't see the payload working or be able to test it against yourself first.

