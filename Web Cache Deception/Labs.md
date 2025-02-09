# Web Cache Deception Write-Up
## Lab: Exploiting path mapping for web cache deception
## **1. Identifying a Target Endpoint**

1. Log in to the application using the provided credentials:
   - **Username:** `wiener`
   - **Password:** `peter`

2. Observe that the response contains your **API key**. This indicates that the `/my-account` endpoint returns sensitive user-specific information.

---

## **2. Identifying a Path Mapping Discrepancy**

1. In **Burp Suite**, navigate to **Proxy > HTTP history**.
2. Locate the `GET /my-account` request, right-click, and select **Send to Repeater**.
3. In the **Repeater** tab, modify the request path by appending an arbitrary segment:
   ```
   GET /my-account/abc HTTP/1.1
   ```
4. Send the request and observe that the response still contains your API key. This suggests that the **origin server abstracts the URL path** and still maps it to `/my-account`.

5. Next, modify the request by adding a static file extension:
   ```
   GET /my-account/abc.js HTTP/1.1
   ```
6. Send the request and examine the response headers:
   - `X-Cache: miss` → Indicates the response was **not served from the cache**.
   - `Cache-Control: max-age=30` → Suggests that if cached, the response will be stored **for 30 seconds**.

7. Resend the request **within 30 seconds** and check the `X-Cache` header. Notice that it now changes to `hit`, confirming that the response was **served from the cache**.
8. This confirms that the caching mechanism **misinterprets** `/my-account/abc.js` as a static file and stores the response, making it a potential vector for exploitation.

---

## **3. Crafting the Exploit**

1. In **Burp Suite's browser**, click **Go to exploit server**.
2. In the **Body** section of the exploit, craft the following payload to force the victim (`carlos`) to visit the malicious cached URL:
   ```html
   <script>document.location="https://YOUR-LAB-ID.web-security-academy.net/my-account/wcd.js"</script>
   ```
3. Click **Deliver exploit to victim**.
4. When the victim (`carlos`) accesses the exploit, their response (including their **API key**) is **cached**.

---

## **4. Extracting the API Key and Submitting the Solution**

1. Manually visit the cached URL:
   ```
   https://YOUR-LAB-ID.web-security-academy.net/my-account/wcd.js
   ```
2. Observe that the response **now contains carlos' API key**.
3. Copy the API key. 
4. Click **Submit solution** and paste carlos’ API key to complete the challenge.
5. API Key for my lab 
```
ydYS2hO3IhusK9NEChqkMbqPlBUQSH
```

# Lab2: Exploiting path delimiters for web cache deception

## 1. Identifying a Target Endpoint

1. Log in to the application using the provided credentials:
   - Username: wiener
   - Password: peter

2. Observe that the response contains your API key. This confirms that the `/my-account` endpoint returns user-specific data.

## 2. Identifying Path Delimiters Used by the Origin Server

1. In Burp Suite, navigate to Proxy > HTTP history.
2. Locate the `GET /my-account` request, right-click, and select Send to Repeater.
3. In the Repeater tab, modify the request path by appending an arbitrary segment:
   ```
   GET /my-account/abc HTTP/1.1
   ```
4. Send the request and observe the 404 Not Found response, confirming that the origin server does not abstract `/my-account`.

5. Modify the request by appending an arbitrary string without a delimiter:
   ```
   GET /my-accountabc HTTP/1.1
   ```
6. Send the request. The 404 Not Found response confirms that the server does not recognize this modification.

7. Right-click the request and select Send to Intruder.
8. In Intruder, ensure that Sniper attack mode is selected.
9. Add a payload position after `/my-account`:
   ```
   /my-account§§abc
   ```
10. In the Payloads side panel, under Payload configuration, add a list of possible delimiter characters.
11. Under Payload encoding, deselect URL encoding for these characters.
12. Click Start attack.

13. Once the attack completes, sort the results by Status code.
14. Observe that the `;` and `?` characters return a 200 OK response with the API key, while all others return 404 Not Found.

## 3. Investigating Path Delimiter Discrepancies

1. Go to the Repeater tab with the `/my-accountabc` request.
2. Add the `?` delimiter and append a static file extension:
   ```
   GET /my-account?abc.js HTTP/1.1
   ```
3. Send the request. No caching headers appear in the response, indicating that the cache also uses `?` as a delimiter.

4. Repeat the test using `;` instead of `?`:
   ```
   GET /my-account;abc.js HTTP/1.1
   ```
5. Send the request. Observe that the response includes the X-Cache: miss header.
6. Resend the request within the cache duration.
7. The X-Cache header changes to `hit`, confirming that:
   - The cache does not use `;` as a delimiter.
   - The cache has a rule based on the `.js` extension, allowing the response to be stored.

## 4. Crafting the Exploit

1. In Burp Suite's browser, go to the Exploit Server.
2. In the Body section, craft an exploit to redirect the victim (carlos) to the malicious cached URL:
   ```html
   <script>document.location="https://YOUR-LAB-ID.web-security-academy.net/my-account;wcd.js"</script>
   ```
3. Click Deliver exploit to victim.
4. When the victim views the exploit, their API key response is cached.

## 5. Extracting the API Key and Submitting the Solution

1. Manually visit the cached exploit URL:
   ```
   https://YOUR-LAB-ID.web-security-academy.net/my-account;wcd.js
   ```
2. Observe that the response contains carlos’ API key.
3. Copy the API key.
4. Click Submit solution and paste carlos' API key to complete the challenge.
5. API key for my lab was :
```
CKA6jfg6ybxTwPDnUJCpqGId5Ql7XB2f
```
