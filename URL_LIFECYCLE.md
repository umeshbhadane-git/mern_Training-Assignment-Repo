Lets take an Example URL such as,

https://www.google.com

Step 1 :- The Browser Scans URL
So when we type the above URL on the browser the Browser sees,
Scheme = https
domain name = google.com


Step 2 :- DNS Resolution
The browser need the IP Address of the Sever to Communication with it.

    Step 1. First the Browser Checks its Own DNS Cache, whether it Already Knows the IP Address

    Step 2. If the Browser does not know the IP Address the the request goes to the Operating System.
            The Operating System has the DNS cache, if the OS has the IP address stored in the DNS cache then 
            the searching for the IP Address is Stoped.

    Step 3. If the computer does not have the IP Address then it ask DNS Resolver for this.
            The Resolver ask the Root DNS Server, 
                Who Handles .com domain ?
            The Root DNS Server asks the " .com TLD Server " for this.
            So now the Resolver Ask the .com TLD Server that
                Who is responsible for google.com ?
            The TLD server knows which " authoritative DNS server " is responsible.
            So now the Resolver ask the Authoritative DNS server for the IP Address
                What is the IP address of www.google.com?
            So the Authoritive Server has the Actual DNS Records, 
            So it Returns the IP Address " 12.234.xx.xx " is returned to the Browser.

            The IP address is then returned to the browser.

            If DNS resolution fails, the browser cannot determine where to send the request, 
            so the connection cannot proceed.

Step 3 :- TCP connection is Established

The browser establishes a TCP connection with the server using the TCP Handshake 

                  Client                                  Server
                    │                                       │
                    │ ----------- SYN --------------------> │
                    │                                       │
                    │ <-------- SYN + ACK ----------------- │
                    │                                       │
                    │ ----------- ACK --------------------> │
                    │                                       │
                    │        TCP connection established     │

    STEP 1.
        The client sends the SYN (Synchronize) packet.
        The SYN contains an initial sequence number, which is used to keep track of bytes sent and received.
        eg = client
            Sequence Number = 100

    STEP 2. 
        The Server sends the SYN + ACK
        It send the same initial sequence number = 100 and ask for 101 

        For example:
                Client sequence = 100

                Server:
                ACK = 101

        The server is essentially saying:
            "I received sequence number 100. I expect 101 next."
    
    STEP 3. Client sends ACK

        The client receives the server's SYN + ACK and responds:

        Client ---> ACK ---> Server

        This means:

        "I received your SYN. We can start communicating."

    WE need this handshake because the TCP is the connection Oriented Protocol .
    Before sending application data, both sides need to establish that,
    The client can reach the server and the server can reach the client.

    
Step 4 :- TLS Handshake

    Because the URL uses HTTPS, TLS is used to secure the communication.

    During the TLS handshake, the browser and server establish encryption keys, and the browser verifies the server's certificate.
    Secure encrypted connection is established.

    After TLS is established, HTTP data can be securely exchanged.

Step 5 :- HTTP Request

    The browser sends an HTTP request to the server.

    GET / HTTP/...
    Host: www.google.com

Step 6 :- Server processes the Request

    The server receives the request and determines what response should be returned.

    The request may pass through different application components:

    Load Balancer
        ↓
    Web/Application Server
        ↓
    Application Logic
        ↓
    Database / Other Services

    The server then creates an HTTP response.

STEP 7 :- HTTP Response

    The server sends a response containing a status code, headers, and usually some content.

    For example:

    HTTP/1.1 200 OK
    Content-Type: text/html

    <html>
    ...
    </html>

    200 OK means the request was successfully handled.

    If the server returns a 301 -> Permanent redirect, the browser follows the Location header and makes a new request to the redirected URL.

Step 8 :- Browser Receives and Parses the Response

    If the response contains HTML, the browser starts parsing it and builds the DOM (Document Object Model).

                                   HTML
                                    ↓
                                HTML Parser
                                    ↓
                                   DOM

    While parsing the HTML, the browser may discover additional resources such as:

    CSS, JavaScript, Images, Fonts

    It can make additional HTTP requests to retrieve these resources.

Step 9 :- CSS and JavaScript Processing

    The browser parses CSS and creates the CSSOM.

    HTML → DOM
    CSS  → CSSOM

    CSS Object Model (CSSOM) is a tree-like map of all CSS styles on a web page combined with a set of APIs that allow JavaScript to read and modify those styles dynamically.

    The DOM and CSSOM are combined to determine what should appear on the page.

    JavaScript can also modify the DOM and CSSOM and trigger additional network requests.

Step 10 :- Rendering the Page

    Finally, the browser performs the rendering process:

                               HTML
                                ↓
                            DOM + CSSOM
                                ↓
                            Render Tree
                                ↓
                              Layout
                                ↓
                              Paint
                                ↓
                        Display on Screen

        DOM 

        HTML
        │
        └── html
            │
            ├── head
            │   ├── meta
            │   └── link
            │
            └── body
                │
                ├── p
                │   ├── Hello
                │   │
                │   ├── span                        
                │   │   └── web performance
                │   │
                │   └── students
                │
                └── div
                    └── img


        CSSOM

        CSS
        │
        └── body
            │
            └── font-size: 16px
                │
                ├── p
                │   ├── font-size: 16px
                │   └── font-weight: bold
                │
                ├── span
                │   ├── font-size: 16px
                │   └── display: none
                │
                └── img
                    ├── font-size: 16px
                    └── float: right


        Resultant Styles
        │
        └── body
            │
            ├── font-size: 16px
            │
            ├── p
            │   ├── font-size: 16px
            │   ├── font-weight: bold
            │   │
            │   ├── Hello
            │   │
            │   └── students
            │
            └── div
                │
                └── img
                    |── font-size: 16px
                    └── float: right

    The browser calculates where elements should appear, paints them, and displays the webpage.