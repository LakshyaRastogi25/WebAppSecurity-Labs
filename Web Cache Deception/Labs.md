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


