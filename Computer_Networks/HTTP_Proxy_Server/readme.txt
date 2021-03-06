HTTP Server with Cache
Author: Swati Singh

This README documents the basic design and the usage for the Project: HTTP Proxy Server.

##################################################
USAGE:

The server can be compiled using:
make

The server should be launched as follows:
./server IPAddress PortNumber

The client should be launched as follows:
./client IPAddress PortNumber WebpageURL

HTTP Clients should connect to the server through the same IPAddress and Port Number.

##################################################
ALGORITHM

proxyserver.cpp:
The program for HTTP proxy server can be found in proxyserver.cpp
It handles socket creation, binding and listeing to an IP Address specified through command line, handling read requests for webpages from the clients, storing the data received from the main server in the cache for future use.
If the page has not been modified on the server, then the proxyserver sends the page stored in cache.
If the page has been modified at the server, then it gets the page from the server to serve the request of the client.
To check for the modified time of the pages, it used HEAD method to compare the headers of the page stored on cache and that on the server.

The basic algorithm implemented is as follows:
If Page found in cache,
  If Expired field is found  
     Then If Page has expired, 
           If Last Modified Field is Found
              If Last Modified Field is same for server page and cache page
                  Get Expired Field from Server page, update in cache and serve page from cache.       
                  Update Last USed time to Current time.
              Else
                  Get Page from Server and replace in cache. Send to Client                      
           Else
             Get Page from Server and replace in cache. Send to Client                      
     If Page is Not expired,
        Send the Page stored in Cache
        Update Last USed time to Current time.
  Else if Last Modified Field is Found
     If page has been modified at server
          Get Page from Server and replace in cache. Send to Client        
     Else if page not modified at Server
          Send the page stored in cache to Client.
          Update Last Used time to Current time.
  Else
     Get the page from Server, replace in Cache. Send to Client
Else if Page not found in cache
  Get page from server, add to cache, send to client.               

client.cpp:
The program for HTTP client can be found in client.cpp
It uses GET command to request for a webpage from the proxyserver.

defn.h
This stores some basic constants used by both proxyserver and client program.

##################################################



