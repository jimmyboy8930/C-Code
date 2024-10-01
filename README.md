# Producer-Consumer Multi-Threaded Buffer with Linked List

## Project Overview

This project is a multi-threaded C implementation of the **Producer-Consumer problem** using a **two-dimensional buffer** and **linked list**. The program demonstrates synchronization and concurrency using POSIX threads (`pthread`) and implements inter-thread communication using **mutexes** and **condition variables**.

### Key Features:
- **Producer-Consumer Logic**: Multiple producer and consumer threads operating concurrently on a shared two-dimensional buffer.
- **Custom Linked List**: Used to manage and store tasks for the producers and consumers.
- **Thread Synchronization**: Implemented using mutexes and condition variables to avoid race conditions and ensure correct data sharing between threads.
- **Barrier Synchronization**: Ensures that all threads start concurrently.
- **Buffer with Columns**: A unique approach where different columns in a 2D buffer represent different data types, processed independently by producers and consumers.

## Implementation Details

### Core Structures:

- **list_t**: Represents the linked list with a head and tail pointer. The list is protected by a `pthread_mutex_t` to ensure thread-safe access.
- **two_d_buffer**: A 2D buffer where producers place items and consumers retrieve them. This buffer includes mutexes and condition variables to synchronize access.
    - `buf[MAX][3]`: Stores the items produced.
    - `counts[3]`: Keeps track of how many items have been added to each column in the buffer.
    - `produce_cond` and `consume_cond`: Condition variables to notify producers and consumers when they can add/remove items from the buffer.

### Functions Explained:

- **add_to_buffer(int item, int col, two_d_buffer *p)**:
    - Adds an item to a specific column in the buffer. The function waits if the buffer is full for that column.
    - Notifies consumers once the item is added.

- **remove_from_buffer(int *a, int *b, int *c, two_d_buffer *p)**:
    - Removes one item from each column of the buffer and provides them to the consumer.
    - Notifies producers once an item has been removed.

- **prepare(int item)**:
    - Simulates work by sleeping for a brief period based on the item value.

### Producer and Consumer Threads:
- **thread_produce(void* threadarg)**:
    - Each producer thread removes items from the linked list and places them into the shared 2D buffer.
    - It uses synchronization to ensure no two threads access the shared resources simultaneously.

- **thread_consume(void* threadarg)**:
    - Each consumer thread retrieves items from the buffer, prints them, and adds corresponding nodes to the linked list.
    - It waits for the buffer to be filled before consuming items.

### Linked List Functions:
- **create_node(int v)**: Creates a new node with the provided value.
- **add_last(node **head, node **tail, node *newnode)**: Adds a new node to the end of the linked list.
- **remove_first(node **head, node **tail)**: Removes the first node from the linked list.
- **print_list(node *head)**: Prints all the values in the linked list.

## Usage Instructions

### Compile and Run
1. **Compilation**: Compile the program using GCC with the following command:
   ```bash
   gcc -pthread -o prod_consumer prod_consumer.c linked-list.c
   ```

2. **Execution**: Run the program by specifying the number of consumers, producers, and buffer size as arguments:
   ```bash
   ./prod_consumer <num_consumers> <num_producers> <buffer_size>
   ```
   Example:
   ```bash
   ./prod_consumer 5 3 10
   ```

### Example Output
The program prints the items consumed by each consumer thread, and once all items have been consumed, it prints the total number of items processed by the producer threads.

### Project Constraints:
- The program asserts that the number of consumers and producers should not exceed 3000.
- The buffer size must not exceed the defined `MAX` (10).

## Synchronization Mechanisms
- **pthread_mutex_t**: Ensures mutual exclusion between threads when accessing shared data structures (buffer, linked list).
- **pthread_cond_t**: Used to signal between threads (producers notify consumers when an item is added and vice versa).

## Future Improvements:
- Add more robust error handling and validation.
- Implement dynamic buffer resizing for more flexible usage.
- Extend the project to support more complex producer-consumer patterns.

---

This project demonstrates proficiency in multi-threaded programming, synchronization, and managing concurrency challenges. The use of a linked list and 2D buffer adds complexity to the classic producer-consumer problem, showcasing an advanced understanding of thread communication.
