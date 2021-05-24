# [What Is HSTS and Why Should I Use It?](https://www.acunetix.com/blog/articles/what-is-hsts-why-use-it/)



HSTS stands for *HTTP Strict Transport Security*. It is a method used by websites to declare that they should only be accessed using a secure connection (HTTPS). If a website declares an HSTS policy, the browser must refuse all HTTP connections and prevent users from accepting insecure SSL certificates. HSTS is currently supported by [most major browsers (only some mobile browsers fail to use it)](https://caniuse.com/#search=HSTS).



HTTP Strict Transport Security was defined as a web security standard in 2012 in [RFC 6797](https://tools.ietf.org/html/rfc6797). The primary goal of creating this standard was to help avoid [man-in-the-middle (MITM)](https://www.acunetix.com/blog/articles/man-in-the-middle-attacks/) attacks that use *SSL stripping*. SSL stripping is a technique where an attacker forces the browser to connect to a site using HTTP so that they can sniff packets and intercept or modify sensitive information. HSTS is also a good method to protect yourself from cookie hijacking.

## How HSTS Works

Typically, when you enter a URL in the web browser, you skip the protocol part. For example, you type *www.acunetix.com*, not *http://www.acunetix.com*. In such a case, the browser assumes that you want to use the HTTP protocol so it makes an [HTTP request](https://www.acunetix.com/blog/articles/http-security-introduction-to-http-part-1/) to *www.acunetix.com*.
At this stage, the web server replies with a redirect (`301` response code) that points to the HTTPS site. The browser makes an HTTPS connection to *www.acunetix.com*. This is when the HSTS security policy protection begins using an HTTP response header:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

The `Strict-Transport-Security` header gives specific instructions to the browser. From now on, every connection to the site and its subdomains for the next year (31536000 seconds) from the moment this header is received must be an HTTPS connection. HTTP connections are not allowed at all. If the browser receives a request to load a resource using HTTP, it must try an HTTPS request instead. If HTTPS is not available, the connection must be terminated.

Additionally, if the certificate is not valid, you will be prevented from making a connection. Usually, if a certificate is not valid (expired, self-signed, signed by an unknown CA, etc.) the browser displays a warning that you can circumvent. However, if the site has HSTS, the browser will not let you circumvent the warning at all. To access the site, you must remove the site from the HSTS list within the browser.

The `Strict-Transport-Security` header is sent for a given website and covers a particular domain name. Therefore, if you have the HSTS header for *www.acunetix.com*, it will not cover *acunetix.com* but only the *www* subdomain. This is why, for complete protection, your website should include a call to the base domain (in this case, *acunetix.com*) and receive a `Strict-Transport-Security` header for that domain with the `includeSubDomains` directive.

[![How HSTS Works](https://www.acunetix.com/wp-content/uploads/2019/05/hsts-1024x683.png)](https://www.acunetix.com/wp-content/uploads/2019/05/hsts.png)

## Is HSTS Completely Secure?

Unfortunately, the first time that you access the website, you are not protected by HSTS. If the website adds an HSTS header to an HTTP connection, that header is ignored. This is because an attacker can remove or add headers during a man-in-the-middle attack. The HSTS header cannot be trusted unless it is delivered via HTTPS.

You should also know that the HSTS `max-age` is refreshed every time your browser reads the header and the maximum value is two years. This means that the protection is permanent as long as no more than two years pass between your visits. If you do not visit a website for two years, it is treated as a new site. At the same time, if you serve the HSTS header with `max-age` of 0, the browser will treat the site as a new one on the next connection attempt (which can be useful for testing).

You can use an additional method of protection called the HSTS *preload list*. The Chromium project maintains a list of websites that use HSTS and the list is distributed with browsers. If you add your website to the preload list, the browser first checks the internal list and so your website is never accessed via HTTP, not even during the first connection attempt. This method is not part of the HSTS standard but it is used by all major browsers (Chrome, Firefox, Safari, Opera, IE11, and Edge).

The only currently known method that could be used to bypass HSTS is an [NTP-based attack](https://www.blackhat.com/docs/eu-14/materials/eu-14-Selvi-Bypassing-HTTP-Strict-Transport-Security.pdf). If the client computer is susceptible to an NTP attack, it can be fooled into expiring the HSTS policy and accessing the site once with HTTP.

### How to Add a Domain to the HSTS Preload List?

To add a domain to the HSTS preload list, the sites for that domain must meet several requirements. Here is what you need to do to add your domain:

1. Make sure that your sites have valid certificates and up-to-date ciphers.
2. If your sites are available via HTTP, redirect all requests to HTTPS.
3. Make sure that points 1 and 2 above apply to all your domains and subdomains (according to your DNS records).
4. Serve the `Strict-Transport-Security` header over HTTPS for the base domain with `max-age` of at least 31536000 (1 year), the `includeSubDomains` directive, and the `preload` directive. See above for an example of such a valid HSTS header.
5. Go to [hstspreload.org](https://hstspreload.org/) and submit your domain using the form. If the conditions are met, your domain will be queued to be added.

For increased security, the preload list is not accessed or downloaded by the browser. It is distributed as a hard-coded resource with new browser versions. This means that it takes quite a lot of time for results to appear on the list and it takes quite a long time for a domain to be removed from the list. If you want to add your site to the list, you must be sure that you are able to maintain full HTTPS access to all resources for an extended period of time. If not, you risk that your website will become completely inaccessible.

## How to Remove a Domain from the HSTS Cache in a Browser?

When you are setting up HSTS and testing it, you may need to clear the HSTS cache in the browser. If you set up HSTS incorrectly, you may receive errors that will lock you out of the site unless you clear the data. Here are methods for several popular browsers. Also note that if your domain is on the HSTS preload list, clearing the HSTS cache will be ineffective and there is no way to force an HTTP connection.

### Removing from Google Chrome

To remove a domain from the Chrome HSTS cache, follow these instructions:

1. Go to [*chrome://net-internals/#hsts*](https://net-internals/#hsts)
2. In the *Delete domain security policies* section, enter the domain to delete in the text box
3. Click the *Delete* button next to the text box

[![Removing a Domain from the Chrome HSTS Cache](https://www.acunetix.com/wp-content/uploads/2019/05/image1-1.png)](https://www.acunetix.com/wp-content/uploads/2019/05/image1-1.png)

Afterward, you can check if the removal was successful:

1. In the *Query HSTS/PKP domain* section, enter the domain to verify in the text box
2. Click the *Query* button next to the text box
3. The response should be *Not found*

[![Checking a Domain in the Chrome HSTS Cache](https://www.acunetix.com/wp-content/uploads/2019/05/image3.png)](https://www.acunetix.com/wp-content/uploads/2019/05/image3.png)

### Removing from Mozilla Firefox

There are many different methods to remove HSTS information from Firefox for a given domain. All of them are [described in detail in a dedicated article](https://appuals.com/how-to-clear-or-disable-hsts-for-chrome-firefox-and-internet-explorer/). The following is the simplest and fastest one, but it removes more than HSTS information from the cache.

1. Close all open tabs for your site
2. Open the Firefox history window (*Library > History > Show All History*)
3. Search for the domain using the search bar
4. Right-click the domain and choose the option *Forget About This Site*
5. Restart Firefox

[![Removing a Domain from the Firefox HSTS Cache](https://www.acunetix.com/wp-content/uploads/2019/05/image4.png)](https://www.acunetix.com/wp-content/uploads/2019/05/image4.png)

### Removing from Apple Safari

Removing HSTS information from Safari is very easy:

1. Close Safari
2. Delete the following file from your home directory: *~/Library/Cookies/HSTS.plist*
3. Open Safari

### Removing from Microsoft Internet Explorer and Microsoft Edge

You cannot remove a domain from the HSTS cache for Microsoft browsers. You can only turn off HSTS temporarily in Internet Explorer 11 and only on Windows 7 or Windows 8.1 (not on Windows 10). Full instructions are available in the relevant [Microsoft support article](https://support.microsoft.com/en-us/help/3071338/internet-explorer-11-adds-support-for-http-strict-transport-security-s).

## How to Add a Domain to the HSTS Preload List?

To add a domain to the HSTS preload list, the sites for that domain must meet several requirements. Here is what you need to do to add your domain.

| ![Check certificates and ciphers](https://www.acunetix.com/wp-content/uploads/2019/05/howto_hsts_step1-150x150.png) | Step 1: Check certificates and ciphersMake sure that your sites have valid certificates and up-to-date ciphers. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![Redirect all traffic to HTTPS](https://www.acunetix.com/wp-content/uploads/2019/05/howto_hsts_step2-150x150.png) | Step 2: Redirect all traffic to HTTPSIf your sites are available via HTTP, redirect all requests to HTTPS. |
| ![Check all your domains and subdomains](https://www.acunetix.com/wp-content/uploads/2019/05/howto_hsts_step3-150x150.png) | Step 3: Check all your domains and subdomainsMake sure that points 1 and 2 above apply to all your domains and subdomains (according to your DNS records). |
| ![Set an HSTS response header](https://www.acunetix.com/wp-content/uploads/2019/05/howto_hsts_step4-150x150.png) | Step 4: Set an HSTS response headerServe the Strict-Transport-Security header over HTTPS for the base domain with max-age of at least 31536000 (1 year), the includeSubDomains directive, and the preload directive. |
| ![Submit your domain](https://www.acunetix.com/wp-content/uploads/2019/05/howto_hsts_step5-150x150.png) | Step 5: Submit your domainGo to hstspreload.org and submit your domain using the form. If the conditions are met, your domain will be queued to be added. |

## Frequently asked questions