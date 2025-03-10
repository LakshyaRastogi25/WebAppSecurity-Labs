# Write-up: Exploiting an API endpoint using documentation

In this lab, we explore an API vulnerability that allows unauthorized access to sensitive information and user deletion. The objective is to analyze the API structure, retrieve documentation, and exploit it to delete the user *Carlos*.

#### **Step 1: Logging in and Modifying User Information**
1. Open Burp’s browser and log in to the application using the credentials `wiener:peter`.
2. Navigate to the account settings and attempt to update the email address.
3. In Burp Suite, go to **Proxy > HTTP history** and locate the `PATCH /api/user/wiener` request.
4. Right-click the request and select **Send to Repeater** for further testing.

#### **Step 2: Manipulating API Endpoints**
5. Go to the **Repeater** tab and send the `PATCH /api/user/wiener` request. The response includes user credentials for *wiener*.
6. Modify the request by removing `/wiener` from the endpoint, changing it to `/api/user`, then send the request again. This results in an error due to the missing user identifier.
7. Further modify the request by removing `/user`, making the endpoint `/api`, then send it. This exposes API documentation.

#### **Step 3: Accessing and Using API Documentation**
8. Right-click the response in **Repeater** and select **Show response in browser**.
9. Copy the provided URL and open it in Burp’s browser.
10. The interactive API documentation is displayed, providing access to various API functionalities.

#### **Step 4: Exploiting the API to Delete a User**
11. In the API documentation interface, locate the **DELETE** method.
12. Enter `carlos` as the target user and submit the request.
13. The user *Carlos* is deleted, successfully solving the lab.

This vulnerability highlights the risks of improper API endpoint exposure, which can lead to unauthorized data access and administrative actions. Proper authentication and endpoint validation are essential to mitigate such risks.

### **Write-up: Finding and exploiting an unused API endpoint**  

This lab demonstrates how an attacker can manipulate API endpoints to modify product prices due to improper authorization checks and misconfigured request validation.  

---

### **Step 1: Identifying the API Endpoint**
1. Open **Burp’s browser** and navigate to the lab.  
2. Click on a product to view its details.  
3. In **Burp Suite**, go to **Proxy > HTTP history** and locate the API request fetching product information, such as:  
   ```
   GET /api/products/1/price
   ```
4. Right-click the request and select **Send to Repeater** for further testing.  

---

### **Step 2: Identifying Supported HTTP Methods**
5. In the **Repeater** tab, change the request **method** from `GET` to `OPTIONS` and send the request.  
6. The response reveals that the **GET** and **PATCH** methods are allowed, meaning price modifications may be possible.  

---

### **Step 3: Checking for Authorization Requirements**
7. Change the request method from `GET` to `PATCH` and send the request.  
8. The response returns an **Unauthorized** message, indicating authentication is required to modify prices.  

---

### **Step 4: Logging in and Reattempting the Request**
9. In **Burp’s browser**, log in using the provided credentials:  
   ```
   Username: wiener  
   Password: peter  
   ```
10. Click on the **Lightweight "l33t" Leather Jacket** product.  
11. In **Proxy > HTTP history**, locate the `GET /api/products/1/price` request for the jacket and send it to **Repeater**.  
12. Change the request method from `GET` to `PATCH` and send the request.  
13. The response returns an error due to a missing `Content-Type` header.  

---

### **Step 5: Bypassing Input Validation**
14. Add the following **Content-Type** header:  
   ```
   Content-Type: application/json
   ```
15. Add an **empty JSON body** `{}` and send the request.  
16. The response returns an error indicating that a required parameter (`price`) is missing.  
17. Modify the JSON body to include the price parameter:  
   ```json
   {"price": 0}
   ```
18. Send the request. If successful, the product price is now **$0.00**.  

---

### **Step 6: Exploiting the Price Manipulation**
19. In **Burp’s browser**, reload the **Leather Jacket** product page and confirm that the price has changed to **$0.00**.  
20. Add the leather jacket to your basket.  
21. Proceed to checkout and click **Place order** to solve the lab.  

---
