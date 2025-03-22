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

