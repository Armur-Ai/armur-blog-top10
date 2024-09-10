---
title: "Cross-Site Request Forgery (CSRF): Understanding and Exploiting Web Application Vulnerabilities"
description: "This tutorial introduces CSRF attacks, explaining how they exploit user trust and browser behavior to perform unauthorized actions on websites."
image: "https://armur-ai.github.io/armur-blog-pentest/images/security-fundamentals.png"
icon: "code"
draft: false
---
## Introduction to Cross-Site Request Forgery (CSRF)

Cross-Site Request Forgery (CSRF), also known as XSRF or one-click attack, is a type of web application vulnerability that tricks a user's browser into executing unwanted actions on a website where they are currently authenticated.  CSRF attacks exploit the trust that a website has in a user's browser and the fact that browsers automatically include cookies and other authentication information with every request.

## How CSRF Attacks Work

1. **The attacker creates a malicious website or webpage:** This could be a specially crafted HTML page, an image tag with a malicious URL, or a hidden form that automatically submits a request.
2. **The user visits the attacker's website:** This could happen through various means, such as clicking a link in a phishing email, visiting a compromised website, or interacting with malicious content on social media.
3. **The attacker's website triggers a request to the vulnerable website:** The malicious content on the attacker's website sends a forged request to the target website where the user is logged in. This request might be hidden from the user or disguised as something harmless.
4. **The user's browser automatically includes their authentication information:**  The user's browser, unaware that the request is forged, automatically includes their cookies and other authentication tokens with the request to the target website.
5. **The target website processes the request:** The target website, trusting the authentication information received, processes the request as if it came from the legitimate user.
6. **The attacker's action is executed:** The forged request could perform any action that the user is authorized to do on the target website, such as changing their password, transferring funds, or making purchases.

## Types of CSRF Attacks

* **GET-based CSRF:** The attacker uses a simple link or image tag to trigger a GET request to the vulnerable website. This is the easiest type of CSRF to exploit but is limited to actions that can be performed using GET requests.
* **POST-based CSRF:** The attacker uses a hidden form or JavaScript code to submit a POST request to the vulnerable website. This is more complex to exploit but allows for a wider range of actions.

## Practical Example: Exploiting a Bank Transfer Functionality

Imagine a banking website that allows users to transfer funds using a POST request with the following parameters:

```
POST /transfer.php HTTP/1.1
...
account_number=1234567890
amount=100
```

An attacker could create a malicious website with a hidden form like this:

```html
<form action="https://bank.com/transfer.php" method="POST">
  <input type="hidden" name="account_number" value="attacker_account">
  <input type="hidden" name="amount" value="1000">
  <input type="submit" value="Click here to win a prize!">
</form>
<script>
  document.forms[0].submit(); // Automatically submit the form
</script>
```

If a user who is logged into their bank account visits the attacker's website, their browser will automatically submit the form to the banking website, transferring funds to the attacker's account without the user's knowledge or consent.

## Detecting and Preventing CSRF Vulnerabilities

**Detection:**

* **Analyze HTTP requests and forms:**  Look for functionalities that modify data or perform sensitive actions using GET or POST requests.
* **Test for missing CSRF protection mechanisms:**  Try submitting requests from a different domain or using tools like Burp Suite to intercept and modify requests.

**Prevention:**

* **Synchronizer Token Pattern:** Generate a unique, unpredictable token for each user session and include it as a hidden field in all forms. Validate this token on the server side to ensure that the request originated from the legitimate user's browser.
* **Double Submit Cookie Pattern:**  Generate a random token and store it both in a cookie and as a hidden form field. Verify that the token in the cookie and the form field match on the server side.
* **SameSite Cookie Attribute:** Set the `SameSite` attribute for cookies to `Strict` or `Lax` to restrict their inclusion in cross-site requests.
* **Verify Origin Header:**  Check the `Origin` header of requests to ensure that they are coming from the expected domain.
* **CAPTCHA for Sensitive Actions:**  Use CAPTCHAs for actions that have significant consequences, such as changing passwords or transferring funds.
* **Educate Users:**  Teach users to be cautious about clicking links or interacting with content from untrusted sources.

## Conclusion

Cross-Site Request Forgery is a serious security vulnerability that can be exploited to perform unauthorized actions on behalf of users. By understanding how CSRF attacks work and implementing appropriate prevention techniques, developers can protect their applications and users from this threat.