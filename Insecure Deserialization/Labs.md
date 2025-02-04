---

# **Lab1 Write-Up: Lab: Modifying serialized objects**  

## **Lab Summary**  
In this lab, we exploit **insecure deserialization** in PHP to escalate privileges and delete a user account. The session cookie contains a **serialized PHP object**, which we modify to gain admin access.  

---

## **Steps to Exploit the Vulnerability**  

### **Step 1: Log in and Identify the Session Cookie**  
1. Log in using your own credentials.  
2. Observe the `GET /my-account` request, which includes a **session cookie** in the request headers.  
3. The session cookie appears to be **Base64-encoded** and **URL-encoded**.  

---

### **Step 2: Decode the Session Cookie**  
1. Use **Burp Suite's Inspector panel** to **decode** the session cookie.  
2. Upon decoding, you will see that the cookie contains a **serialized PHP object** similar to:  
   ```
   O:4:"User":1:{s:5:"admin";b:0;}
   ```
   - `O:4:"User"` → This means an object of class `User` with 4 characters in the name.  
   - `s:5:"admin";b:0;` → This indicates the **admin attribute** is set to `false` (`b:0`).  

---

### **Step 3: Modify the Serialized Object to Gain Admin Access**  
1. Send the request to **Burp Repeater**.  
2. In Burp Repeater, open the **Inspector panel** and modify the `admin` attribute:  
   ```
   O:4:"User":1:{s:5:"admin";b:1;}
   ```
   - Changing `b:0` → `b:1` sets **admin privileges to true**.  
3. Click **Apply Changes**.  
   - Burp Suite will automatically **re-encode** the object into Base64 and URL encoding.  

---

### **Step 4: Send the Modified Request**  
1. Send the modified request with the **updated session cookie**.  
2. Check the response.  
   - If successful, you should now see a **link to the admin panel (`/admin`)**, confirming that you have admin access.  

---

### **Step 5: Access the Admin Panel**  
1. Change the **request path** to:  
   ```
   GET /admin HTTP/1.1
   ```
2. Send the request.  
3. Observe that the `/admin` page contains options to **delete user accounts**.  

---

### **Step 6: Delete the User "Carlos"**  
1. Change the request path to:  
   ```
   GET /admin/delete?username=carlos HTTP/1.1
   ```
2. Send the request.  
3. If successful, **Carlos' account is deleted**, and you have solved the lab!  

