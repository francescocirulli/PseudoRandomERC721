# PseudoRandomERC721
This is an abstract contract that you can import into your ERC721 contract in order to pseudo-randomly assign NFTs Ids in a range 1 -> n.
Eg. your Collection has 10000 NFTs. Using this abstract contract you can pseudo-randomly assign the Ids from 1 to 10000. The contract avoids collisions between ids and gas-consuming iterations. The operation is highly gas-optimized (114,222 Average Gas Used on minting 2000 Ids).
You could use it to pseudo-randomly assign NFT metadata stored off-chain. 
# Why is it a pseudo-randomness?
The contract leverages the built-in capabilities of EVM to generate pseudo-randomness using hash function of parameters such as block.timestamp, block.number, block.prevrandao, msg.sender etc... It is a pseudo-randomness because the block variables can be manipulated by miners, so they cannot be used as a source of full entropy because of the miners’ incentive.
# How can you implement it?
1) Import the PseudoRandomERC721 contract in your NFT contract 
2) Initialize the contract through the Constructor: _idsSize param identifies how many NFT ids the Collection must have
3) Write your mint function: in your NFT mint function you should use the generateTokenPseudoRandomness() and _pickRandomUniqueId(uint256 pseudoRandom) functions. Here is an example:

function mint(...) public returns (uint256) {
        .....
        uint256 pseudoRandom = generateTokenPseudoRandomness();
        uint256 id = _pickRandomUniqueId(pseudoRandom);
        _safeMint(msg.sender, id);
        ...
        return Id;
    }
