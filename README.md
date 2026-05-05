# 🎫 Queue Line System — C++ Console App

![C++](https://img.shields.io/badge/Language-C%2B%2B-blue?style=flat-square&logo=cplusplus)
![OOP](https://img.shields.io/badge/Paradigm-OOP%20%2B%20Nested%20Class-blueviolet?style=flat-square)
![STL](https://img.shields.io/badge/Uses-STL%20queue%20%2B%20stack-orange?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

A real-world **Queue Line ticketing system** (like bank or hospital queues) built in C++ using OOP and STL containers. Each ticket is issued with a timestamp, waiting count, and expected serve time.

---

## 📸 Output Preview

```
Pay Bills Queue Info:

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

                  _______________________
                        A02
                    23/04/2026 10:15:42
                    Waiting Clients = 1
                      Serve Time In
                       10 Minutes.
                  _______________________
```

---

## ✨ Features

- 🎫 **Issue tickets** — auto-incremented number with prefix, timestamp, waiting count, and expected serve time
- 📋 **Print queue RTL** — `A01 <-- A02 <-- A03`
- 📋 **Print queue LTR** — `A03 --> A02 --> A01`
- 🧾 **Print all tickets** — full card per ticket (time, waiting, serve time)
- ✅ **Serve next client** — FIFO order
- 👁️ **Who is next** — peek without removing
- 📊 **Queue info** — total, served, waiting
- 🔁 **Multiple independent queues** — each with its own prefix and avg serve time

---

## 🗂️ Project Structure

```
QueueLineProject/
│
├── main.cpp           # Demo — PayBills (A0) and Subscriptions (B0) queues
├── clsQueueLine.h     # Main system — clsQueueLine + nested clsTicket
├── clsDate.h          # Date/time utility (ticket timestamp)
├── clsString.h        # String utility class
└── README.md
```

---

## 🧱 Code Architecture

### clsQueueLine

| Member | Type | Role |
|---|---|---|
| `_Prefix` | `string` | Ticket prefix e.g. `"A0"` → tickets: `A01, A02...` |
| `_TotalTickets` | `short` | Counter of all issued tickets |
| `_AverageServeTime` | `short` | Average minutes to serve one client |
| `QueueLine` | `queue<clsTicket>` | STL queue holding all active tickets |

### Nested class clsTicket (private)

| Field | Description |
|---|---|
| `_Number` | Auto-incremented ticket number |
| `_TicketTime` | System datetime at moment of issue |
| `_WaitingClients` | How many clients were waiting when ticket was issued |
| `ExpectedServeTime()` | `_WaitingClients × _AverageServeTime` minutes |
| `FullNumber()` | Returns e.g. `"A03"` |

### PrintTicketsLTR — clever use of Stack

To print the queue in reverse without modifying the original:

```cpp
// 1. Drain queue into a temp stack (reverses order naturally)
while (!TempQueue.empty()) {
    TempStack.push(TempQueue.front());
    TempQueue.pop();
}
// 2. Pop stack → now prints LTR direction
while (!TempStack.empty()) {
    cout << TempStack.top().FullNumber() << " --> ";
    TempStack.pop();
}
```

Classic **Queue reversal using a Stack** — O(n) time, O(n) space.

---

## 🚀 Getting Started

```bash
g++ main.cpp -o QueueLine
./QueueLine
```

Or in Visual Studio: Add files → `Ctrl + F5`

### Usage Example

```cpp
#include "clsQueueLine.h"

clsQueueLine PayBillsQueue("A0", 10); // prefix "A0", 10 min avg serve time

PayBillsQueue.IssueTicket();  // A01 — 0 waiting → 0 min
PayBillsQueue.IssueTicket();  // A02 — 1 waiting → 10 min
PayBillsQueue.IssueTicket();  // A03 — 2 waiting → 20 min

PayBillsQueue.PrintInfo();
PayBillsQueue.PrintAllTickets();

cout << PayBillsQueue.WhoIsNext(); // "A01"
PayBillsQueue.ServeNextClient();   // A01 served ✅
```

---

## 📊 Big O Analysis

| Operation | Time | Space | Notes |
|---|---|---|---|
| `IssueTicket()` | O(1) | O(1) | `std::queue::push` is O(1) |
| `ServeNextClient()` | O(1) | O(1) | `std::queue::pop` is O(1) |
| `WhoIsNext()` | O(1) | O(1) | `queue::front()` is O(1) |
| `WaitingClients()` | O(1) | O(1) | `queue::size()` is O(1) |
| `PrintTicketsRTL/LTR()` | O(n) | O(n) | temp copy of queue |
| `PrintAllTickets()` | O(n) | O(n) | temp copy of queue |

> All **core operations are O(1)** — scales cleanly regardless of queue size.

---

## 🔮 Possible Improvements

- [ ] Add **VIP / priority tickets** — insert at front of queue
- [ ] Record **actual serve time** when `ServeNextClient()` is called
- [ ] Export ticket history to a **log file**
- [ ] Add **multi-counter** support — distribute clients across service windows
- [ ] Add **cancel ticket** — remove a specific ticket by number

---

## 👨‍💻 Author

> Built with ❤️ as part of a C++ OOP & Algorithms learning journey.

Feel free to fork, star ⭐, or contribute!

---

## 📄 License

This project is licensed under the **MIT License** — free to use and modify.
