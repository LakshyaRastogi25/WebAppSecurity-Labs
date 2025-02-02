**What is Serialization?**

Serialization is the process of **converting complex data structures**, such as objects and their fields, **into a "flatter" format** that can be sent and received as a sequential stream of bytes.
In other words "Serialization is the process of converting an object (like a Python dictionary, a Java object, or a JSON structure) into a format that can be easily stored or transmitted (e.g., JSON, XML, or binary data)."

**Serializing data makes it much simpler to:**

 - Write complex data to inter-process memory, a file, or a database
 - Send complex data, for example, over a network, between different components of an application, or in an API call
 - Crucially, when serializing an object, its state is also persisted. In other words, the object's attributes are preserved, along with their assigned values.

**Deserialization** is the process of **restoring this byte stream** to a **fully functional replica** of the original object, in the exact state as when it was serialized. The website's logic can then interact with this deserialized object, just like it would with any other object.
In short it is the **reverse process** — taking that serialized data and converting it back into an object that the application can use.

**EXAMPLE :**
```
import pickle  

# Serialization: Convert a Python object into bytes
data = {"username": "Lakshya", "role": "admin"}
serialized_data = pickle.dumps(data)  

# Deserialization: Convert bytes back into a Python object
deserialized_data = pickle.loads(serialized_data)  
print(deserialized_data)  # Output: {'username': 'Lakshya', 'role': 'admin'}

```

**What is insecure deserialization?**

Insecure deserialization occurs when an application **blindly accepts** and **deserializes** untrusted data without **validation or sanitization**. If an attacker sends a malicious serialized object, they can execute arbitrary code, modify application logic, or escalate privileges.

It is even possible to replace a serialized object with an object of an entirely different class. Alarmingly, objects of any class that is available to the website will be deserialized and instantiated, regardless of which class was expected. For this reason, insecure deserialization is sometimes known as an **"object injection" vulnerability**.

An object of an unexpected class might cause an exception. By this time, however, the damage may already be done. Many deserialization-based attacks are completed before deserialization is finished. This means that the deserialization process itself can initiate an attack, even if the website's own functionality does not directly interact with the malicious object. For this reason, websites whose logic is based on strongly typed languages can also be vulnerable to these techniques.
