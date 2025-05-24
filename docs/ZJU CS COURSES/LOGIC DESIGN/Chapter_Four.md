## Sequential Logic


* A seqential circuit

* latch

* flip-flop

* Mealy: output depends on both the present state and the present input

* Moore: output depends only on the present state

* Synchronous: clock-controlled


* Asynchronous: any type of input

### Storage elements

* discrete event simulation

#### SR Latch

```mermaid
graph TD
    A[SR Latch] --> B[S]
    A --> C[R]
    B --> D[Q]
    C --> D
    D --> E[Q']
    E --> F[Q]
```

##### Clocked SR Latch

```mermaid

graph TD
    A[Clocked SR Latch] --> B[S]
    A --> C[R]
    B --> D[Q]
    C --> D
    D --> E[Q']
    E --> F[Q]
```         

* add a clock signal to transfer r and s

##### D Latch

* Make sure that S and R are not both 1

```mermaid
graph TD
    A[D Latch] --> B[D]
    A --> C[CLK]
    B --> D[Q]
    C --> D
    D --> E[Q']
    E --> F[Q]
    F --> G[Q']
    G --> H[Q]
```

#### Flip-Flop

* The latch timing problem

* Y should change only one time during the clock cycle

* break the closed path

##### D Flip-Flop

* Negative edge triggered

```mermaid
graph TD
    A[D Flip-Flop] --> B[D]
    A --> C[CLK]
    B --> D[Q]
    C --> D
    D --> E[Q']
    E --> F[Q]
    F --> G[Q']
    G --> H[Q]
```

##### Positive Edge Triggered D Flip-Flop

* Positive edge triggered

```mermaid
graph TD
    A[Positive Edge Triggered D Flip-Flop] --> B[D]
    A --> C[CLK]
    B --> D[Q]
```

#### Direct Input Flip-Flop

* Direct input flip-flop

```mermaid
graph TD
    A[Direct Input Flip-Flop] --> B[D]
    A --> C[CLK]
    B --> D[Q]
```

##### J-K Flip-Flop

* J-K flip-flop is a universal flip-flop

```mermaid
graph TD
    A[J-K Flip-Flop] --> B[J]
    A --> C[K]
    B --> D[Q]
```

* J=1, K=1: toggle

##### T Flip-Flop

* Connect the J and K inputs together

```mermaid
graph TD
    A[T Flip-Flop] --> B[T]
    A --> C[CLK]
    B --> D[Q]
    C --> D
    D --> E[Q']
    E --> F[Q]
    F --> G[Q']
    G --> H[Q]
```

* The difference between fil-flops and latches is that flip-flops are edge-triggered, while latches are level-triggered.

#### Basic Description of Flip-Flops

##### State Table

##### State Diagram

!!! note

    ![alt text](image.png)


##### Equivalent State Definitions

* 相同的状态可以合并
   * how to define the same state: same output procedure

!!! note

    ![alt text](image-1.png)




