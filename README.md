# Multi-Token-Reward-Farm
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.0/contracts/token/ERC20/IERC20.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.0/contracts/access/Ownable.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.0/contracts/security/ReentrancyGuard.sol";

contract MultiRewardFarm is Ownable, ReentrancyGuard {
    IERC20 public stakingToken;
    IERC20 public rewardToken;

    mapping(address => uint256) public staked;
    uint256 public totalStaked;

    event Staked(address user, uint256 amount);
    event RewardClaimed(address user, uint256 amount);

    constructor(address _staking, address _reward) Ownable(msg.sender) {
        stakingToken = IERC20(_staking);
        rewardToken = IERC20(_reward);
    }

    function stake(uint256 amount) external nonReentrant {
        stakingToken.transferFrom(msg.sender, address(this), amount);
        staked[msg.sender] += amount;
        totalStaked += amount;
        emit Staked(msg.sender, amount);
    }

    function claimReward(uint256 amount) external nonReentrant {
        // Simplified - in production calculate based on time/stake
        rewardToken.transfer(msg.sender, amount);
        emit RewardClaimed(msg.sender, amount);
    }
}
