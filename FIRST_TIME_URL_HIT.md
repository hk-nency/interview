
 You type maps.google.com into the address bar of your browser.
 
1. The browser checks the cache for a DNS record to find the corresponding IP address of maps.google.com.

    ● DNS(Domain Name System) is a database that maintains the name of the website (URL) and the particular IP address it links to.
    ● Every single URL on the internet has a unique IP address assigned to it. The IP address belongs to the computer which hosts the server of the website we are requesting to access
    ● the main purpose of DNS is human-friendly navigation

    ● First, it checks the browser cache. The browser maintains a repository of DNS records for a fixed duration for websites you have previously visited. So, it is the first place to run a DNS query.
    ● Second, the browser checks the OS cache. If it is not found in the browser cache, the browser would make a system call (i.e. gethostname on Windows) to your underlying computer OS to fetch the record since the OS also maintains a cache of DNS records.
    ● Third, it checks the router cache. If it’s not found on your computer, the browser would communicate with the router that maintains its’ own cache of DNS records.

2. If the requested URL is not in the cache, ISP’s DNS server initiates a DNS query to find the IP address of the server that hosts maps.google.com.
 
 Once IP address is located.
 
 3. Browser initiates a TCP connection with the server.
 
 Once the browser receives the correct IP address it will build a connection with the server that matches IP address to transfer information.
 Browsers use internet protocols to build such connections. There are a number of different internet protocols which can be used but TCP is the most common protocol used for any type of HTTP request
 
 In order to transfer data packets between your computer(client) and the server, it is important to have a TCP connection established.
 This connection is established using a process called the TCP/IP three-way handshake. 
 
  This is a three step process where the client and the server exchange SYN(synchronize) and ACK(acknowledge) messages to establish a connection.
  1. Client machine sends a SYN packet to the server over the internet asking if it is open for new connections.
  2. If the server has open ports that can accept and initiate new connections, it’ll respond with an ACKnowledgment of the SYN packet using a SYN/ACK packet.
  3. The client will receive the SYN/ACK packet from the server and will acknowledge it by sending an ACK packet.
  Then a TCP connection is established for data transmission!
  
  
  4.  The browser sends an HTTP request to the web server.
  
  Once the TCP connection is established, it is time to start transferring data! The browser will send a GET request asking for maps.google.com web page. 
  If you’re entering credentials or submitting a form this could be a POST request. 
  This request will also contain additional information such as browser identification (User-Agent header), types of requests that it will accept (Accept header), and connection headers asking it to keep the TCP connection alive for additional requests.
  It will also pass information taken from cookies the browser has in store for this domain.
  
  
  5.The server handles the request and sends back a response.
  
  6. The server sends out an HTTP response.
  The server response contains the web page you requested as well as the status code, compression type (Content-Encoding), how to cache the page (Cache-Control), any cookies to set, privacy information, etc.

    There are five types of statuses detailed using a numerical code.
    ● 1xx indicates an informational message only
    ● 2xx indicates success of some kind
    ● 3xx redirects the client to another URL
    ● 4xx indicates an error on the client’s part
    ● 5xx indicates an error on the server’s part
    
   7. The browser displays the HTML content (for HTML responses which is the most common). 
   Then explain browser rendering cycle.
   
