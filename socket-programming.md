# Socket Programming

    - an API with two methods, send data & recieve data

    - people often visualize computer networks are peer exchanging messages
    - but the most common protocol, TCP doesn't actually produce messages
    - it produces a continious stream of bytes, with no internal boundaries
    - interpreting this byte stream is the job of the application protocol

    - data serialization
        - data over a network must be transmitted as 0s and 1s but in programming languages we represent data as high-level objects such as string, structs, lists, etc.
        - to transfer data we need to serialize it and deserialize it when were recieve it

    - concurrent programming
        - dealing with concurrent connections is difficult
        - the modern solution is event-based concurrency with event loops

    - layers of protocol
        - networks protocols are divided into layers, a lower layer can contain a higher layer as payload, and the higher layer adds new functions
        - ethernet contains ip, ip contains UDP or TCP, UDP or TCP contains application protocols

        - layer of small, discrete messages(IP)
            - when downloading a large file hardware cannot possibly store the whole data before forwarding, it can only handle smaller units (ip packets)
        - layer of multiplexing (port number)
            - multiple apps can share the same network on a single computer, so the computer needs to figure out which packet goes to which application (demultiplexing). UDP or TCP adds a 16-bit port number to distinguish different apps
            (src_ip, src_port, dst_ip, dst_port)
        - layer of reliable & ordered bytes
            - TCP provides a layer of reliable & ordered bytes on top of ip packets, handling retransmission and reordering automatically

    - overview
        TCP (reliable & ordered bytes) -> Port in TCP/UDP (multiplex to programs) -> IP (small, discrete messages)

    - typically applications do not interact with the ip layer because multiplexing is a universal need, we only care about the source and destination address
    - ethernet is below ip and is also packet based but uses MAC addresses which do not care about IP
    - application use TCP or UDP either directly by rolling out their own protocol, or indirectly by using an implementation of a well known protocol

    - request-response protocols
        - each request message is paired with a response which we need reliable and ordered messages so we use TCP
    - packet vs. stream
        - TCP provides a byte stream, by typical maps expect messages
        - so we either need a message layer for TCP or reliablity and ordering for UDP

    - socket primitives
        - a socket is handle to refer to a connection or something else
        - a handle is a opaque integer used to refer to things that cross an API boundary
        - in linux a handle is called a file descriptior
        - socket() method allocated adn returns a socket fd(handle) which is used to create connections
        - a handle must be closed when you're done to free the associate resources on the os side

    - listening socket and connection socket
        - listening is telling the os that an app is ready to accept TCP connections from a given port. the os then returns a socket handle for apps to refer to that port. from the listening socket, apps can retrieve(accept) incoming TCP connections, which is also represented as a socket handle
        ```
        fd = socket()
        bind(fd, address)
        listen(fd)
        while True:
            conn_fd = accept(fd)
            do_something_with(conn_fd)
            close(conn_fd)
        ```
        - connect from a client
        ```
        fd = socket()
        connect(fd, address)
        do_something_with(fd)
        close(fd)

    - although TCP and UDP provide different services they share the same api
        - tcp
            - send() & recv() appends to / consumes a byte stream
        - udp
            - send() & recv() corresponds to a single packet

    - summary
        - listening to a tcp socket
            - bind() & listen()
            - accept()
            - close()
        - using a tcp socket
            - read()
            - write()
            - close()
        - create a tcp connection
            - connect()
