# mq-benchmark-js

## Prerequisites

- NodeJS v8.9 or later
- npm

## How to use

1. Install dependencies

    ```
    npm install
    ```

2. Run a receiver node

    ```
    node src/benchmark.js <TEST_MODE> receiver --mq <MESSAGE_QUEUE_LIB>
    ```

    **Example:**

    ```
    node src/benchmark.js uniform receiver --mq libp2p
    ```


3. Run a broker node

    ```
    node src/benchmark.js <TEST_MODE> broker --mq <MESSAGE_QUEUE_LIB> --receiverIp <RECEIVER_IP>
    ```

    **Example (Uniform):**

    ```
    node src/benchmark.js uniform broker --mq libp2p --receiverIp 127.0.0.1
    ```

    **Example (Poisson):**

    ```
    node src/benchmark.js poisson broker --mq libp2p --receiverIp 127.0.0.1
    ```

4. Run a sender node

    #### Uniform

    ```
    node src/benchmark.js <TEST_MODE> sender --mq <MESSAGE_QUEUE_LIB> -s <MESSAGE_SIZE_IN_BYTES> -c <MESSAGE_COUNT> -d <DURATION_IN_SECONDS> --brokerIp <BROKER_IP>
    ```

    **Example:**

    ```
    node src/benchmark.js uniform sender --mq libp2p -s 1000 -c 10000 --brokerIp 127.0.0.1
    ```

    ```
    node src/benchmark.js uniform sender --mq libp2p -s 1000 -d 3 --brokerIp 127.0.0.1
    ```

    #### Poisson

     ```
    node src/benchmark.js <TEST_MODE> sender --mq <MESSAGE_QUEUE_LIB> -c <MESSAGE_COUNT>  --avgSize <AVERAGE_MESSAGE_SIZE_IN_BYTES> --avgDelay <AVERAGE_DELAY_IN_MICROSECONDS> --brokerIp <BROKER_IP>
    ```

    **Example:**

    The command below: stop send when 10000 messages sent OR some 10 seconds after first message sent

    avgDelay is in microsecond, min 1, max 1000

    ```
    node src/benchmark.js poisson sender --mq libp2p -c 10000 --duration 10 --avgSize 1000 --avgDelay 1000 --brokerIp 127.0.0.1
    ```

Benchmark result will be printed to the console on sender and receiver nodes.

## Use with Docker

- Build an image
    
    ```
    docker build -t green-lantern/mq-benchmark-js:0.1 .
    ```

- Receiver

    **Example (Uniform):**

    ```
    docker run -it --rm \
      -e TEST_MODE='uniform' \
      -e ROLE='receiver' \
      -e MESSAGE_QUEUE='libp2p' \
      green-lantern/mq-benchmark-js:0.1
    ```

- Broker
    
    **Example (Uniform):**

    ```
    docker run -it --rm \
      -e TEST_MODE='uniform' \
      -e ROLE='broker' \
      -e MESSAGE_QUEUE='libp2p' \
      -e RECEIVER_IP='172.17.0.2' \
      green-lantern/mq-benchmark-js:0.1
    ```

- Sender

    **Example (Uniform):**

    ```
    docker run -it --rm \
      -e TEST_MODE='uniform' \
      -e ROLE='sender' \
      -e MESSAGE_QUEUE='libp2p' \
      -e MESSAGE_COUNT='1000' \
      -e MESSAGE_SIZE='1000' \
      -e BROKER_IP='172.17.0.3' \
      green-lantern/mq-benchmark-js:0.1
    ```

     ```
    docker run -it --rm \
      -e TEST_MODE='uniform' \
      -e ROLE='sender' \
      -e MESSAGE_QUEUE='libp2p' \
      -e MESSAGE_COUNT='1000' \
      -e DURATION='3' \
      -e BROKER_IP='172.17.0.3' \
      green-lantern/mq-benchmark-js:0.1
    ```
