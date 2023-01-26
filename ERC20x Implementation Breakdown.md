#erc20 #superfluid

1. Implement NFT contracts
	- **QUESTION:** Do we want to use virtual override or just override for functions in NFT contracts?
	- CFANFTBase - a contract which holds shared state between CIFNFT and COF NFT
		- Ensure to take special care with regards to storage layout-use storage gap in base contract to ensure future storage changes are non-breaking
		- Important to note that both NFT contracts should be upgradeable and therefore should use initialize and work with UUPS proxy pattern
	- COFNFT - the Outflow NFT contract
		- Minted to sender and holds the `tokenId` to flow data mapping to be shared by CIFNFT
		- Has `_mint`/`_burn` functions which creates/deletes storage in `tokenId` to flow data mapping for nft
		- Has `mint`/`burn` functions which create/delete flow (not working at this stage)
		- Transfer functionality is likely to be limited
			- **QUESTION:** Transfer functionality is questionable-maybe we don't allow it.
		- **QUESTION:** Should we store CIFNFT in storage vs. calling it via SuperToken
	- CIFNFT
		- Minted to receiver and holds no storage of its own
		- Has `mint`/`burn` functions which emit mint/burn `Transfer` events
		- Transfer functionality has implications at protocol level: deletes current flow from `flowSender`->`flowReceiver` (`nftSender`) and creates new flow from `flow`
		- **QUESTION:** Should we store COFNFT in storage vs. calling it via SuperToken
	- SuperToken - add COFNFT/CIFNFT to storage
		- Ensure to take special care with regards to storage layout
			- Ensure to modify tests in validateStorageLayout
2. Implement NFT contracts tests (Foundry!)
	- Document invariants for different functions
	- Write tests to ensure invariants can't be broken
	- Write upgradability tests for NFTs
3. Implement ERC20x TCI Library to be shared by COFNFT/CIFNFT
	- Implement new `trustedTokenForwardBatchCall` in `Superfluid.sol`
		- Only managed `SuperToken's` and NFTs can call this function
			- Managed is a list inside of SuperToken factory
				- Wrapped, Pure and NativeAsset should all be handled
4. TODO