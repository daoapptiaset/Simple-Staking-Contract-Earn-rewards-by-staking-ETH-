# Simple-Staking-Contract-Earn-rewards-by-staking-ETH-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleStaking {
    mapping(address => uint256) public stakedAmount;
    mapping(address => uint256) public stakingTime;
    uint256 public rewardRate = 10; // 10% per year (simplified)

    function stake() public payable {
        require(msg.value > 0, "Cannot stake 0");
        stakedAmount[msg.sender] += msg.value;
        stakingTime[msg.sender] = block.timestamp;
    }

    function calculateReward(address user) public view returns (uint256) {
        uint256 timeStaked = block.timestamp - stakingTime[user];
        return (stakedAmount[user] * rewardRate * timeStaked) / (365 days * 100);
    }

    function unstake() public {
        uint256 reward = calculateReward(msg.sender);
        uint256 total = stakedAmount[msg.sender] + reward;
        stakedAmount[msg.sender] = 0;
        payable(msg.sender).transfer(total);
    }
}
