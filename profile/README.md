# Farcoin

Farcoin is an attempt to bring social tokens to Farcaster and fix the creative monetization gap that exists on the web. Past attempts at social tokens (Bitclout, FriendTech, etc) have all floundered. In particular, bonding curve models died once curves became too steep for new people to participate and for existing people to realize any upside. Farcoin takes a different approach: whenever someone likes your post, you get to mint 1 of their Farcoins. A social-first issuance model can ultimately unlock more social primitives. For instance, someone could create "a private group for people holding at least 5 of my Farcoins." Additionally, users are able to curate who initially receives their Farcoins, rather than leaving it entirely to the market. Long term, we hope to see a market to arise for Farcoins and for people to be able to use Farcoins as a source of income.

## Farcoin Contract

The core of Farcoin is the `FarcoinMinter` contract. It deploys a new Farcoin ERC-20 for each FID. Whenever a token is deployed, it is assigned a name `Farcoin {FID}` and symbol `FRC-{FID}`, where FID corresponds to the user who gave the Like. All mints of existing Farcoins are routed through the `FarcoinMinter` contract.

`FarcoinMinter` also includes its own ERC-20 token named `Farcoin` with symbol `FRC`. While the supply of user' Farcoins grows unlimited, the supply of this particular token is fixed at 10B. It is intended to be a governance token for Farcoin. More on that at the bottom.

The `FarcoinMinter` contract can be found here: https://basescan.org/address/0xEcB5DF8f302706bC0a8F383904b67663b886a9e1

Each user's Farcoin can be found by using the contract method `getFarcoin` and passing in the associated user's FID. If a coin hasn't been created yet, an exception will be thrown.

## Progressively Decentralizing the Like Oracle

The Farcoin contract allows minting Likes as long as they are authorized by an onchain threshold of signers. Currently, that threshold is 1 and the only signer is a key owned by the Farcoin  team. The Farcoin contract natively supports raising this threshold from 1/1 to M/N. Raising it to include non-Farcoin team members (each running independent verification servers) is a near-term goal. This will ensure no single party can behave maliciously and mint Likes based on invalid Farcaster data.

That said, whenever Likes are minted, the `FarcoinMinter` emits a log event making it easy to Audit the issuance. The log event includes:

1) the FID of the liker, 
2) the FID of the liked, 
3) the time range the likes were received,
4) the number of likes received.

Anyone running a Farcaster Hub will be able to verify mints against Hub data. See the `server` repo in this org to understand how Farcoin currently does this.

## Treasury & Protocol Fees

Whenever a user calls `mint` to mint Likes received, 1 of their Farcoins is also minted and added to the Farcoin treasury. 

The Farcoin treasury is a Gnosis Safe living at `base:0x933701AC5eE6e1e8c1214A51ea3Cb2BBcA5749c1`. It is currently administered by a 1/1 key managed by the Farcoin team.

The vision for Farcoin is to ultimately be owned and governed by decentralized community of stakeholders. The `FarcoinMinter` includes an `FRC` governance ERC-20 token. The intent is to eventually transfer ownership of the Treasury to these tokenholders. Currently, governance tokens have been granted to the following contributors:

* 0.1% to alexgrover.eth
* 0.1% to perceptive.eth
* 0.1% to meemmo.eth
* 0.1% to artlu.eth
* 0.1% to @jessepollak (`0x849151d7D0bF1F34b70d5caD5149D28CC2308bf1`)
* 0.1% to @camille (`0xa674F2C33f504345F50cA6C850F9fd8338612166`)

Note: these grants are not endorsements from the recipients of Farcoin. They have been awarded for helping hatch the idea that is Farcoin. In fact, they may not know they've received these tokens until they see this README :) 

The remainder of the 10B Farcoins currently sit in the Treasury. 
