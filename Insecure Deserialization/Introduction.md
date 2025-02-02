**What is serialization?**

Serialization is the process of **converting complex data structures**, such as objects and their fields, **into a "flatter" format** that can be sent and received as a sequential stream of bytes.
In other words "Serialization is the process of converting an object (like a Python dictionary, a Java object, or a JSON structure) into a format that can be easily stored or transmitted (e.g., JSON, XML, or binary data)."

**Serializing data makes it much simpler to:**

 - Write complex data to inter-process memory, a file, or a database
 - Send complex data, for example, over a network, between different components of an application, or in an API call
 - Crucially, when serializing an object, its state is also persisted. In other words, the object's attributes are preserved, along with their assigned values.

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
