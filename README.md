import hashlib
import time
import json

class Block:
    def __init__(self, index, transactions, timestamp, previous_hash):
        self.index = index
        self.transactions = transactions
        self.timestamp = timestamp
        self.previous_hash = previous_hash
        self.hash = self.calculate_hash()

    def calculate_hash(self):
        block_string = json.dumps({
            "index": self.index,
            "transactions": self.transactions,
            "timestamp": self.timestamp,
            "previous_hash": self.previous_hash
        }, sort_keys=True).encode()
        return hashlib.sha256(block_string).hexdigest()

class Blockchain:
    def __init__(self):
        self.chain = []
        self.create_genesis_block()

    def create_genesis_block(self):
        genesis_block = Block(0, [], time.time(), "0")
        self.chain.append(genesis_block)

    def get_latest_block(self):
        return self.chain[-1]

    def add_block(self, transactions):
        previous_block = self.get_latest_block()
        new_block = Block(previous_block.index + 1, transactions, time.time(), previous_block.hash)
        self.chain.append(new_block)

# टेस्टिंग
blockchain = Blockchain()
blockchain.add_block(["Alice sent 10 coins to Bob"])
blockchain.add_block(["Bob sent 5 coins to Charlie"])

for block in blockchain.chain:
    print(f"Block #{block.index}")
    print(f"Transactions: {block.transactions}")
    print(f"Timestamp: {block.timestamp}")
    print(f"Previous Hash: {block.previous_hash}")
    print(f"Hash: {block.hash}\n")
