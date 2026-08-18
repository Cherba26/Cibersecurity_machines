`difficulty: easy`  `Theme: phishing`

We can see this suspicious email.
![](</Imagenes/image11.png>)
### Q1. Which individual received the email regarding a **Quote for Services Rendered**?

`Answer: William McClean`

We can see the subject  "Quote for Services Rendered" in the list of emails that have to be investigated.  (the same of the above picture)

### Q2. What email address was used by the adversary to send the phishing emails?

`Answer: Accounts.Payable@groupmarketingonline.icu`

### Q3. Investigate the attachment in the email addressed to Zoe Duncan. What is the root domain of the redirection URL found within the file?

`Answer:kennaroads.buzz`

![](</Imagenes/image12.png>)

Opening the file, we can see that it wants to redirect us to the following url.

### Q4.  Open the attachment in your VM web browser.  Which company is the login page impersonating?

`Answer: Microsoft`

![](</Imagenes/image13.png>)

### Q5. Navigate to the `/data` directory.  What is the name of the archive file?

`Answer: Update365.zip`

![](</Imagenes/image14.png>)

### Q6.  Using the `sha256sum` command, what is the `SHA256` hash of the file?
`Answer: ba3c15267393419eb08c7b2652b8b6b39b406ef300ae8a18fee4d16b19ac9686`

### Q7. Aside from **phishing**, what other threat category is assigned to the `ZIP` archive?

`Answer: Trojan`

Searching it in VirusTotal, the page gave us the information.

### Q8. How many files are contained within the archive?

`Answer: 49`

![](</Imagenes/image15.png>)

### Q9. What is the email address of the user who submitted their credentials more than once?

`Answer: michael.ascot@swiftspend.finance`

![](</Imagenes/image16.png>)


### Q10. What email address is used by the adversary to collect compromised credentials?

`Answer: m3npat@yandex.com`

![](</Imagenes/image17.png>)

### Q11. Return to the phishing URL and locate the `flag.txt` file. Using [CyberChef (opens in new tab)](https://gchq.github.io/CyberChef/#recipe=From_Base64\('A-Za-z0-9%2B/%3D',true,false\)Reverse\('Character'\)&ieol=CRLF) to decode the flag, what is the secret value?

`Answer: THM{pL4y_w1Th_tH3_URL}`

Checking the URL there is an empty folder, so it has something hidden. If we search `kennaroads.buzz/data/Update365/office365/flag.txt` we get the flag. 
