## **Backend execution patterns**

**Process (PID)**

- a set of instructions (run one by one)
- has an isolated memory (no other process can read from this memory locaiton or access it)
- has a PID (each procss has its own unique identifier)
- scheduled in the CPU

**Threads**

- a lightweight process (based on Linux) - its just a process that inherits memory from its parent (LWP)
- a set of instructions
- shares memory with parent process
- has a ID

- single threaded process
    - one process with a single thread
    - simple
    - examples node JS

- multi processes
    - app has multiple processes
    - each has its own memory
    - examples NGINX/Postgres
    - takes advantage of multi-cores

PAUSE on this topic
