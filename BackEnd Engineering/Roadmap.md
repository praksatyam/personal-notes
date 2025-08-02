- [02:22](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=142s) - A high-level understanding
- [02:51](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=171s) - HTTP protocol
- [04:25](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=265s) - Routing
- [05:04](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=304s) - Serialisation and deserialisation
- [07:13](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=433s) - Authentication and authorisation
- [08:45](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=525s) - Validation and transformation
- [12:03](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=723s) - Middlewares
- [14:03](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=843s) - Request context
- [15:28](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=928s) - Handlers, controllers and services
- [15:45](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=945s) - CRUD deepdive
- [16:33](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=993s) - RESTful architecture and best practices
- [17:08](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1028s) - Databases
- [17:44](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1064s) - Business logic layer (BLL)
- [18:51](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1131s) - Caching
- [20:04](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1204s) - Transactional emails
- [20:19](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1219s) - Task queuing and scheduling
- [21:35](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1295s) - Elasticsearch
- [22:33](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1353s) - Error handling
- [23:16](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1396s) - Config management
- [24:07](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1447s) - Logging, monitoring and observability
- [25:13](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1513s) - Graceful shutdown
- [25:50](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1550s) - Security
- [26:23](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1583s) - Scaling and performance
- [27:36](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1656s) - Concurrency and parallelism
- [27:47](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1667s) - Object storage and large files
- [27:59](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1679s) - Real-time backend systems
- [28:06](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1686s) - Testing and code quality
- [28:50](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1730s) - 12 factor app
- [28:55](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1735s) - OpenAPI standards
- [29:58](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1798s) - Webhooks
- [30:39](https://www.youtube.com/watch?v=0Rwb4Xmlcwc&t=1839s) - DevOps for backend engineers


https://www.youtube.com/watch?v=3qFjZbFRSAU&list=PLui3EUkuMTPgZcV0QhQrOcwMPcBCcd_Q1&index=2


# 1. Random Wisdom:

HTTPs and HTTP
- HTTPS, through port 443, uses protocols like TLS/SSL to encrypt data, making it unreadable to anyone who might intercept the traffic
- Many websites now redirect traffic from port 80 to port 443 to ensure secure connections, but port 80 is still used for the initial request and often for redirection purposes
- HTTP over port 80 transmits data in plain text, meaning it's not encrypted. This makes it susceptible to eavesdropping and potential security risks.

CORS policy:
- Browsers Isolate environments
- Only fetch/call resources that follow within the CORS policy. Here this is mostly the headers that are blocked.
- Sandboxing and Security Restrictions