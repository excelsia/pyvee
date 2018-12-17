# pyvee
A python wrapper for vee api.

For more detail instructions, please refer:

[PYVEE User Guide Specification (English)](https://github.com/excelsia/pyvee/wiki/PYVEE-User-Guide-Specification-%28English%29)

[PYVEE 使用详细指南(中文)](https://github.com/excelsia/pyvee/wiki/PYVEE-%E4%BD%BF%E7%94%A8%E8%AF%A6%E7%BB%86%E6%8C%87%E5%8D%97%28%E4%B8%AD%E6%96%87%29)

## Install
For now, 
1. clone the repo under you workspace
2. install packages in pyvee/requirement.txt by 
```pip install -r ./pyvee/requirements.txt```
3. Then you can ```import pyvee``` in your workspace

## Usage

### chain object
1. For testnet:
    ```python
    import pyvee as pv
    ts_chain = pv.testnet_chain()
    ```
2. For default chain:
    ```python
    import pyvee as pv
    main_chain = pv.default_chain()
    ```

3. For custom api node:
    ```python
    import pyvee as pv
    custom_wrapper = pv.create_api_wrapper('http://<full node ip>:9922', api_key='')
    ts_chain = pv.testnet_chain(custom_wrapper)
    ```

4. For completely custom chain:
    ```python
    import pyvee as pv
    custom_wrapper = pv.create_api_wrapper('http://<full node ip>:9922', api_key='')
    t_chain = pv.Chain(chain_name='testnet', chain_id='T', address_version=5, api_wrapper=custom_wrapper)
    custom_wrapper2 = pv.create_api_wrapper('http://<full node ip>:9922', api_key='')
    m_chain = pv.Chain(chain_name='mainnet', chain_id='M', address_version=5, api_wrapper=custom_wrapper2)
    custom_wrapper3 = pv.create_api_wrapper('http://<full node ip>:9922', api_key='')
    c_chain = pv.Chain(chain_name='mychain', chain_id='C', address_version=1, api_wrapper=custom_wrapper3)
    ```

### chain api list
1. look up current block height of the chain:
    ```python
    ts_chain.height()
    ```

2. look up the last block info of the chain:
    ```python
    ts_chain.lastblock()
    ```


3. look up a block info at n in the chain:
    ```python
    ts_chain.block(n)
    ```

4. Get a transaction info by transacion id in the chain:
    ```python
    ts_chain.tx(tx_id)
    ```
    
5. Validate an address of the chain:
    ```python
    ts_chain.validate_address(addr)
    ```

### address object
1. constructed by seed
    ```python
    from pyvee import Account
    my_address = Account(chain=ts_chain, seed='<your seed>', nonce=0)
    ```
2. constructed by private key
    ```python
    from pyvee import Account
    my_address = Account(chain=ts_chain, private_key='<your base58 private key>')
    ```
3. constructed by public key
    ```python
    from pyvee import Account
    recipient = Account(chain=ts_chain, public_key='<base58 public key>')
    ```
4. constructed by wallet address
    ```python
    from pyvee import Account
    recipient = Account(chain=ts_chain, address='<base58 wallet address>')
    ```
 
### address api list
1. Get balance
    ```python
    # get VEE balance
    balance = my_address.balance()
    print("The balance is {}".format(balance))
    # get VEE balance after 5 confirmations 
    balance = my_address.balance(confirmations = 5)
    print("The balance is {}".format(balance))
    ```
2. Send payment transaction
    ```python
    # send payment (100000000 = 1 VEE)
    my_address.send_payment(recipient, amount=100000000)
    ```
3. Send and cancel lease transaction
    ```python
    # send lease (100000000 = 1 VEE)
    response = my_address.lease(recipient, amount=100000000)
    tx_id = response["id"]
    # cancel lease
    my_address.lease_cancel(tx_id)
    ```
