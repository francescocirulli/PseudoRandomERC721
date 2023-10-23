pragma solidity ^0.8.9;
// SPDX-License-Identifier:  CC0-1.0
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

 contract ERC721PseudoRandom is ERC721 {

    uint16[] public ids;
    uint16 private index;
    uint256 private n;

    constructor(string memory name_, string memory symbol_, uint16 _idsSize)
        ERC721(name_, symbol_)
    {
        require(_idsSize > 0, "Size must be greater than 0");
        ids = new uint16[](_idsSize);
    }

    /**
     * @dev Generates a unique token ID.
     * @return pseudoRandom The generated token id.
     */
    function generateTokenPseudoRandomness() internal returns (uint256 pseudoRandom) {
        n++;
        bytes32 hashPseudoRandom = keccak256(abi.encode(n, block.timestamp, block.number, tx.origin, tx.gasprice, block.coinbase, block.prevrandao, msg.sender, blockhash(block.number - 5)));
        return uint256(hashPseudoRandom);    
    }

    /**
     * @dev Picks a random unique ID for a token.
     * @param pseudoRandom The random number to use for ID generation.
     * @return id The unique token id.
     */
    function _pickRandomUniqueId(uint256 pseudoRandom) private returns (uint256 id) {
        unchecked { ++index; }
        uint256 len = ids.length - index;
        require(len != 0, 'no ids left');
        uint256 randomIndex = pseudoRandom % len;
        id = ids[randomIndex] != 0 ? ids[randomIndex] : randomIndex;
        ids[randomIndex] = uint16(ids[len - 1] == 0 ? len - 1 : ids[len - 1]);
        ids[len - 1] = 0;
    }
}
