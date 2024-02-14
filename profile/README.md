# Farcoin

Farcoin is an attempt to bring social tokens to Farcaster and fix the creative monetization gap that exists on the web. In the past, social tokens that led with financialization (Bitclout, FriendTech, etc) all died once there was no more upside. Farcoin takes a different approach: whenever someone likes your post, you get to mint 1 of their Farcoins. The immediate goal is to unlock more social primitives using Likes as the gates of entry. For instance, someone could create "a private group for people holding at least 5 of my Farcoins." Since issuance is as easy as Liking, users are able to curate who receives their Farcoins initially. Long term, the goal is for a market to arise for Farcoins, and for people to be able to use Farcoins as a source of income.

## Token Contract

Farcoins are Semi-Fungible ERC-1155s. Each user has their own token id - their FID - and a supply reflecting the number of their Likes that have been minted by others.

All Farcoins are housed in a single contract, found here: https://basescan.org/address/0x69ac278f393f4daa698e685909b3aedbf95c1c49

## Oracle

The Farcoin contract allows minting of Likes as long as they are authorized by an onchain threshold of signers. Currently, that threshold is 1 and the only signer is a key owned by the Farcoin  team. The Farcoin contract natively supports raising this threshold and raising the threshold to include non-Farcoin team members is a near-term goal. This will ensure no single party can behave maliciously and mint fake Likes.

That said, whenever Likes are minted, the contract emits a log event making it easy to Audit the issuance. The log event includes:

1) the FID of the liker, 
2) the FID of the liked, 
3) the time range the likes were received,
4) the number of likes received.

Anyone running a Farcaster Hub will be able to verify mints against Hub data. See the `server` repo in this org to understand how Farcoin currently does this.

## Treasury & Protocol Fees

Whenever a user calls `mint` to mint Likes received, 1 of their Farcoins is also minted. This gets added to the Farcoin treasury. 

The Farcoin treasury is a Gnosis Safe living at `base:0x933701AC5eE6e1e8c1214A51ea3Cb2BBcA5749c1`. It is currently administered by a 1/1 key managed by the Farcoin team.

The vision for Farcoin is to ultimately be administered by a DAO. The team intends to transfer ownership of the Safe to this DAO. It is not yet determined how DAO membership will be granted.
