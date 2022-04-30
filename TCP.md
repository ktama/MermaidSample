# StateDiagram

```mermaid
stateDiagram-v2
    [*] --> CLOSED
    CLOSED --> LISTEN
    state PassiveOpen {
        LISTEN --> SYN_RECEIVED: SYN受信, SYN/ACK送信
    }
    CLOSED --> SYN_SENT: SYN送信
    state ActiveOpen {
        SYN_SENT
    }
    SYN_SENT --> ESTABLISHED: SYN/ACK受信, ACK送信
    state CONNECTING {
        ESTABLISHED
    }
    SYN_RECEIVED --> ESTABLISHED
    ESTABLISHED --> FIN_WAIT1: 閉鎖, FIN送信
    state ActiveClose {
        FIN_WAIT1 --> FIN_WAIT2: FINのACK受信
        FIN_WAIT1 --> CLOSING: FIN受信, ACK送信
        CLOSING --> TIME_WAIT: FINのACK受信
        FIN_WAIT2 --> TIME_WAIT: FIN受信, ACK送信
    }
    TIME_WAIT --> CLOSED
    ESTABLISHED --> CLOSE_WAIT: FIN受信, ACK送信
    state PassiveClose {
        CLOSE_WAIT --> LAST_ACK: 閉鎖, FIN送信
    }
    LAST_ACK --> CLOSED: FINのACK送信
```

[TCPの状態遷移 - Qiita](https://qiita.com/mogulla3/items/196124b9fb36578e5c80)

# Sequence

## 接続

```mermaid
sequenceDiagram
    Client -->> Server: SYN
    Server -->> Client: SYN/ACK
    Client -->> Server: ACK
```

## クライアントから切断

```mermaid
sequenceDiagram
    Client -->> Server: FIN/ACK
    Server -->> Client: FIN/ACK
    Client -->> Server: ACK
```

## サーバーから切断

```mermaid
sequenceDiagram
    Server -->> Client: FIN/ACK
    Client -->> Server: FIN/ACK
    Server -->> Client: ACK
```
