stellar-py
==========

Python client library for Stellar

[![PyPI version](https://badge.fury.io/py/stellar-py.svg)](http://badge.fury.io/py/stellar-py)

# Generating keys

```python
account, secret = generate_keypair(password=None)
```

# Signing and submitting transactions

At the highest level of abstraction there are functions like these,
```python
send_payment(secret, account, destination, amount)
set_regular_key(secret, account, regular_key)
```

Lower down the operations are separated for easy offline usage,
where you create the transaction json object, sign the transaction,
and then submit it as separate steps.
```python
tc_json = get_payment_tx_json(account, destination, amount)
tx_blob = sign_transaction(secret, tx_json)
submit_transaction(tx_blob)
```

At an even lower level, you can create your own transaction json object,
and have stellar-py take over from there.

```python
import stellar

tx_json = {
	'TransactionType': 	'Payment',
	'Account': 			account,
	'Amount': 			amount,
	'Destination':		dest,
}

stellar.complete_transaction_fields(tx_json)
tx_blob = stellar.sign_transaction(secret, tx_json)
stellar.submit_transaction(tx_blob)
```

# Federation

Federation is used to translate user names into Stellar account IDs,
so you don't have to remember 34-character base58 strings.

```python
account = get_account(name)
```

Stellar-py maintains a cache with user names and federation servers it
has come across before. By default this is kept in memory and is gone
when your application is finished, but it is possible to persist the
cache to disk by registering a federation cache.

```python
stellar.federation.cache = stellar.FileFederationCache(filename)
```

# Installation

Only tested on Python 2.7

In most cases, this is enough to install:

	$ pip install stellar-py

If not, then you might need these too:

	$ apt-get install python-pip
	$ apt-get install python-dev
