//SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.5.0 <0.9.0;

// Vaibhavi Chincholkar BE IT (UID: 2019140012)
contract Lottery{
    address public manager;
    address payable[] public participants;
    constructor(){
        manager = msg.sender;
    }
    modifier onlyManager(){
        require(msg.sender == manager, "Only manager can Access");
        _;
    }
    receive() external payable{
        require(msg.value ==  1 ether);
        participants.push(payable(msg.sender));
    }
    function getBalance() public onlyManager view returns(uint) {
        return address(this).balance;
    }
    function getRandom() public view returns (uint){
        return uint(keccak256(abi.encodePacked(block.timestamp, block.difficulty, participants.length)));
    }
    function getWinner() public onlyManager returns(address) {
        require(participants.length >= 3);
        uint index = getRandom() % participants.length;
        address payable winner = participants[index];
        winner.transfer(getBalance());
        participants = new address payable[](0);
        return winner;
    }
}