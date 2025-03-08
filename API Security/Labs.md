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
