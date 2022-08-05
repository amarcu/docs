---
description: How to host FIAT UI on fleek for free (in less than 5 minutes)
---

# Host a Frontend

1. [Create a free Alchemy account](https://www.alchemy.com/)
2. Create two apps on Alchemy: one for Goerli Testnet and for Mainnet

!['Create App' form on Alchemy](https://user-images.githubusercontent.com/101981457/182910069-ca9e3828-b5fd-4777-b390-a038806ade5f.png)

3\. Fork the [FIAT UI GitHub repo](https://github.com/fiatdao/fiat-ui.git)

4\. [Create a free Fleek account](https://fleek.co/) (Make sure you use your GitHub account to register)

5\. Create a new Fleek site

6\. Connect Fleek to your forked version of the FIAT UI repo (you should see your GitHub account and repo listed)

!['Create a new site' page on Fleek](https://user-images.githubusercontent.com/101981457/182909143-72860a47-a729-4f7b-b698-b82ddf791b76.png)

7\. Select `IPFS` Hosting Services

8\. Select `NextJS` as the framework:&#x20;

![NextJS - 'Basic build settings'](https://user-images.githubusercontent.com/101981457/182909327-73dee41d-f488-4e47-ab85-7a58a36b718e.png)

9\. Update the build script to `yarn && yarn build && yarn run export`

10\. Set the base directory to `./`

Your build settings should look something like this (`Repository` should point to your repo you selected in step 6.)

![Build settings](https://user-images.githubusercontent.com/101981457/182911878-3add0525-4614-42b2-834e-058caf704334.png)

11\. Open the "Advanced Section" and plug in the following environments variables using the secrets for your newly created Alchemy account:

```
NEXT_PUBLIC_REACT_APP_DEFAULT_CHAIN_ID=1
NEXT_PUBLIC_REACT_APP_SUBGRAPH_MAINNET=https://api.thegraph.com/subgraphs/name/fiatdao/fiat-subgraph
NEXT_PUBLIC_REACT_APP_SUBGRAPH_GOERLI=https://api.thegraph.com/subgraphs/name/fiatdao/fiat-subgraph-goerli
NEXT_PUBLIC_REACT_APP_RPC_URL_MAINNET=REPLACE_ME_WITH_ALCHEMY_URL
NEXT_PUBLIC_REACT_APP_RPC_URL_GOERLI=REPLACE_ME_WITH_ALCHEMY_URL
NEXT_PUBLIC_VERCEL_ENV=production
```

!['Advanced Section' on Fleek](https://user-images.githubusercontent.com/101981457/182909706-0278e0a1-eb84-4988-b112-3a55f74469d4.png)

12\. Click "Deploy Site"!

You're all done! You've successfully deployed the FIAT UI to Fleek.
