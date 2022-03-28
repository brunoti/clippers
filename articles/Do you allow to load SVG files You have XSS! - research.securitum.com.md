# Do you allow to load SVG files? You have XSS! - research.securitum.com
**Uploading files by web application users creates many vulnerabilities. In this functionality, pentesters are looking for gaps leading to remote code execution on the server side. What if the upload of a new file resulted in the execution of a malicious JS script?**

**Such opportunity provides SVG files that describe vector graphics in modern browsers.**

## **Graphics or document?**

Intuitively _Scalable Vector Graphic_ is a vector graphic that defines the image with shapes instead of the colors of individual groups of pixels. Thanks to this, SVG images are infinitely scalable: both the thumbnail and the image presented on the big screen will look just as good—without loss of quality associated with raster graphics represented on the internet by files such as _gif, png_ or _jpeg_.

The SVG format has been natively supported in all browsers for many years, including Internet Explorer 9+ and mobile browsers. Therefore, the popularity of SVG is constantly growing and this format is now becoming the standard for various types of icons and bootstrapping interfaces of modern websites.

But let’s go back to the security context. The term SVG as a graphic is a big shorthand. In practice, **SVG is not a graphical format, but an XML document** describing the elements that make up graphics and its additional interactions with the environment. That is, SVG files can be an interactive document, such as HTML and can change depending on pre-programmed actions.

Just like in HTML we have a DOM tree, so SVG has its own object tree (SVG DOM), which is available from JS scripts. Due to the fact that HTML, JavaScript and SVG work in the same ecosystem – in the browser – you can easily create an SVG file that performs malicious actions in the area of the tested web application.

## **SVG = XSS**

Let’s assume that a website has a file upload function. For the sake of security, the creator of the website decides solely on the ability to load graphic files, including SVG files.

What happens when the attacker uploads the SVG file below to the site?

\|  \| 

&lt;svg xmlns\\="[http://www.w3.org/1999/svg"\\>](http://www.w3.org/1999/svg"\>)

<script>

alert(1)

</script>

&lt;/svg>

 \|

![](https://research.securitum.com/wp-content/uploads/sites/2/2019/03/Obrazek-1-6.png)

Upload a malicious SVG file to the site

In this case, the file will be saved and possible to access by referring to the same domain (for example, through the images directory or a script displaying the image with the given identifier). The image will be sent to the browser in the form of the src parameter of the <img> element. The result will display only the empty alt image attribute:

![](https://research.securitum.com/wp-content/uploads/sites/2/2019/03/Obrazek-2-6.png)

A malicious SVG file in the DOM of the website

However, when we refer to the full path of the above graphic file, we notice that **we managed to save the file capable of executing the Persistent XSS attack:**

![](https://research.securitum.com/wp-content/uploads/sites/2/2019/03/Obrazek-3-5.png)

XSS Persistent error in the website domain

The response headers are also worth noting. In the above example, the programmer did not specify the type of response returned for the image serving script—it simply (naively) relied on the logic of the server or on the _Content Sniffing_ mechanism of the browsers. The SVG file was interpreted as text/html, which is exactly like a regular HTML page with an embedded inline script.

However, this situation is not often encountered and is not particularly interesting. In this case, we can probably upload any HTML code leading to the HTML Injection gap, so SVG does not add anything special here. The situation where the Content-Type header contains a value corresponding to the content of the previously loaded file is much more common. In the case of SVG, the correct MIME type is image/svg+xml.

When such a heading is returned, the earlier attack will not take place:

![](https://research.securitum.com/wp-content/uploads/sites/2/2019/03/Obrazek-4-4.png)

Interpreting the SVG file as an XML document in the browser

It turns out that the above security can be bypassed.

For this purpose, we will create a valid SVG file that will contain both a graphic and a script displaying a message demonstrating the execution of the XSS attack:

\|  \| 

<?xml version\="1.0"  standalone\="no"?>

<!DOCTYPE svg PUBLIC  "-//W3C//DTD SVG 1.1//EN"  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd"\>

&lt;svg version\\="1.1"  baseProfile\\="full"  xmlns\\="[http://www.w3.org/2000/svg"\\>](http://www.w3.org/2000/svg"\>)

&lt;polygon id\\="triangle"  points\\="0,0 0,50 50,0"  fill\\="#009900"  stroke\\="#004400"/>

<script type\="text/javascript"\>

alert('This app is probably vulnerable to XSS attacks!');

</script>

&lt;/svg>

 \|

We assume that the programmer correctly sends the Content-Type headers and adds the .svg extension to the file name so that the browser has no doubts about the content being served. After loading the file through the site’s mechanisms and loading the new resource, we will see that we are still able to execute malicious scripts.

A properly handled SVG file has become a gateway to the _Persistent XSS_ attack:

![](https://research.securitum.com/wp-content/uploads/sites/2/2019/03/Obrazek-5-3.png)

Storage XSS error despite the correct HTTP response headers setting

Then set up a very strict [Content Security Policy (CSP)](http://sekurak.pl/czym-jest-content-security-policy/) for a script that returns vector graphics content or for a server that serves SVG files from a directory. Setting the CSP to a value such as Content-Security-Policy: script-src ‘none’ will disable the ability to execute scripts for a given document (i.e., in a malicious SVG graphic).

SVG files can also be processed on the server side and converted into more secure raster image formats (e.g., _png_). You can also try to remove potentially malicious scripts; for this purpose, you can use, for example, the [SVG Purifier](http://heideri.ch/svgpurifier/SVGPurifier/index.php) tool.

As you can see, for managing SVG there are quite a few ideas, but the best solution is to use SVG files only created by programmers, with strict prohibition of uploading this kind of document.

**Say NO to SVG files from an untrusted source.**

![](https://research.securitum.com/wp-content/uploads/sites/2/2019/03/Obrazek-6-3.png)

Tagged: [Upload](https://research.securitum.com/tag/upload/), [XSS](https://research.securitum.com/tag/xss/) 
 [https://research.securitum.com/do-you-allow-to-load-svg-files-you-have-xss/](https://research.securitum.com/do-you-allow-to-load-svg-files-you-have-xss/)
