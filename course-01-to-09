// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.8.1;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
// import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/security/Pausable.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
// import "@openzeppelin/contracts/token/common/ERC2981.sol";

contract NFT is ERC721Enumerable, ReentrancyGuard, Ownable {
    // using liberaries
    using Strings for uint256;
    using Counters for Counters.Counter; // no need, but added for education

    Counters.Counter private _tokenIdCounter; // no need, but added for education

    // feed
    uint8 public limit; // max 255 -- how much for minting
    uint256 private _tokenId = 1;
    uint256 public cost;
    uint256 public expandCost = 0.01 ether; // if costExpand is true
    bool public costExpand = true;
    bool public pause = false; 
    string baseURI;
    mapping(address => mapping(uint256 => bool)) public collector; // need an array for iterate

    // constructor
    constructor(
        string memory _name,
        string memory _symbol,
        string memory _initBaseURI,
        uint8 _limit
    ) ERC721(_name, _symbol) {
        setBaseURI(_initBaseURI);
        cost = 0.01 ether;
        limit = _limit;
    }

    // modifier

    // event
    event newMint(uint256 indexed id,address indexed collector, uint256 newP);

    // setup
    function _baseURI() internal view virtual override returns (string memory) {
        return baseURI;
    }
    
    // no need and gas looser
    function _checker(address _collector) internal view returns (bool x) {
        uint8 j = limit;
        // for(uint8 i = 0; i <= j; i++){
        //     if(collector[_collector][i] == false){
        //         x = true;
        //         return x;
        //     }
        //     x = false;
        //     return x;
        // }
        uint8 i = 0;
        while (i <= j){
            // return collector[_collector][i] == false ? true : false;
            if(collector[_collector][i] == false){
                x = true;
            }
            else {
                x = false;
            }
            i += 1;
        }
    } 

    // exacute
    function setBaseURI(string memory _uri) public onlyOwner {
        baseURI = _uri;
    }

    function getURI() external view returns (string memory) {
        return _baseURI();
    }

    function setCost(uint256 _newCost) public onlyOwner {
        expandCost = _newCost;
    }

    function setExpandCost(uint256 _newCost) public onlyOwner {
        cost = _newCost;
    }

    function toggleCostExpand() public onlyOwner {
        costExpand = !costExpand;
    }

    function togglePause() public onlyOwner {
        pause = !pause;
    }
    
    function mint() public payable {
        // require
        require(!pause);
        require(limit > totalSupply(), "mint is finish");
        if (msg.sender != owner()) require(msg.value >= cost);
        // require(!_checker(msg.sender)); // cheker make error. gas looser
        _mint(msg.sender, _tokenId);
        collector[msg.sender][_tokenId] = true;
        _tokenId += 1;
        _tokenIdCounter.increment(); // no need, but added for education
        if(costExpand == true) cost += 0.01 ether;
        emit newMint(_tokenId, msg.sender, cost);
    }

    function burn(uint256 _id) public {
        // require
        _burn(_id);
    }


    
    
    

}

/// @notice: opensea recomended -> ownable, ierc165, erc2981

/**
pausable modifiers:
- whenNotPaused
- whenPaused

event Unpaused(address account);
event Paused(address account);

    function paused() public view virtual returns (bool) {
        return _paused;
    }

_pause()
_unpause()
*/

/**
reentrancy modifier:
- nonReentrant
*/

/**
ownable modifier:
- onlyOwner
*/

/**
counter:
x.current()
x.increment()
x.decrement()
x.reset()
*/
