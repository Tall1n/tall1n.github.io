# Getting Started With Hathor Development and Python

Hathor is a new blockchain that combines the trust and reliabilty of bitcoins proof-of-work concept and combines it with a DAG layer to create
a blockchain with instant feeless transactions that is easy to use. The users should focus on their domain knowledge and not on the blockchain knowledge.

Hathor provides a public api to directly interact with the blockchain. https://docs.hathor.network/

So you can just use 

```python
import requests

token_uid = "00000000dbe81c288c32ae6f9e586a29e1069a6083e1bb481546e7d584fcd223"

base_url = "https://node.explorer.hathor.network/v1a"

tokensResponse = requests.get(
    f"{base_url}/thin_wallet/token_history",
    params={"id": token_uid, "count": 1},
)
res_json = tokensResponse.json()

# ->
# res_json.keys()
# dict_keys(['success', 'transactions', 'has_more'])

```
to get all transactions of the NFT with the UID `00000000dbe81c288c32ae6f9e586a29e1069a6083e1bb481546e7d584fcd223`.

To interact with your own wallet, the Hathor team provides a headless-wallet that you can clone from github https://github.com/HathorNetwork/hathor-wallet-headless, with a ReDoc API Documentation https://wallet-headless.docs.hathor.network/

To get it up and running locally clone the github repository and getup a config.js file as described in the hathor-wallet-headless github repository.

In the `seeds` section you can add your wallet seeds you want to use. You can generate a new seed phrase and transfer 0.01 HTR to the address to play around with. Yay feeless transactions^^.
For the api_key you can just set 123 or something until you really want to use it in production. Just dont forget it =).

```
...
http_api_key: '123',
...
  seeds: {
      default: 'my seed phrase goes here ...',
  },
...
```

Then you can start the wallet with `npm start` and access it from python. 


I am using a class for it but you can also paste the URL together yourself.
But if you have the same header you have to pass and the same base URL, its more convenient to store it in a class

```python

class HeadlessWallet(object):
    """Hathor Headless Wallet Connection.
    Github Docs for basic functionality: https://github.com/HathorNetwork/hathor-wallet-headless
    Full, up-to-date swagger docs: https://wallet-headless.docs.hathor.network/

    Run `npm start` in path/to/cloned/git/repo/hathor-wallet-headlesst.
    Follow the Github Docs to get the wallet started with a seed phrase.
    """

    def __init__(
        self,
        base_url="http://localhost:8000",
        wallet_id="123",
        api_key="123",
    ):
        self.wallet_id = wallet_id
        self.api_key = api_key
        self.base_url = base_url
        self.headers = {"X-Wallet-Id": self.wallet_id, "X-API-KEY": self.api_key}

    def start(self, wallet_id=None):
        if wallet_id is None:
            wallet_id = self.wallet_id
        start_data = {"wallet-id": wallet_id, "seedKey": "default"}
        logging.info(f"Starting wallet with {wallet_id=}")
        start = requests.post(
            f"{self.base_url}/start",
            headers=self.headers,
            data=start_data,
        )
        if not start.json() == {"success": True}:
            raise ConnectionError("Could not start headless wallet.")

    def status(self):
        status = requests.get(f"{self.base_url}/wallet/status", headers=self.headers)
        return status.json()


headless_wallet = HeadlessWallet()
headless_wallet.start()
headless_wallet.status()

```

The starting of the wallet takes a few seconds, so `headless_wallet.status()` will return a "wallet not ready" if you execute it directly.

You only have to run `start` once to get the wallet running. You can check the console in which you executed `npm start` to see your requests
reaching the headless wallet.


### Other Resources:

* Hathor provides an explorer for you to get details about a token: https://explorer.hathor.network/.
* The community project HAthor Bullz Club proves a general purpose NFT viewer: https://explorer.hathorbullzclub.io/


------

## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/Tall1n/tall1n.github.io/edit/main/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Tall1n/tall1n.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
