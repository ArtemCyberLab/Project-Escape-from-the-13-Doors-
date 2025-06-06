Project Goal
This project documents my analysis and solution of the "Corridor" room on TryHackMe. The main objective was to identify and exploit an IDOR (Insecure Direct Object Reference) vulnerability in a web application. This task helped me sharpen my web security testing skills by investigating URL patterns and hash-based resource access.

Target Information
The challenge was hosted at the following IP:

http://10.10.6.73
The web interface presents a corridor with several doors. Each door, when clicked, redirects to a new URL in the format:

Where <hash> is the MD5 hash of the door number.

Methodology
I noticed that each door's redirection uses an MD5 hash of a numeric identifier.

I assumed the doors were numbered from 0 to 13 and decided to brute-force the hashes for these numbers.

Using CrackStation and a Python script, I generated MD5 hashes for all numbers from 0 to 13.

I discovered that using the MD5 hash of 0, which is:

nginx
cfcd208495d565ef66e7dff9f98764da
led me to a hidden page containing the flag:

http://10.10.6.73/cfcd208495d565ef66e7dff9f98764da
🛠 Tools Used
CrackStation — for MD5 hash decoding

Python — for hash generation

Browser / Burp Suite — for URL testing

TryHackMe — CTF platform

Python snippet for generating MD5 hashes:

import hashlib

for i in range(14):
    print(f"{i} -> {hashlib.md5(str(i).encode()).hexdigest()}")
 Vulnerability
Type: IDOR (Insecure Direct Object Reference)

Problem: Predictable and unprotected access to resources using MD5 hashes of numeric IDs.

Conclusion
Through careful observation and analysis, I was able to identify and exploit an IDOR vulnerability. The use of MD5 hashes for page routing without access control allowed me to reveal hidden content by simply reversing or generating hash values. This exercise illustrates the risks of insecure direct object references and the importance of proper access validation in web applications.

Flag (Spoiler Alert)
Flag found at:
http://10.10.6.73/cfcd208495d565ef66e7dff9f98764da
→ which is the result of md5(0)
