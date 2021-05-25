---
layout: post
title:  "XSS & CSRF"
date:   2021-01-28 10:00:04 +0530
tags: owasp-top-10 web-security rails node-js xss csrf
author: 'Sushanth Sathesh Rao'
excerpt: 'Introduction to XSS & CSRF'
type: 'web-security'
asset_img: '/assets/img/xss_and_csrf/xss_and_csrf.png'
---

<img src="/assets/img/xss_and_csrf/xss_and_csrf.png">
<div align="center">Image is taken from <a href="https://www.cloudflare.com/learning/security/threats/cross-site-scripting/">Cloudfare</a></div>

# Introduction
**Cross-site scripting(XSS)** is an exploit/vulnerability in which an attacker sends an injected script to a victim that gets executed in a legitimate website on the internet. The extent of impact depends on the type of attack & the extent of security present in the codebase of the compromised website. XSS is listed in OWASP Top 10 vulnerabilities. XSS is a very common type of flaw observed in quite a lot of the websites on the internet. A [2019 report by Positive Technologies](https://www.ptsecurity.com/ww-en/analytics/web-application-vulnerabilities-statistics-2019/) shows that three quarters of websites are vulnerable to XSS attacks. The aforementioned report goes onto prove the multitude & magnitude of XSS attacks. This also brings us to a hot topic for discussion - **Mitigation of XSS Attacks**.

# Types
XSS is essentially injection of a malicious script & sent to a victim that gets executed in a website, thereby compromising the website's user(s). XSS attacks are generally classified/categorised into two types:

> 1) **Stored XSS** - Stored XSS attacks occur when a malicious script is injected & stored into a storage space of the compromised server, like, database, forums, files, logs, etc. Now when a victim tries to retrieve data from the storage space, the script stored in the storage space is executed, if no security measure is in place & any data related to the website/the victim is compromised.

<img src="/assets/img/xss_and_csrf/stored_xss.png">
<div align="center">OWASP WebGoat 7.1 - Cross Site Scripting</div>

> Above is an image of the WebGoat 7.1 web application, in which we are expected to execute a stored XSS attack. In this we are to login as Tom & edit his profile to store a script. So, when we edit address & store a script inline like ```<script>alert("I'm trying XSS");</script>```, we actually realise that the script got executed & an alert is shown by the browser with the message "I'm trying XSS". So what just happened was that we stored the script & on retrieval the script got executed, since there was no security check in place.  
> 2) Reflected XSS - Reflected XSS attacks occur when a malicious script is injected & reflected off the web server, like, a url parameter or in response, etc. Since the response is coming from a "trusted" web server, the browser executes the script without a doubt, thereby compromising the data on the website/of the victim. For example, a url of the form ```https://example.com/search?user=<script>alert("XSS on the rise");</script>```, if reflected off the server without sanitization, then the alert message is just proved to be true. Here, reflected means to respond with the same parameters which is seen usually in searches where the user input is reflected off the server to display results for the query.

<img src="/assets/img/xss_and_csrf/reflected_xss.png">
<div align="center">OWASP WebGoat 7.1 - Cross Site Scripting</div>

> There are several other types of XSS attacks like, Server side XSS, Client Side XSS, DOM based XSS, although each of them are interrelated. For more information regarding different types of XSS attacks, refer to the [OWASP REPORT](https://owasp.org/www-community/Types_of_Cross-Site_Scripting).

<br/>

___

<br/>

# Mitigation of XSS
After having gained some insight into different types of XSS attacks, we can successfully enter one of the current hot topics for discussion, **“Mitigation of XSS Attacks”**. As mentioned earlier nearly 3 quarters of the websites are vulnerable to XSS attacks. So it is extremely important to identify the vulnerabilities in a codebase & mitigate any possible XSS attacks.

* Validate & sanitize all user input before reflecting it or storing it(temporarily/permanently). For this, it is recommended to use framework built-in/recommended packages as they compile modular & flexible code. For example, Rails comes bundled with the `Rails::Html::Sanitizer gem`, which sanitizes content of both formats — HTML & XML.

* A deeper look into the implementation of Rails::Html::Sanitizer gem. I’m explaining the implementation with Rails, since I have worked with Rails for quite some time now.

    * The first idea that comes to one’s mind(at least mine) was to use **RegEx(Regular Expressions)** directly to match any unwanted tags/malicious tags in the input. We could use extremely complex constructs of RegEx for pattern matching & sanitizing the input, but this involves several cons. **Firstly**, the computational resources wasted in executing a complex RegEx construct can lead to DoS attacks(which sounds like a meme, removing a bug & adding another :P). **Secondly**, such complex regex constructs are insufficient to break down the constructs employed by HTML. Even the upgraded RegEx offered by Perl is not capable of completely breaking down HTML into meaningful segments.
    * So, what’s the solution? Here’s where our basic DSA comes in handy. The idea is to mimic the implementation of a browser to sanitize input. So, the implementation goes like this — Firstly, figure out if the input is a [document or a fragment](https://stackoverflow.com/questions/56160162/whats-the-difference-between-a-document-fragment-and-creating-a-parent-element).
    * Now, the solution boils down to the most basic concept of solving problems - either a **[“top-down” (or) “bottom-up”](https://www.tutorialspoint.com/difference-between-bottom-up-model-and-top-down-model)** approach. Example of [top-down parsing](https://www.geeksforgeeks.org/working-of-top-down-parser/) & [bottom-up parsing](https://www.geeksforgeeks.org/working-of-bottom-up-parser/).
    * So, the basic idea is to iterate over the root node(s) & it’s(their) children, either following a top-down or a bottom-up approach. `Rails::Html::Sanitizer` employs a gem called **_“Loofah”_**, which in turn uses the APIs provided by a gem called **_“Nokogiri”_**, for iterating & sanitizing the malicious string.
    * **Trace Implementation**-
       * The actual method called to sanitize the string is a method of the [SafeListSanitizer class](https://github.com/rails/rails-html-sanitizer/blob/master/lib/rails/html/sanitizer.rb#L117).
       * As seen in the [source code](https://github.com/rails/rails-html-sanitizer/blob/master/lib/rails/html/sanitizer.rb#L121), Loofah modules method, [fragment](https://github.com/flavorjones/loofah/blob/fbe0846a43d11d5d41d7555fba1985fedc4e9582/lib/loofah.rb#L41) is used to construct the tree for sanitizing the input. This fragment method makes use of the `Loofah::HTML::DocumentFragment` class which in turn inherits from [Nokogiri::HTML::DocumentFragment](https://github.com/sparklemotion/nokogiri/blob/de00e33e5078a0186f4c373b68ba76cedd1135b1/lib/nokogiri/html/document_fragment.rb) & makes use of it’s methods.
       * The DocumentFragment iterates over the string & constructs [node_sets](https://github.com/sparklemotion/nokogiri/blob/de00e33e5078a0186f4c373b68ba76cedd1135b1/lib/nokogiri/xml/node_set.rb) which is a collection of [nodes](https://github.com/sparklemotion/nokogiri/blob/de00e33e5078a0186f4c373b68ba76cedd1135b1/lib/nokogiri/xml/node.rb). It might be confusing to see that HTML & XML both are using methods defined in the XML classes. This is because HTML is just a regulated version of XML, since HTML has a restricted number of tags to be used. The image below depicts the construction of a `Loofah::HTML::DocumentFragment`, which is essentially a tree.
       <img src="/assets/img/xss_and_csrf/loofah_fragment.png">
       <div align="center">Construction of <code>Loofah::HTML::DocumentFragment</code></div>
       <br/>
       * After construction of the document/document-fragment, now this document/fragment is scrubbed to sanitize the input finally.
       * It is done by iterating over the nodes with a bottom up approach & deleting the tags with their content/only the tags, etc, based on the option given.
       * OWASP cheat sheet recommends a positive XSS prevention model, i.e, allow only the ones that you consider safe, instead of rejecting the ones that are unsafe. “Whitelist” the tags instead of blacklisting tags.
       * So now, the scrubber removes the tags that are not specifically whitelisted. This is a brief explanation for the implementation of a sanitization library.
       * There are several such libraries, one of them being the [Google Closure Library](https://github.com/google/closure-library), which provides in-built sanitization features, which is a maintained version of the archive [Caja library](https://github.com/google/caja). The Caja source code has a lot to offer & I would highly recommend going through the source of the library.
* Another very important aspect that is usually overlooked is encoding of meta characters before inserting untrusted data into HTML/CSS. As [OWASP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html) recommends, it is a very good practice to encode data appropriately before inserting data into HTML. OWASP cheat sheet explains really well the need for encoding data. There are several packages available for each framework to escape meta characters in an unsafe string. Selecting a package mostly depends on the use case & the framework being used.
* Never insert user input directly into any DOM object.
* Always escape any HTML present in user input before inserting into display.
* Always JAVASCRIPT encode user input before inserting into any event (or) script.
* Always CSS encode user input before inserting into property attribute.
* URL encode data before inserting into query parameters. It is recommended to use framework in-built URL generators to automatically URL encode parameters.
* Highly recommended to not use Javascript URLs.

<br/>

___

<br/>

# Cross Site Request Forgery(CSRF)
Till now we have been discussing **“Cross Site”** scripting, but now let’s discuss forging a cross site request. **CSRF** refers to forging requests from a user that they never intended to perform. In this type of attack, attackers circumvent the **“SameOrigin”** policy, which is meant to prevent interference between different websites. The impact of CSRF attacks(dependent on the nature of action used), depend on the powers vested with the account of the victim user.
I highly recommend going through the following blogs/articles for further information on CSRF:

* [OWASP](https://owasp.org/www-community/attacks/csrf)
* [PortSwigger](https://portswigger.net/web-security/csrf)
* [Netspark](https://www.netsparker.com/blog/web-security/csrf-cross-site-request-forgery/)

<br/>

___

<br/>

# Mitigation of CSRF Attacks

* Always use CSRF protection middleware provided by the framework.
* Use HTTP headers like - X-XSS-Protection, X-Frame-Options, etc. Alternatively, you can also set a strong [Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy).
* It is highly recommended to insert a CSRF token in a custom request header.
* **“SameSite”** is a relatively new attribute which is supported only by recent versions of browsers. [SameSite](https://web.dev/samesite-cookies-explained/) has been explained really well in the hyperlinked blog. Using **“SameSite”** effectively minimizes the risk of a csrf attack.
* It is recommended to add interaction based defense - like Captcha.
* Verify origin of request, via headers -> [origin referer X-Forwarded-Host].
* [OWASP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) covers almost all defense mechanisms against CSRF attacks.

<br/>

___

<br/>

# Resources

* [NodeJS Security Threats](https://medium.com/@nodepractices/were-under-attack-23-node-js-security-best-practices-e33c146cb87d)
* [NodeJS Mitigations](https://cheatsheetseries.owasp.org/cheatsheets/Nodejs_Security_Cheat_Sheet.html)
* [Reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected)
* [Parsing HTML](https://blog.codinghorror.com/parsing-html-the-cthulhu-way/)
* [WebGoat](https://owasp.org/www-project-webgoat/)
* [WebGoat Web Application](https://github.com/WebGoat/WebGoat)
* [Rails Best Practices](https://hixonrails.com/ruby-on-rails-tutorials/ruby-on-rails-security-best-practices/)
* [Rails OWASP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Ruby_on_Rails_Cheat_Sheet.html)






