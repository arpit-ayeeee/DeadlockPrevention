# Multi-threaded Locking Simulation with Deadlock Prevention

## Overview

This C program simulates a simple multi-threaded environment where multiple transactions attempt to acquire locks on records in a database. The goal is to demonstrate locking mechanisms using **pthread mutexes** while avoiding deadlock scenarios.

The program simulates six concurrent transactions, each attempting to acquire locks on two random records. If locks are not acquired in a consistent order, deadlock could occur, but this is prevented by always locking records in ascending order of their indices.

## Features

- **Concurrency**: Simulates multiple concurrent transactions using threads.
- **Record Locking**: Each transaction locks two random records.
- **Deadlock Prevention**: Ensures locks are always acquired in a consistent order (ascending record index) to prevent deadlocks.
- **Thread Synchronization**: Uses pthread mutexes to ensure that only one thread can modify a record at a time.

## Key Components

- **Database Structure**:
    - A database contains multiple records, each with an array of attributes and a mutex for locking.
    - Each record stores an ID and a random age.

- **Transaction Simulation**:
    - Each thread represents a transaction that tries to lock two random records, simulates some processing (using `sleep`), and then releases the locks.
    - Deadlock prevention is achieved by ensuring records are locked in ascending index order.

## Code Structure

### 1. **Data Structures**:
- `RecordData`: Holds attributes for each record.
- `Record`: Contains the data of a record and a mutex lock.
- `Database`: Contains an array of `Record` objects, representing the database.

### 2. **Functions**:
- **`initDB()`**: Initializes the database with random values and sets up mutex locks for each record.
- **`acquireLock()`**: Acquires a lock on a specified record.
- **`releaseLock()`**: Releases the lock on a specified record.
- **`mimic_load()`**: Represents a transaction where two random records are locked and unlocked.
- **`main()`**: Entry point of the program where threads are created and transactions are simulated.

### 3. **Deadlock Prevention**:
- Locks are always acquired in ascending order of record indices, ensuring that transactions don’t cause a circular wait, which is the primary cause of deadlock.

## Compilation and Execution

### Prerequisites

- GCC compiler installed
- Pthreads library (standard with most Linux distributions)

### Steps

1. **Compile the program**:

   ```bash
   gcc -o deadlock_simulation deadlock_simulation.c -lpthread
   ```

2. **Run the program**:

   ```bash
   ./deadlock_simulation
   ```

   The program will output the lock acquisition and release events for each transaction.

## Sample Output

```
txn A: wants to acquire lock on record: 1
txn A: acquired lock on record: 1
txn A: wants to acquire lock on record: 2
txn A: acquired lock on record: 2
txn A: released lock on record: 2
txn A: released lock on record: 1
txn B: wants to acquire lock on record: 0
txn B: acquired lock on record: 0
txn B: wants to acquire lock on record: 1
txn B: acquired lock on record: 1
...
```

## Potential Deadlock Fix

The original issue of deadlock is avoided by always locking records in ascending index order:

```c
if (rec1 > rec2) {
    int temp = rec1;
    rec1 = rec2;
    rec2 = temp;
}
```

This ensures that no circular waits occur, which is a common cause of deadlocks.
