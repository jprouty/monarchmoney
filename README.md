# Monarch Money

Python library for accessing [Monarch Money](https://www.monarchmoney.com/referral/ngam2i643l) data.

# Installation

## From Source Code

Clone this repository from Git

`git clone https://github.com/hammem/monarchmoney.git`

## Via `pip`

`pip install monarchmoney`
# Instantiate & Login

There are two ways to use this library: interactive and non-interactive.

## Interactive

If you're using this library in something like iPython or Jupyter, you can run an interactive-login which supports multi-factor authentication:

```python
mm = MonarchMoney()
await mm.interactive_login()
```
This will prompt you for the email, password and, if needed, the multi-factor token.

## Non-interactive

For a non-interactive session, you'll need to create an instance and login:

```python
mm = MonarchMoney()
mm.login(email, password)
```

This may throw a `RequireMFAException`.  If it does, you'll need to get a multi-factor token and call the following method:

```python
mm.multi_factor_authenticate(email, password, multi_factor_code)
```

Alternatively, you can provide the MFA Secret Key. The MFA Secret Key is found when setting up the MFA in Monarch Money by going to Settings -> Security -> Enable MFA -> and copy the "Two-factor text code". Then provide it in the login() method:
```python
await mm.login(
        email=email,
        password=password,
        save_session=False,
        use_saved_session=False,
        mfa_secret_key=mfa_secret_key,
    )

```

# Accessing Data

As of writing this README, the following methods are supported:

## Non-Mutating Methods

- `get_accounts` - gets all the accounts linked to Monarch Money
- `get_account_holdings` - gets all of the securities in a brokerage or similar type of account
- `get_budgets` — all the budgets and the corresponding actual amounts
- `get_subscription_details` - gets the Monarch Money account's status (e.g. paid or trial)
- `get_transactions` - gets transaction data, defaults to returning the last 100 transactions; can also be searched by date range
- `get_transaction_categories` - gets all of the categories configured in the account
- `get_transaction_details` - gets detailed transaction data for a single transaction
- `get_transaction_splits` - gets transaction splits for a single transaction
- `get_transaction_tags` - gets all of the tags configured in the account
- `get_cashflow` - gets cashflow data (by category, category group, merchant and a summary)
- `get_cashflow_summary` - gets cashflow summary (income, expense, savings, savings rate)
- `is_accounts_refresh_complete` - gets the status of a running account refresh

## Mutating Methods

- `request_accounts_refresh` - requests a syncronization / refresh of all accounts linked to Monarch Money. This is a **non-blocking call**. If the user wants to check on the status afterwards, they must call `is_accounts_refresh_complete`.
- `request_accounts_refresh_and_waid` - requests a syncronization / refresh of all accounts linked to Monarch Money. This is a **blocking call** and will not return until the refresh is complete or no longer running.
- `create_transaction` - creates a transaction with the given attributes
- `update_transaction` - modifes one or more attributes for an existing transaction
- `update_transaction_splits` - modifes how a transaction is split (or not)
- `set_budget_amount` - sets a budget's value to the given amount (date allowed, will only apply to month specified by default). A zero amount value will "unset" or "clear" the budget for the given category.

# Contributing

Any and all contributions -- code, documentation, feature requests, feedback -- are welcome!

If you plan to submit up a pull request, you can expect a timely review.  There aren't any strict requirements around the environment you need to configure aside from using [Black](https://github.com/psf/black) to auto-format the code.  An action is configured in this repo to run against all PRs and merges and will block them from being committed.

# FAQ

**How do I use this API if I login to Monarch via Google?**

If you currently use Google or 'Continue with Google' to access your Monarch account, you'll need to set a password to leverage this API.  You can set a password on your Monarch account by going to your [security settings](https://app.monarchmoney.com/settings/security).  

Don't forget to use a password unique to your Monarch account and to enable multi-factor authentication!
