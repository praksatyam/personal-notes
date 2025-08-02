
## OPTIONS Method

- Used in CORS (Cross-Origin Resource Sharing - Same Origin Policy) 
- Safety mechanism that prevents webpages from accessing resources like API's, images, scripts from different domains or subdomains. Preventing malicious websites from accessing sensitive data on other sites without permission.


- Browsers generally have a 'same origin policy', which blocks access, but this can be modified using CORS to bypass it safely and selectively. 
- Browsers employ a 'same origin policy', which blocks request/access from different domains. CORS enables developers to modify this safely, selectively. 

- `OPTIONS` requests is used to fetch the capabilities of a servers CORS, to check if the actual request will be permitted, preventing potential issues with complex requests that might have unexpected side effects.

How : 

1. Simple Request:
2. Pre-flight Request:
	To be qualify as a preflight request it can be either: 
	1. The method is not GET, POST, or HEAD (so it is a PUT, DELETE)
	2. Non-simple headers (Authorization, X-custom header)
	3. Has a content type other than the general content types(application/x-www-form-urlencoded, multimart/form-data, text/plain)
	Made using `OPTIONS`:
	![[Screenshot 2025-08-01 at 17.53.26.png|500]]
	
- CORS relies on HTTP headers. When a browser makes a cross-origin request, it includes an "Origin" header specifying the origin of the request. The server then responds with "Access-Control-Allow-Origin" headers, indicating which origins are permitted to access its resources. The browser then checks these headers to determine if the request should be allowed.




## Reponses:
![[Screenshot 2025-08-01 at 17.43.47.png | 300]]


## Caching: 
- Use ETAG for caching:
	![[Screenshot 2025-08-01 at 17.56.39.png|500]]


## Content Negotiation:
- Client Server agree on the correct format to exchange request data


## HTTP Compression:

Different when using gzip compression: ![[Screenshot 2025-08-01 at 18.07.29.png]]


## Large Files:
- Client Sending Data could use Multipart Requests for file upload, in the request we contain a delimiter on the ContentType header. 
- Streaming Response (Real-time Updates) The server continues to send data in small manageable chunks. 


## SSL -> TLS:
- TLS is the new version of SSL


