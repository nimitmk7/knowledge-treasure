The protocol that governs the permissions granted by a web server to web browsers for accessing resources from different domains is called **Cross-Origin Resource Sharing (CORS)**.

CORS is a security feature implemented in web browsers to prevent potentially malicious websites from accessing data from other sites without permission. It dictates how a web server can explicitly allow or deny requests from origins (domains, protocols, and ports) different from its own.

When a browser makes a cross-origin request, it first checks with the server by sending an **OPTIONS** request, called a "preflight request," to verify if the server allows the method and headers that the request wants to use. 

The server then responds with headers specifying the allowed methods, headers, and origin domains if it permits cross-origin access.

Key headers used in CORS include:

- **Access-Control-Allow-Origin**: Specifies the domains allowed to access the resources.
- **Access-Control-Allow-Methods**: Specifies HTTP methods allowed for cross-origin requests.
- **Access-Control-Allow-Headers**: Specifies headers that can be used in requests.

If the response headers permit access, the browser proceeds with the actual request. Otherwise, the browser restricts the access to protect the user's data.