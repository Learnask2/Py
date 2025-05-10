_from web3 import Web3_
_from eth_account import Account_

_infura_url = "YOUR_INFURA_GOERLI_URL"_
_private_key = "YOUR_PRIVATE_KEY"_
_account = Account.from_key(private_key)_
_address = account.address_

_w3 = Web3(Web3.HTTPProvider(infura_url))_
_uniswap_router_address = "0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D"_
_uniswap_router_abi = [...] # Ambil dari Etherscan_

_uniswap_router = w3.eth.contract(address=uniswap_router_address, abi=uniswap_router_abi)_

_amount_in_eth = w3.to_wei(0.01, 'ether')_
_path = [_
    _w3.to_checksum_address("0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeeeE"), # ETH_
    _w3.to_checksum_address("0xdc31Ee1784292379Fbb2964b3B9C249F0b8527ba") # DAI_
_]_
_recipient_address = address_
_deadline = w3.eth.get_block('latest')['timestamp'] + 60 * 5_
_slippage = 0.01_

_amounts_out = uniswap_router.functions.getAmountsOut(amount_in_eth, path).call()_
_amount_out_min = int(amounts_out[1] * (1 - slippage))_

_nonce = w3.eth.get_transaction_count(address)_
_gas_price = w3.eth.gas_price_
_transaction = uniswap_router.functions.swapExactETHForTokens(_
    _amount_out_min, path, recipient_address, deadline_
_).build_transaction({_
    _'from': address, 'value': amount_in_eth, 'gasPrice': gas_price, 'nonce': nonce_
_})_

_gas_estimate = w3.eth.estimate_gas(transaction)_
_transaction['gas'] = gas_estimate * 2_

_signed_txn = account.sign_transaction(transaction)_
_tx_hash = w3.eth.send_raw_transaction(signed_txn.rawTransaction)_
_print(f"Transaction Hash: {tx_hash.hex()}")_
