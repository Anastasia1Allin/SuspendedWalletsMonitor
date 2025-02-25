from web3 import Web3
import json

# Connect to Ethereum blockchain (Infura)
infura_url = "https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID"
web3 = Web3(Web3.HTTPProvider(infura_url))

# List of monitored wallets
monitored_wallets = {
    "0xSuspiciousWallet1": {"flagged": False},
    "0xSuspiciousWallet2": {"flagged": False},
}

# Function to check wallet activity
def check_wallet_activity(wallet_address):
    try:
        balance = web3.eth.get_balance(wallet_address)
        balance_eth = web3.from_wei(balance, "ether")

        print(f"Wallet {wallet_address} Balance: {balance_eth} ETH")

        # Simulated rule: Flag wallets with 0 balance or rapid withdrawals
        if balance_eth == 0 or balance_eth < 0.01:
            monitored_wallets[wallet_address]["flagged"] = True
            print(f"WARNING: Wallet {wallet_address} has been flagged!")

    except Exception as e:
        print(f"Error checking wallet {wallet_address}: {str(e)}")

# Run the monitoring check
def monitor_wallets():
    print("Checking wallets for suspicious activity...")
    for wallet in monitored_wallets.keys():
        check_wallet_activity(wallet)

# Execute monitoring
if __name__ == "__main__":
    monitor_wallets()
