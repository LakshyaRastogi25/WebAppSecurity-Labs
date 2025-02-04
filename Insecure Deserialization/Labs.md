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
----
# **Lab2: Modifying serialized data types**  

## **Lab Summary**  
This lab demonstrates how **insecure deserialization** in PHP can be exploited to gain **admin privileges** by modifying serialized session data. By changing a username field and altering the data type of an access token, we escalate our privileges and gain access to the admin panel.  

---

## **Steps to Exploit the Vulnerability**  

### **Step 1: Log in and Identify the Session Cookie**  
1. Log in using your own credentials.  
2. In **Burp Suite**, open the **GET /my-account** request after logging in.  
3. Locate the **session cookie** in the request headers.  
4. Use **Burp Inspector** to **decode** the session cookie.  

#### **Decoded PHP Serialized Object (Example Before Modification)**  
```
O:4:"User":2:{s:8:"username";s:4:"user";s:12:"access_token";s:10:"random1234";}
```
- `O:4:"User"` → Object of class `User`.  
- `s:8:"username";s:4:"user";` → Username field (4-character string "user").  
- `s:12:"access_token";s:10:"random1234";` → Access token (10-character string "random1234").  

---

### **Step 2: Modify the Session Cookie to Escalate Privileges**  
1. **Send the request to Burp Repeater.**  
2. **Modify the session cookie using Burp's Inspector panel:**  
   - **Change the username to `"administrator"`** and update its length to **13** characters.  
   - **Change the access token to an integer (`0`)** instead of a string:  
     - **Remove the double quotes** around the value.  
     - **Update the type label from `s` (string) to `i` (integer).**  

#### **Modified Serialized Object**  
```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```
- `s:13:"administrator";` → Username is now **"administrator"** (13-character string).  
- `s:12:"access_token";i:0;` → Access token is now **integer 0** (not a string).  

---

### **Step 3: Apply Changes and Send the Request**  
1. Click **"Apply changes"** in Burp.  
   - Burp will **re-encode** the object automatically.  
2. **Send the modified request.**  
3. Check the response.  
   - If successful, the response should now include a **link to the admin panel (`/admin`)**, confirming that you have **admin access**.  

---

### **Step 4: Access the Admin Panel**  
1. Change the **request path** to:  
   ```
   GET /admin HTTP/1.1
   ```
2. Send the request.  
3. The **admin panel** should now be accessible.  
4. The page contains **links to delete specific user accounts**.  

---

### **Step 5: Delete the User "Carlos"**  
1. Change the request path to:  
   ```
   GET /admin/delete?username=carlos HTTP/1.1
   ```
2. Send the request.  
3. If successful, **Carlos' account is deleted**, and the lab is solved!  
----
