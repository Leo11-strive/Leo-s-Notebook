```mermaid
flowchart TD

A0([Start]) --> A1[Create socket(AF_INET, SOCK_STREAM, 0)]
A1 --> A2[setsockopt(SO_REUSEADDR)]
A2 --> A3[Bind to port 5000]
A3 --> A4[Listen(backlog=128)]
A4 --> A5{{Main Loop: accept() new client}}

A5 -->|New connection| B1[Allocate memory for new socket fd]
B1 --> B2[add_client(): record fd, id, IP, port]
B2 --> B3[Create thread(thread_function)]
B3 --> A5


subgraph "thread_function (each client)"
B3 --> C1[Detach thread]
C1 --> C2[Receive message using receive_message()]
C2 --> C3{{Parse command}}

C3 -->|TIME| C4[time() + ctime() → send current time]
C3 -->|NAME| C5[gethostname() → send server name]
C3 -->|LIST| C6[Lock clients_mutex → enumerate all clients → Unlock]
C6 --> C7[send_message() with list result]
C3 -->|MSG id content| C8[Find target client id → forward message]
C3 -->|EXIT| C9[break loop → close socket]
C3 -->|Unknown| C10[send 'Unknown command']

C4 --> C2
C5 --> C2
C7 --> C2
C8 --> C2
C10 --> C2
C9 --> D1[remove_client(fd)]
D1 --> D2[close(fd)]
D2 --> D3([Thread exit])
end


```
