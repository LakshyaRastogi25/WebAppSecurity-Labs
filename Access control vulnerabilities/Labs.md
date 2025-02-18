---
#  Lab:1 Unprotected admin functionality
## Write-up: Accessing the Admin Panel via robots.txt  

### Step 1: Discovering the Admin Panel Path  
1. Open the lab URL in your browser.  
2. Append `/robots.txt` to the URL (e.g., `https://lab-url.com/robots.txt`).  
3. The `Disallow` directive reveals a restricted path, which leads to the admin panel.  

### Step 2: Accessing the Admin Panel  
1. Replace `/robots.txt` in the URL with `/administrator-panel`.  
2. Press Enter to navigate to the admin panel.  

### Step 3: Deleting the User  
1. Locate the option to manage or delete users.  
2. Find the user "carlos" and delete their account.  
---
---
#  Lab:2 Unprotected admin functionality with unpredictable URL
## Write-up: Finding the Admin Panel URL in JavaScript  

### Step 1: Reviewing the Source Code  
1. Open the lab home page in your browser.  
2. Use **Burp Suite** (HTTP history) or your browser’s **Developer Tools** (`Ctrl + U` or `Ctrl + Shift + I` → Elements/Network tab).  
3. Look for JavaScript code that reveals the admin panel URL.  

### Step 2: Accessing the Admin Panel  
1. Copy the disclosed admin panel URL.  
2. Paste it into the browser’s address bar and load the page.  

### Step 3: Deleting the User  
1. Locate the user management section.  
2. Find and delete the user **carlos**.  

This completes the lab.

---
---
#  Lab:3 User role controlled by request parameter
## Write-up: Bypassing Admin Restriction via Cookie Manipulation  

### Step 1: Checking Admin Access  
1. Open the lab URL in your browser.  
2. Navigate to `/admin` and observe that access is restricted.  

### Step 2: Capturing and Modifying the Login Response  
1. Browse to the login page and open **Burp Suite**.  
2. In **Burp Proxy**, turn **interception on** and enable **response interception**.  
3. Enter any credentials and submit the login form.  
4. In Burp, forward the login request.  
5. Observe the server’s response, which sets a cookie:  
   ```
   Set-Cookie: Admin=false
   ```  
6. Modify the cookie value from `Admin=false` to `Admin=true`.  
7. Forward the modified response.  

### Step 3: Accessing the Admin Panel and Deleting Carlos  
1. Reload `/admin`, and you should now have access.  
2. Locate the user **carlos** and delete the account.  

This completes the lab.

---
---
#  Lab:4 User role can be modified in user profile
## Write-up: Privilege Escalation via Role ID Manipulation  

### Step 1: Logging In  
1. Use the provided credentials to log in to your account.  
2. Navigate to your **account settings** page.  

### Step 2: Identifying Role ID in the Response  
1. Use the feature to update your **email address**.  
2. Observe the server’s response, which contains your `roleid`.  

### Step 3: Modifying the Role ID  
1. Open **Burp Suite** and send the email update request to **Burp Repeater**.  
2. Modify the JSON request body to include:  
   ```json
   "roleid": 2
   ```  
3. Resend the request and check the response to confirm that `roleid` has changed to **2**.  

### Step 4: Accessing the Admin Panel and Deleting Carlos  
1. Browse to `/admin` and confirm admin access.  
2. Locate **carlos** in the user management section and delete the account.  

This completes the lab.

---
