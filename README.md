
# SkillAuction — Talent Marketplace Smart Contract**

## **Overview**

This project implements a decentralized **on-chain talent marketplace** using the **Stacks blockchain** and the **Clarity** smart-contract language.
The contract enables registered talent to list timed auctions for their services while allowing users to place bids, pay fees, and settle auctions transparently and trustlessly.

The contract includes **extensive validation, error handling, and state controls** to ensure secure market participation.

---

## **Key Features**

### **✔ Talent Registration**

* Users register once to become a verified talent provider.
* Stores rating, earnings, completed auctions, and registration height.
* Prevents duplicate registrations.

### **✔ Auction Creation**

* Talent creates time-bounded auctions with:

  * Title
  * Description
  * Category
  * Starting price
  * Auction duration
* Validates price boundaries, duration limits, and non-empty metadata.

### **✔ Bidding System**

* Users place bids that must exceed the current bid by at least 5%.
* Self-bidding is prevented.
* Bids include marketplace fees.
* Previous bidders are automatically refunded.
* Fee transfers and refunds use safe, contract-controlled STX transfers.

### **✔ Auction Completion**

* Only the talent who created the auction can finalize it.
* Auctions must have ended and must have at least one valid bidder.
* Transfers winning bid amount to the talent.
* Updates talent statistics and global marketplace statistics.

### **✔ Marketplace Fee System**

* 5% fee rate (configurable).
* Automatically collected on each bid.
* Accumulates total fees collected for transparency.

### **✔ Read-Only Utilities**

* Fetch auction details.
* Fetch talent info.
* Check if an address is registered.
* Retrieve global marketplace statistics.
* Determine if an auction can be completed.

---

## **Contract Architecture**

### **Constants**

Includes fee rate, price limits, auction duration limits, and error codes.

### **Maps**

* `talents`: Stores talent information.
* `auctions`: Stores auction details and state.

### **Data Variables**

* `next-auction-id`
* `total-auctions-completed`
* `total-fees-collected`

### **Private Functions**

* String validation.
* Auction state validation.

### **Public Functions**

* `register-talent`
* `create-auction`
* `place-bid`
* `complete-auction`

### **Read-Only Functions**

* `get-auction`
* `get-talent-info`
* `get-contract-stats`
* `is-registered`
* `can-complete-auction`

---

## **Error Handling**

The contract uses a structured set of error codes for clarity and easier integration:

| Code                                     | Meaning                               |
| ---------------------------------------- | ------------------------------------- |
| ERR-NOT-AUTHORIZED                       | Attempted unauthorized action         |
| ERR-INVALID-STATE                        | Auction or data not in expected state |
| ERR-NOT-FOUND                            | Auction or talent not found           |
| ERR-INVALID-DURATION                     | Auction duration not within limits    |
| ERR-INSUFFICIENT-FUNDS                   | STX balance too low                   |
| ERR-ALREADY-REGISTERED                   | Talent already registered             |
| ERR-INVALID-PRICE                        | Price outside allowed range           |
| ERR-AUCTION-EXPIRED                      | Auction already ended                 |
| ERR-AUCTION-NOT-ENDED                    | Auction not yet ended                 |
| ERR-SELF-BIDDING                         | Talent cannot bid on own auction      |
| ERR-INVALID-BID                          | Bid too low                           |
| ERR-EMPTY-TITLE / DESCRIPTION / CATEGORY | Metadata validation failures          |
| ERR-AUCTION-NOT-ACTIVE                   | Auction is not active                 |

This structured approach ensures the contract fails safely and predictably.

---

## **How It Works — Flow Summary**

### **1. Talent Registers**

```
(register-talent)
```

### **2. Talent Creates an Auction**

```
(create-auction title description category price duration)
```

### **3. Users Place Bids**

```
(place-bid auction-id amount)
```

### **4. Talent Completes Auction**

```
(complete-auction auction-id)
```

### **5. View Marketplace Data**

```
(get-auction id)
(get-talent-info principal)
(get-contract-stats)
```

---

## **Installation & Deployment**

1. Ensure you have the **Stacks CLI** and Clarity tools installed.
2. Add the contract `.clar` file to your Stacks project.
3. Deploy via:

```
clarity-cli launch
```

4. Interact using your local devnet or a live Stacks network.

---

## **Security Considerations**

* Contract prevents re-entrancy by design (Clarity language guarantee).
* All STX transfers use `try!` for safe error-handled execution.
* Bid refunds ensure no locked funds.
* Validations prevent invalid auction states.

---

