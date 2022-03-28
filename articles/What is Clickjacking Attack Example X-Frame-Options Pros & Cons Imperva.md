# What is Clickjacking | Attack Example | X-Frame-Options Pros & Cons | Imperva
## What is clickjacking

Clickjacking is an attack that tricks a user into clicking a webpage element which is invisible or disguised as another element. This can cause users to unwittingly download malware, visit malicious web pages, provide credentials or sensitive information, transfer money, or purchase products online.

Typically, clickjacking is performed by displaying an invisible page or HTML element, inside an iframe, on top of the page the user sees. The user believes they are clicking the visible page but in fact they are clicking an invisible element in the additional page transposed on top of it.

The invisible page could be a [malicious page](https://www.imperva.com/learn/application-security/malware-detection-and-removal/), or a legitimate page the user did not intend to visit – for example, a page on the user’s banking site that authorizes the transfer of money.

There are several variations of the clickjacking attack, such as:

-   **Likejacking** – a technique in which the Facebook “Like” button is manipulated, causing users to “like” a page they actually did not intend to like.
-   **Cursorjacking** – a UI redressing technique that changes the cursor for the position the user perceives to another position. Cursorjacking relies on vulnerabilities in Flash and the Firefox browser, which have now been fixed.

## Clickjacking attack example

1.  The attacker creates an attractive page which promises to give the user a free trip to Tahiti.
2.  In the background the attacker checks if the user is logged into his banking site and if so, loads the screen that enables transfer of funds, using query parameters to insert the attacker’s bank details into the form.
3.  The bank transfer page is displayed in an invisible iframe above the free gift page, with the “Confirm Transfer” button exactly aligned over the “Receive Gift” button visible to the user.
4.  The user visits the page and clicks the “Book My Free Trip” button.
5.  In reality the user is clicking on the invisible iframe, and has clicked the “Confirm Transfer” button. Funds are transferred to the attacker.
6.  The user is redirected to a page with information about the free gift (not knowing what happened in the background).

![](https://www.imperva.com/learn/wp-content/uploads/sites/13/2019/01/Clickjacking.png.webp)

This example illustrates that, in a clickjacking attack, the malicious action (on the bank website, in this case) cannot be traced back to the [attacker](https://www.imperva.com/learn/application-security/ethical-hacking/) because the user performed it while being legitimately signed into their own account.

## Clickjacking mitigation

There are two general ways to defend against clickjacking:

-   **Client-side methods** – the most common is called Frame Busting. Client-side methods can be effective in some cases, but are considered not to be a best practice, because they can be easily bypassed.
-   **Server-side methods** – the most common is X-Frame-Options. Server-side methods are recommended by security experts as an effective way to defend against clickjacking.

The X-Frame-Options response header is passed as part of the HTTP response of a web page, indicating whether or not a browser should be allowed to render a page inside a <FRAME> or <IFRAME> tag.

There are three values allowed for the X-Frame-Options header:

-   **DENY** – does not allow any domain to display this page within a frame
-   **SAMEORIGIN** – allows the current page to be displayed in a frame on another page, but only within the current domain
-   **ALLOW-FROM URI** – allows the current page to be displayed in a frame, but only in a specific URI – for example _www.example.com/frame-page_

## Using the SAMEORIGIN option to defend against clickjacking

X-Frame-Options allows content publishers to prevent their own content from being used in an invisible frame by attackers.

The DENY option is the most secure, preventing any use of the current page in a frame. More commonly, SAMEORIGIN is used, as it does enable the use of frames, but limits them to the current domain.

### Limitations of X-Frame-Options

-   To enable the SAMEORIGIN option across a website, the X-Frame-Options header needs to be returned as part of the HTTP response for each individual page (cannot be applied cross-site).
-   X-Frame-Options does not support a whitelist of allowed domains, so it doesn’t work with multi-domain sites that need to display framed content between them.
-   Only one option can be used on a single page, so, for example, it is not possible for the same page to be displayed as a frame both on the current website and an external site.
-   The ALLOW-FROM option is not supported by all browsers.
-   X-Frame-Options is a deprecated option in most browsers.

### Clickjacking test – Is your site vulnerable?

A basic way to test if your site is [vulnerable](https://www.imperva.com/learn/application-security/vulnerability-management/) to clickjacking is to create an HTML page and attempt to include a sensitive page from your website in an iframe. It is important to execute the test code on another web server, because this is the typical behavior in a clickjacking attack.

Use code like the following, provided as part of the [OWASP Testing Guide](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet#Defending_with_X-Frame-Options_Response_Headers):

<html>
<head>
<title>Clickjack test page</title>
</head>
<body>
<p>Website is vulnerable to clickjacking!</p>
<iframe src="http://www.yoursite.com/sensitive-page" width="500" height="500"></iframe>
</body>
</html>

View the HTML page in a browser and evaluate the page as follows:

-   If the text “Website is vulnerable to clickjacking” appears and below it you see the content of your sensitive page, **the page is vulnerable to clickjacking**.
-   If only the text “Website is vulnerable to clickjacking” appears, and you do not see the content of your sensitive page, the page is not vulnerable to the simplest form of clickjacking.

However, additional testing is needed to see which anti-clickjacking methods are used on the page, and whether they can be bypassed by attackers.

### How Imperva helps mitigate clickjacking attack

To get to the point of clickjacking a site, the site will have to be compromised, something [Imperva WAF](https://www.imperva.com/products/web-application-firewall-waf/) prevents. You should also make sure your site resources are sending the proper X-Frame-Options HTTP headers, which would prevent some parts of your site from being framed in other pages or outside your domain. 
 [https://www.imperva.com/learn/application-security/clickjacking/](https://www.imperva.com/learn/application-security/clickjacking/)
