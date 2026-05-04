# 🎫 Queue Line System — C++ Data Structures Project

![C++](https://img.shields.io/badge/Language-C%2B%2B-blue?style=flat-square&logo=cplusplus)
![Type](https://img.shields.io/badge/Type-Data%20Structures%20from%20Scratch-teal?style=flat-square)
![OOP](https://img.shields.io/badge/Paradigm-OOP%20%2B%20Templates-blueviolet?style=flat-square)
![Pattern](https://img.shields.io/badge/Pattern-Queue%20%2F%20Stack%20%2F%20LinkedList-orange?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

A real-world **Queue Line ticketing system** (like bank or hospital queues) built on top of custom-implemented data structures from scratch — Doubly Linked List, Generic Queue, Generic Stack — all built without using STL containers internally.

---

## 📸 Output Preview

```
            _________________________
                    Queue Info
            _________________________
                Prefix          = A0
                Total Tickets   = 5
                Served Clients  = 0
                Waiting Clients = 5
            _________________________

        Tickets:  A01 <--  A02 <--  A03 <--  A04 <--  A05 <--
        Tickets:  A05 -->  A04 -->  A03 -->  A02 -->  A01 -->

                       ---Tickets---

                  _______________________

                        A01
                    23/04/2026 10:15:42
                    Waiting Clients = 0
                      Serve Time In
                       0 Minutes.
                  _______________________
                  ...
```

---

## ✨ Features

- 🎫 **Issue tickets** with auto-increment number, prefix, timestamp, and expected wait time
- 👁️ **Print tickets line** left-to-right and right-to-left
- 🧾 **Print all tickets** with full details (time issued, waiting clients, expected serve time)
- ✅ **Serve next client** — FIFO order (first in, first out)
- 📊 **Queue info** — total tickets, served, and currently waiting
- 🔢 **Multiple independent queues** — each with its own prefix and average serve time
- ⏱️ **Expected serve time** per ticket = `waiting clients × average serve time`

---

## 🗂️ Project Structure

```
QueueLineProject/
│
├── main.cpp                  # Demo — PayBills and Subscriptions queues
│
├── clsQueueLine.h            # Main queue line system (uses std::queue + stack)
│
├── clsDblLinkedList.h        # Custom Doubly Linked List (template)
├── clsMyQueue.h              # Custom Generic Queue (built on clsDblLinkedList)
├── clsMyStack.h              # Custom Generic Stack (inherits clsMyQueue)
├── clsMyQueueArr.h           # Array-based Queue implementation
├── clsMyStackArr.h           # Array-based Stack implementation
│
├── clsMyString.h             # String class with Undo/Redo (from prev project)
├── clsString.h               # String utility class
├── clsDate.h                 # Date/time utility (used for ticket timestamp)
└── README.md
```

---

## 🧱 Data Structures Architecture

The project builds each structure on top of the previous one:

```
clsDblLinkedList<T>          ← built from scratch with raw pointers
        │
        └──► clsMyQueue<T>   ← Queue on top of Doubly Linked List
                  │
                  └──► clsMyStack<T>   ← Stack inherits Queue (overrides push)
```

### clsDblLinkedList\<T\> — Doubly Linked List

A fully generic doubly linked list with:

| Method | Description | Time |
|---|---|---|
| `InsertAtBeginning(T)` | Insert node at head | O(1) |
| `InsertAtEnd(T)` | Insert node at tail | O(n) |
| `InsertAfter(Node*, T)` | Insert after a given node | O(1) |
| `InsertAfter(index, T)` | Insert after index | O(n) |
| `DeleteNode(Node*&)` | Delete a specific node | O(1) |
| `DeleteFirstNode()` | Delete head | O(1) |
| `DeleteLastNode()` | Delete tail | O(n) |
| `Find(T)` | Find node by value | O(n) |
| `GetNode(index)` | Get node by index | O(n) |
| `GetItem(index)` | Get value by index | O(n) |
| `UpdateItem(index, T)` | Update value at index | O(n) |
| `Reverse()` | Reverse the list in-place | O(n) |
| `Clear()` | Delete all nodes | O(n) |
| `Size()` / `IsEmpty()` | Size info | O(1) |
| `PrintList()` | Print all values | O(n) |

Each `Node` holds: `Value`, `Next*`, `Prev*`

---

### clsMyQueue\<T\> — Generic Queue

Built on `clsDblLinkedList` — wraps it with Queue semantics (FIFO):

| Method | Description | Time |
|---|---|---|
| `push(T)` | Enqueue at back | O(n)* |
| `pop()` | Dequeue from front | O(1) |
| `front()` | Peek at front | O(1) |
| `back()` | Peek at back | O(n)* |
| `IsEmpty()` / `Size()` | Info | O(1) |
| `Reverse()` | Reverse queue | O(n) |
| `InsertAtFront(T)` | Priority insert | O(1) |
| `InsertAfter(index, T)` | Insert at position | O(n) |
| `UpdateItem(index, T)` | Update at position | O(n) |
| `Clear()` | Empty the queue | O(n) |

> *`InsertAtEnd` traverses to tail since there's no tail pointer — improvable with O(1) by adding a `Tail` pointer

---

### clsMyStack\<T\> — Generic Stack

Inherits `clsMyQueue<T>` and overrides `push` to insert at the beginning (LIFO):

```cpp
class clsMyStack : public clsMyQueue<T> {
    void push(T Value) {
        _MyList.InsertAtBeginning(Value); // LIFO — newest at front
    }
    T Top()    { return front(); }
    T Bottom() { return back();  }
};
```

| Method | Description | Time |
|---|---|---|
| `push(T)` | Push to top | O(1) |
| `pop()` | Pop from top | O(1) |
| `Top()` | Peek at top | O(1) |
| `Bottom()` | Peek at bottom | O(n)* |

---

### clsQueueLine — The Ticket System

Uses `std::queue<clsTicket>` internally and wraps it with:

| Method | Description |
|---|---|
| `IssueTicket()` | Creates a ticket with number, time, waiting count, expected serve time |
| `ServeNextClient()` | Pops front of queue (FIFO) |
| `WhoIsNext()` | Returns full ticket number of next client |
| `WaitingClients()` | Returns current queue size |
| `ServedClients()` | Total issued - currently waiting |
| `PrintInfo()` | Shows queue stats |
| `PrintTicketsLineRTL()` | Prints queue left-to-right using temp copy |
| `PrintTicketsLineLTR()` | Prints queue right-to-left using temp stack |
| `PrintAllTickets()` | Prints full ticket card for each client |

**Inner class `clsTicket`** holds:
- Ticket number + prefix → `FullNumber()` e.g. `"A03"`
- Issue timestamp from `clsDate::GetSystemDateTimeString()`
- Waiting clients count at time of issue
- `ExpectedServeTime()` = `waitingClients × averageServeTime`

---

## 🚀 Getting Started

### Compile & Run

```bash
g++ main.cpp -o QueueLine
./QueueLine
```

**Or in Visual Studio:** Open the `.sln` → `Ctrl + F5`

### Usage Example

```cpp
#include "clsQueueLine.h"

clsQueueLine PayBillsQueue("A0", 10);   // prefix "A0", 10 min avg serve time
clsQueueLine SubsQueue("B0", 5);        // prefix "B0", 5 min avg serve time

PayBillsQueue.IssueTicket();  // A01 — 0 waiting, serve in 0 min
PayBillsQueue.IssueTicket();  // A02 — 1 waiting, serve in 10 min
PayBillsQueue.IssueTicket();  // A03 — 2 waiting, serve in 20 min

PayBillsQueue.PrintInfo();
PayBillsQueue.PrintAllTickets();
PayBillsQueue.ServeNextClient();  // A01 served
```

---

## 📊 Big O Summary

| Operation | Time | Space |
|---|---|---|
| `IssueTicket()` | O(n) — InsertAtEnd traverses to tail | O(1) |
| `ServeNextClient()` | O(1) — DeleteFirstNode | O(1) |
| `WhoIsNext()` / `WaitingClients()` | O(1) | O(1) |
| `PrintTicketsLineRTL()` | O(n) | O(n) — temp copy |
| `PrintTicketsLineLTR()` | O(n) | O(n) — temp queue + stack |
| `PrintAllTickets()` | O(n) | O(n) — temp copy |

> 💡 `IssueTicket` could be O(1) by adding a `Tail*` pointer to `clsDblLinkedList`

---

## 🔮 Possible Improvements

- [ ] Add `Tail*` pointer to `clsDblLinkedList` → make `InsertAtEnd` O(1)
- [ ] Add **VIP / priority queue** — insert at front for urgent clients
- [ ] Add **estimated wait display** that updates as clients are served
- [ ] Add **multi-counter support** — distribute clients across counters
- [ ] Store ticket history to a log file

---

## 👨‍💻 Author

> Built with ❤️ as part of a C++ Algorithms & Data Structures learning journey.

Feel free to fork, star ⭐, or contribute!

---

## 📄 License

This project is licensed under the **MIT License** — free to use and modify.
