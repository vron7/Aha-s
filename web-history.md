Sir Tim Berners, british scientist, invented the **World Wide Web** in 1989  
Tim had written 3 fundamental technologies that remain foundation of todays web:  
1) **HTML** ( *HyperText Markup Language* ) - a textual format to represent hypertext documents
2) **URI** ( *Uniform Resource Identifier* ) - unique 'address' to identify each resource on the web
3) **HTTP** ( *HyperText Transfer Protocol* ) - protocol that enables featching of data(resources) on the web

**WorldWideWeb** - first web browser and editor, later renamed to Nexus to avoid confusion with World Wide Web.

---
## HTTP evolution

### HTTP/0.9: (initial version)  
Request consist of a single line GET method, followed by the path to the resource (no URL)  
```GET /mypage.html```

Response was the file itself  
```<HTML>Simple HTML page</HTML>``` 

No HTTP headers, status or error codes. Only HTML files could be trasmitted.

### HTTP/1.0  
HTTP headers introduced for both response and request.  
Headers enable to transport other documents rather than just HTML (using Content-Type).

Request:
```
GET /mypage.html HTTP/1.0  
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)
```

Response: 
```
200 OK 
Date: Tue, 15 Nov 1994 08:12:31 GMT  
Server: CERN/3.0 libwww/2.17
Content-Type: text/html 
<HTML>
A page with an image
  <IMG SRC="/avatar.gif"> 
</HTML>
```

Next request to fetch image:
```
GET /avatar.gif HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)
```

Response with image:  
```
200 OK
Date: Tue, 15 Nov 1994 08:12:32 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/gif
(image content)
```

### HTTP/1.1
* Connection resue    
* Pipelining - second request can be sent before answer to the first one is fully transmitted    
* Additional cache control mechanisms    
* Chunked responses  
* Content negotiation - language, encoding, type etc  

### HTTP/2
* Binary protocol rather than text (optimization!)
* Multiplexed -parallel requests can be handled over the same connection  
* Compressed headers (removed duplication and overhead)  
* Allows a server to populate data in a client cache, in advance of it being required, through a mechanism called the server push.

--

*An **HTML form** on a web page is nothing more than a convenient user-friendly way to configure an HTTP request to send data to a server. This enables the user to provide information to be delivered in the HTTP request.

--


