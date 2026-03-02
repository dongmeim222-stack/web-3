# First Week Summary
<h1>First Week Summary</h1>
<h2>Understanding of Core Ethereum Whitepaper Concepts (Concepts from Ethereum Whitepaper + Doubao Interpretation)</h2>
<h4>Blockchain: A public ledger in every villager’s hand</h3>
<p>

**Example**: In the past, transfers in the village were recorded by the village chief (centralized), who could embezzle or make mistakes. After Bitcoin was invented, every villager had an identical copy of the ledger. Whenever someone transferred money, they announced it to the whole village, and everyone recorded the transaction in their own ledger at the same time. No one could tamper with it (changing your own copy was useless, as everyone else’s was different).

**Key Point**: Everyone records one page (a block) every 10 minutes. Each page has the number (hash) of the previous page, linked together to form a “blockchain” — like numbered pages of a ledger. Changing any page would invalidate all later pages.</p>

<h4>Proof of Work (PoW): A “fair competition” for block-writing rights</h4>
<p>

**Example**: Who writes the current page? The whole village competes to solve an extremely hard math problem (e.g., “find a number whose hash is less than 2¹⁸⁷”). The first to solve it writes the block and gets a 25 BTC reward (mining reward) + transaction fees.

**Key Point**: This problem can only be solved by “brute‑force guessing”. The more computing power you have, the higher your chance to win. It ensures “anyone can participate” and prevents cheating (cheating requires more power than the whole village combined — a 51% attack).
</p>

<h4>UTXO: “Change tickets” in the ledger</h4>
<p>

**Example**: Money in Bitcoin is not an “account balance”, but a set of “indivisible change tickets”. For example, if you have 12 BTC in tickets (6+4+2) and want to send 11.7 BTC to someone, you “spend” those 3 tickets (marked as used in the ledger) and create two new tickets: 11.7 BTC for the receiver and 0.3 BTC as change for yourself.

**Key Point**: Every transaction must reference previous “unspent tickets” (UTXOs) to ensure you are spending your own money and avoid double‑spending.  </p>
<h4>Merkle Tree: An “efficient packing list” for the ledger</h4>
<p>

**Example**: A block may have 1000 transactions. Recording all 1000 directly wastes space. A Merkle Tree works like “layered package sorting”: first pair 1000 transactions into 500 pairs and compute a hash for each; then pair those 500 hashes into 250, and so on… until only one “root hash” remains, stored in the block header.

**Key Point**: To verify a transaction, you don’t need the full 1000 transactions — just its “proof path” (a few hashes). Phones (light nodes) don’t need the full ledger, making verification much faster (this is SPV: Simplified Payment Verification).
</p>

<h4>Limitations of Bitcoin: Only “keeps accounts”, cannot “act automatically”</h4>
<p>

**Example**: Bitcoin’s ledger rules (script) have 4 critical flaws, like a “calculator that only adds and subtracts”:
- Non-Turing-complete: no loops, no complex logic (e.g., “auto transfer after 30 days”);
- Value-blind: can only spend full tickets, cannot split proportionally (e.g., “send 10% to A, 90% to B”);
- Stateless: only records whether tickets are spent, cannot store extra data (e.g., “membership level”, “contract progress”);
- Blockchain-blind: cannot see timestamps, block hashes, etc. (cannot build games like “guess next block hash”).

<h4>Core Ethereum Upgrade: Installing a “smart operating system” on the ledger</h4>

Ethereum keeps the core of the “public ledger” (blockchain) but makes 3 key upgrades, explained using the “village smart ATM” analogy:

<p>

**1. Account Model**: From “change tickets” to “bank accounts”

**Example**: Bitcoin uses “tickets”, while Ethereum directly uses “a bank account for everyone”. Each account has 4 values: transaction counter (prevents repeated transactions), ETH balance, contract code (if it’s a contract account), and storage (extra data like membership points).
**Two types of accounts**:
**Externally Owned Account (EOA)**: Your personal bank card (controlled by a private key, e.g., wallet app on your phone). No code, only sends transactions actively.
**Contract Account**: The village “smart ATM” (controlled by code). No private key. Automatically runs code when it receives a transaction or message.

</p>
<p>

**2. EVM (Ethereum Virtual Machine)**: The “unified operating system” for all smart ATMs
**Example**: For the village’s smart ATMs (contract accounts) to run different programs (transfer, insurance payout, domain registration), they need a unified OS — that’s the EVM. It acts like a “virtual machine”: any device (phone, computer) with EVM can run Ethereum smart contracts, ensuring all nodes (ledger keepers) get the same result.
**Key Point**: EVM is Turing-complete — supports loops and conditional logic (e.g., “send money to farmers if rainfall < 50mm”). It’s like adding a “programming language” to the ledger for any complex automatic rule.</p>
**3. Smart Contract**: “Automatic rules” inside the smart ATM
<p>

**Example**: You write a rule (contract code) for the smart ATM: “If A and B both deposit 1000 ETH, then after 30 days, send back ETH worth 1000 USD to A at the NASDAQ ETH/USD price, and the rest to B” — this is a “hedge contract” that automatically protects both from price volatility.
**Essence**: A smart contract is an “encrypted automatic box” holding money and rules. It runs automatically when conditions are met, with no human interference. Examples:
**Token system**: Two lines of code to make “village exclusive points”, with rules like “subtract X points from A, add X points to B”;
**Savings wallet**: “Alice can withdraw at most 1% per day; Bob can help her withdraw, but Alice can cancel his permission anytime”;
**Crop insurance**: “Auto send money to insured farmers if Iowa rainfall < X”.

<p>

**4. GAS (Fuel)**: “Electricity fee + service fee” for the smart ATM
**Example**: Using the smart ATM (executing contracts) consumes computing power, storage, bandwidth — these resources cost money. GAS is the “electricity fee”. GASPRICE is the “price per unit”, STARTGAS is the “maximum electricity you are willing to pay” (max computation steps).
For example, transferring + registering a domain uses more steps and more GAS.
If the code is broken (e.g., infinite loop), the “electricity” runs out (out of gas), the operation reverts, but the “fee” is not refunded — prevents malicious resource abuse.
**5. Message**: “Communication instructions” between smart ATMs
<p>

**Example**: Smart ATM A (insurance contract) wants to know the NASDAQ ETH price, so it sends a “message” (instruction) to smart ATM B (data feed contract): “Please tell me the current ETH/USD price”. After receiving the message, B runs code to get the price and sends it back to A.
**Key Point**: Difference between messages and transactions — transactions are “instructions from personal bank cards” (signed), messages are “instructions between smart ATMs” (unsigned, triggered by code). Both cost GAS.
<p>

**6. Uncle Block (GHOST Protocol)**: Reward for “late ledger pages”
**Example**: In the village competition, A solves it first but his block spreads slowly. B doesn’t receive A’s block and also solves it — under Bitcoin rules, B’s block is discarded (stale block) and B gets nothing. The base block reward on the main chain = 5 ETH. To avoid wasting computing power, Ethereum gives B’s block (uncle block) 7/8 of the reward, and gives A (nephew block), who includes B’s block, an extra 3.125% reward.**This concept deserves detailed understanding**
<p>

**Key Point**: A maximum of 7 generations of uncle blocks are accepted **(invalid beyond 7 generations)**. Encourages participation and prevents intentional forking.
**7. Ether (ETH)**: “Universal currency + electricity fee” of the ledger
<p>

**Example**: ETH is the “official currency” of the Ethereum ledger, with two roles:
Medium of exchange: send to others, buy things;
Pay GAS: must use ETH as “electricity fee” to use the smart ATM (run contracts).
**Units**: Like yuan, jiao, fen — 1 ETH = 10³ finney = 10⁶ szabo = 10¹⁸ wei (ETH daily, finney small amounts, szabo/wei technical).
**Issuance**: Initially sold via crowdfunding (1 BTC for 1000–2000 ETH), then miners get 0.26× the total crowdfunded ETH per year. Long‑term growth tends to zero (controls inflation).

**8. DAO (Decentralized Autonomous Organization)**: A “community” run by smart ATMs
**Example**: The village wants a “fruit cooperative”. Everyone contributes money (ETH) into a smart ATM (DAO contract) with written rules:
Spending cooperative money needs approval from 2/3 of members via vote;
New members need approval from 2/3 existing members via vote;
Dividends are automatically distributed proportionally to investment.
**Essence**: A DAO is a “company without a boss”. All decisions are made by code and member votes. No one can embezzle funds or change rules — fully decentralized.

**9. Patricia Tree**: Upgraded “Merkle Tree”

**Example**: Ethereum’s ledger needs frequent updates to balances and stored data (like contract progress). Merkle Tree is good for “static packaging”, Patricia Tree for “dynamic updates” — like a “dictionary with indexes”, supports fast insert, delete, modify without recalculating the whole tree hash, saving storage and computation.
**Key Point**: Every Ethereum block stores a “final state root” (Patricia Tree root hash). Nodes don’t need the full history — only the latest state to verify transactions.
<h4>III. Connecting All Concepts: Full Ethereum Workflow</h4>
Now connect all concepts in one line with the story of “farmer buying crop insurance”:

**Scenario**: Farmer A in Iowa wants to hedge drought risk and finds speculator B on Ethereum to join a “crop insurance contract”.
**Account & Transaction**: A and B use their own external accounts (personal bank cards) to send 1000 ETH each to the “insurance contract account” (smart ATM). Transaction includes: recipient address, signature, amount, data field, STARTGAS, GASPRICE.
**GAS Cost**: A and B’s transactions cost GAS — transfer is basic, each byte of data (e.g., “insured area: Iowa”) is 5 GAS. STARTGAS = 2000 (max 2000 steps), GASPRICE = 0.001 ETH per unit.
**Smart Contract Execution**: After receiving the transaction, the insurance contract (smart ATM) starts the EVM (OS) and runs the code (automatic rules):
<p>

- Check if A and B have enough balance and the transactions are valid;
- Deduct 1000 ETH from A and B, deposit into the contract account;
- Call the “weather data feed contract” (another smart ATM) and send a message: “Record today’s Iowa rainfall and ETH/USD price”;
- The weather contract runs code to get data, returns it to the insurance contract, which stores the data in its storage (Patricia Tree).
</p>

**Block Packaging & Mining**: Miners (villagers) package A and B’s transactions and execution into a new block (includes transaction list, latest state root, block number, difficulty);
<p>

- Miners do Proof of Work (find hash below target) and may include an uncle block (valid late block) for extra reward;
- After success, the whole village (nodes) sync the block and update ledgers (A and B balances down, contract balance up, weather data stored).
</p>

**Automatic Payout**: After 30 days, if Iowa rainfall < 50mm, the contract code triggers automatically:
<p>

- Call the weather contract again for latest rainfall and ETH price;
- According to rules, send ETH worth 1000 USD back to A, remaining to B (speculator profits);
- No need for A, B or third parties — contract runs automatically, all nodes sync the result.
</p>

**Scalability**: If the ledger becomes huge (e.g., 100 TB), ordinary villagers (light nodes) don’t need the full ledger. They use SPV to download block headers and Merkle branches to verify transactions. If a malicious miner makes an invalid block, honest nodes provide “invalidity proof” to the whole village.

<h4>Core Summary</h4>

The essence of Ethereum is: **a decentralized public ledger with a built-in Turing-complete programming language (EVM)**. It fixes Bitcoin’s limitation of “only bookkeeping, no automatic action”. Through “account model + smart contract + GAS mechanism”, the ledger can run all kinds of decentralized applications — from financial derivatives, insurance, domain registration, file storage, DAOs, to cloud computing and prediction markets.

<h3>Extensions</h3>

**Private Key & Digital Signature:**

Private key = your top-secret key, ultimate password (never show anyone)
Digital signature = a “one-time signature” on this transaction made with your private key
The private key is the tool to create signatures; the signature is just the result.

**nonce:**
<p>

- **Definition of nonce**
nonce is a “one-time counter” for Ethereum accounts, two types:
Externally Owned Account (EOA): number of successful transactions sent (starts at 0);
Contract Account: number of contracts created (used to calculate new contract addresses).
- **Why nonce must increase**
Prevent replay attack: transactions with duplicate nonce are rejected, stopping attackers from rebroadcasting valid transactions;
Ensure transaction order: same account’s transactions processed in nonce order, avoiding state errors;
Guarantee unique contract addresses: nonce is used to generate contract addresses, increasing nonce avoids conflicts.

</p>

**Why is EVM "Turing-complete"?**

Turing-complete means: can simulate any computation a Turing machine can do, theoretically run any computable logic. EVM qualifies because:
Supports loops and conditional jumps: uses JUMP (unconditional) and JUMPI (conditional) opcodes;
Supports recursion and inter-contract calls: uses CALL opcode for nested/recursive calls;
Complete compute & storage system: stack (temp compute) + memory (temp storage) + persistent storage (contract state) for full logic;
Only limits resources, not logic: GAS controls computation (prevents infinite loops), but not complexity — any logic works with enough GAS.

**Why do transactions need signatures?**
<p>

Transaction signatures use ECDSA asymmetric cryptography, core of Ethereum security. Reasons:
- Identity verification: no central authority, signature proves the transaction is from the real owner;
- Anti-tampering: signature is bound to full transaction data; changing anything breaks verification;
- Non-repudiation: only you have the private key, so you can’t deny sending the transaction.
</p>