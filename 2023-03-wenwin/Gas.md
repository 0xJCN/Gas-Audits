## Summary

A majority of the optimizations were benchmarked via the protocol's tests. All benchmarked issues will include gas savings as seen from the output of `forge snapshot --diff`.

Issue [G-02](#structs-can-be-packed-to-use-fewer-storage-slots) does not include benchmarks due to required test suite refactoring needed to run `forge snapshot --diff`. These instances are explained via opcodes and EVM gas costs.

**It should be noted that functions which are essential to the core functionality of the protocol, such as `buyTickets()`, `executeDraw()`, and `claimWinningTickets()` have seen gas savings of `~47644`, `~47942`, and `~47856` respectively.**

Overall change from forge snapshot (*not including gas savings from issue [G-02](#structs-can-be-packed-to-use-fewer-storage-slots))*:
```
testFixedReward() (gas: 0 (0.000%))
testRequestRandomNumbers() (gas: 0 (0.000%))
testInitSourceFailsWhenAlreadyInitialized() (gas: 0 (0.000%))
testInitSourceFailsWithZeroAddress() (gas: 0 (0.000%))
testOnRandomNumberFulfilledFailedWhenCalledByNonSource() (gas: 0 (0.000%))
testSwapSourceFailsWithNonOwnerInvocation() (gas: 0 (0.000%))
testSwapSourceFailsWithZeroAddress() (gas: 0 (0.000%))
testDepositAfterDeadline() (gas: 0 (0.000%))
testGetRewardSendsEntireBalanceToOwner() (gas: 0 (0.000%))
testConstructorZeroAddress() (gas: 0 (0.000%))
testStakeWithZeroAmount() (gas: 0 (0.000%))
testWithdrawSendsTokens() (gas: 0 (0.000%))
testWithdrawWithZeroAmount() (gas: 0 (0.000%))
testRandomNumberReconstruct(uint256) (gas: 0 (0.000%))
testReconstructTicket() (gas: 0 (0.000%))
testTicketWinTier(uint256,uint256,uint8,uint8) (gas: 0 (0.000%))
testWrongSetups() (gas: 109 (0.002%))
testBuyInvalidTicket() (gas: -7 (-0.003%))
testGetRewardsMultipleStakersStakePostIncome() (gas: -22 (-0.004%))
testClaimFees() (gas: -13 (-0.004%))
testGetRewardsMultipleStakersSplit() (gas: -35 (-0.004%))
testBuyMultipleTickets() (gas: -15 (-0.004%))
testExit() (gas: -32 (-0.005%))
testTransferFromDoesNotTransferRewards() (gas: -40 (-0.006%))
testTransferDoesNotTransferRewards() (gas: -40 (-0.006%))
testGetRewardsSingleStaker() (gas: -40 (-0.006%))
testWithdrawAfterDeadlineBeforeDurationEnd() (gas: -12 (-0.011%))
testDepositOwnerOnly() (gas: -13 (-0.012%))
testDepositPullsStakedTokens() (gas: -13 (-0.012%))
testWithdrawOwnerOnly() (gas: -25 (-0.019%))
testWithdrawSendsStakedTokens() (gas: -25 (-0.020%))
testWithdrawAfterDeadlineAfterDurationEnd() (gas: -26 (-0.021%))
testWithdrawBeforeDeadline() (gas: -26 (-0.021%))
testBuyTicketDuringInitialPotRaise() (gas: -12824 (-0.149%))
testFinalizeInitialPot(uint256,uint256) (gas: -12824 (-0.150%))
testRetryFailsWhenNoPendingRequest() (gas: 17 (0.160%))
testFulfillRandomNumber() (gas: -225 (-0.375%))
testRawFulfillRandomWords() (gas: -379 (-0.518%))
testZeroClaimableAfterDeadline() (gas: -65164 (-0.835%))
testJackpotReturnToPot() (gas: -65749 (-0.887%))
testMultiClaim() (gas: -47856 (-2.742%))
testNonJackpotWinClaimable() (gas: -47749 (-4.325%))
testExecuteDraw() (gas: -47942 (-6.266%))
testBuyTicket() (gas: -47644 (-7.508%))
invariantSufficientFunds() (gas: 0 (NaN%))
testCalculateExcessPot(uint256,uint256) (gas: 0 (0.000%))
testCalculateFrontendFees(uint256,uint256) (gas: 0 (0.000%))
testCalculateJackpotReward(uint256,uint256,uint256) (gas: 0 (0.000%))
testCalculateMultiplier(uint256,uint256) (gas: 0 (0.000%))
testCalculateNonJackpotReward(uint256,uint256,uint256,uint256) (gas: 0 (0.000%))
testCalculateStakingFees(uint256,uint256) (gas: 0 (0.000%))
testInflation(uint256) (gas: 0 (0.000%))
testUnauthorizedMinting() (gas: 0 (0.000%))
testBuyClaimFinalize(uint256[],address[],uint256) (gas: -2152 (-0.013%))
testSuccessfulSwapSource() (gas: -40464 (-39.734%))
testRandomnessRequestEmitsFailedEventWithRequire() (gas: -19404 (-39.801%))
testRandomnessRequestEmitsFailedEventWithCustomError() (gas: -19404 (-40.266%))
testRandomnessRequestEmitsSuccessEvent() (gas: -19401 (-45.259%))
testRetryFailsBeforeRequestDelayThresholdIsReached() (gas: -19361 (-46.165%))
testRequestRandomNumberFailsWhenPendingRequestExists() (gas: -19384 (-47.877%))
testSwapSourceFailsWithInsufficientFailedAttempts() (gas: -43308 (-50.575%))
testSwapSourceFailsWithInsufficientFailedAttemptsWhenNotEnoughTimeSinceReachingMaxFailedAttempts() (gas: -63138 (-55.264%))
testRetryIncrementsFailedSequentialAttempts() (gas: -41103 (-58.762%))
Overall gas change: -635733 (-0.960%)
```

### Gas Optimizations
| Number |Issue|Instances|Estimated Gas Savings| Gas savings from `forge snapshot --diff` | 
|:-:|:-|:-:|:-:|:-:|
|[G-01](#state-variables-can-be-packed-to-use-fewer-storage-slots) | State variables can be packed to use fewer storage slots | 6 | 12000 | ~40000 | 
| [G-02](#structs-can-be-packed-to-use-fewer-storage-slots) | Structs can be packed to use fewer storage slots | 4 | 8000 | - |
| [G-03](#state-variables-can-be-cached-instead-of-re-reading-them-from-storage) | State variables can be cached instead of re-reading them from storage | 11 | 1100 | **See instances* |
| [G-04](#avoid-emitting-storage-values) | Avoid emitting storage values | 1 | 100 | ~103 |
| [G-05](#multiple-accesses-of-a-mappingarray-should-use-a-storage-pointer) | Multiple accesses of a mapping/array should use a storage pointer | 1 | 40 | - |
| [G-06](#x--yx---y-costs-more-gas-than-x--x--yx--x---y-for-state-variables) |`x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables| 3 | 45 | ~45 |
| [G-07](#expressions-that-are-guaranteed-to-not-underoverflow-due-to-previous-if-or-require-statement-can-be-unchecked) | Expressions that are guaranteed to not under/overflow due to previous `if` or `require` statement can be unchecked | 1 | - | - |
| [G-08](#pre-increments-and-pre-decrements-are-cheaper-than-post-increments-and-post-decrements) | Pre-increments and pre-decrements are cheaper than post-increments and post-decrements| 1 | - | - |

## State variables can be packed to use fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L31-L34

### We are able to pack `currentDraw`, `lastDrawFinalTicketId`, and `drawExecutionInProgress` into a single storage slot to save 2 SLOTs (~4000 gas).

Given that `uint120` and `uint128` is used often in this codebase for variables that represent aspects of tickets, `uint120` should also be sufficient for `currentDraw` and `lastDrawFinalTicketId`. Alternatively, either of those variables can be of type `uint128` while the other is of type `uint120` as long as all three variables can fit into one single storage slot.

Functions which would benefit from these values occupying the same storage slot by incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)` for every subsequent storage slot access: [Lottery.receiveRandomNumber()](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L203-L232) and [Lottery.executeDraw()](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L133-L142)

*Gas Savings as shown via `forge snapshot --diff`*:
```
testWrongSetups() (gas: 51 (0.001%))
testBuyInvalidTicket() (gas: -9 (-0.004%))
testGetRewardsMultipleStakersStakePostIncome() (gas: -26 (-0.004%))
testClaimFees() (gas: -15 (-0.005%))
testGetRewardsMultipleStakersSplit() (gas: -41 (-0.005%))
testBuyMultipleTickets() (gas: -19 (-0.006%))
testExit() (gas: -39 (-0.006%))
testTransferFromDoesNotTransferRewards() (gas: -48 (-0.007%))
testTransferDoesNotTransferRewards() (gas: -48 (-0.007%))
testGetRewardsSingleStaker() (gas: -48 (-0.007%))
testBuyTicketDuringInitialPotRaise() (gas: 7219 (0.084%))
testFinalizeInitialPot(uint256,uint256) (gas: 7219 (0.085%))
testZeroClaimableAfterDeadline() (gas: -17542 (-0.225%))
testJackpotReturnToPot() (gas: -17423 (-0.235%))
testMultiClaim() (gas: -22088 (-1.266%))
testNonJackpotWinClaimable() (gas: -21951 (-1.988%))
testExecuteDraw() (gas: -21724 (-2.840%))
testBuyTicket() (gas: -21846 (-3.442%))
invariantSufficientFunds() (gas: 0 (NaN%))
testBuyClaimFinalize(uint256[],address[],uint256) (gas: 55438 (0.325%))
Overall gas change: -52940 (-0.080%)
```
```solidity
File: src/Lottery.sol
31:    uint256 public override lastDrawFinalTicketId;
32:
33:    bool public override drawExecutionInProgress;
34:    uint128 public override currentDraw;
```
```diff
diff --git a/src/Lottery.sol b/src/Lottery.sol
index 28d3477..f120f62 100644
--- a/src/Lottery.sol
+++ b/src/Lottery.sol
@@ -28,10 +28,10 @@ contract Lottery is ILottery, Ticket, LotterySetup, ReferralSystem, RNSourceCont

     address public immutable override stakingRewardRecipient;

-    uint256 public override lastDrawFinalTicketId;

+    uint120 public override currentDraw;
+    uint120 public override lastDrawFinalTicketId;
     bool public override drawExecutionInProgress;
-    uint128 public override currentDraw;
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L11-L16

### We are able to pack `source`, `failedSequentialAttempts`, `lastRequestTimestamp`, `maxFailedAttemptsReachedAt`, and `lastRequestFulfilled` into a single storage slot to save 4 SLOTs (~8000 gas).

`lastRequestTimestamp` and `maxFailedAttemptsReachedAt` are simply timestamps and therefore can be packed as a `uint40` value without fear of overflowing. See [reference 1](https://twitter.com/PaulRBerg/status/1591832937179250693) and [reference 2](https://twitter.com/RareSkills_io/status/1632033211609137156). The maximum number of failed attempts is 10 according to [Line 19 in RNSourceController.sol](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L19), therefore there should not be more than 10 sequential failed attempts. Under that assumption, `uint8` should be sufficient for `failedSequentialAttempts`. `lastRequestFulfilled` is a bool and only takes up 1 byte. `source` is an address and takes up 20 bytes.

Functions which would benefit from these values occupying the same storage slot by incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)` for every subsequent storage slot access: [RNSourceController.onRandomNumberFulfilled()](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L46-L56), [RNSourceController.retry()](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L60-L75), [RNSourceController.swapSource()](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L89-L104), [RNSourceController.requestRandomNumberFromSource()](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L106-L121), [RNSourceController.requestRandomNumber()](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L38-L44). 

It should also be noted that everytime `retry()` is called after `onRandomNumberFulfilled()` or `swapSource()` has been called, `failedSequentialAttempts` and `maxFailedAttemptsReachedAt` will undergo a `Gsset (20000 gas)` since it was previously set to 0. The same happens for `lastRequestFulfilled` when `retry()` is called after `requestRandomNumberFromSource()` has been called. However, with this optimmization we will not undergo a `Gsset` since the storage slot will always be occupied by `source` and therefore will not be empty/set to 0. In scenarios where `onRandomNumberFulfilled()` is called multiple times in succession, we will also avoid a `Gsreset (2900)` which would be a result of the storage slot's `zeroness` being unchanged, i.e. set to 0 from 0. This is also true for `requestRandomNumberFromSource()`.

*Gas Savings as shown via `forge snapshot --diff`*:
```
testWrongSetups() (gas: 564 (0.011%))
testRetryFailsWhenNoPendingRequest() (gas: 17 (0.160%))
testZeroClaimableAfterDeadline() (gas: -41618 (-0.533%))
testJackpotReturnToPot() (gas: -41618 (-0.561%))
testBuyTicketDuringInitialPotRaise() (gas: 48750 (0.568%))
testFinalizeInitialPot(uint256,uint256) (gas: 48750 (0.571%))
testMultiClaim() (gas: -25706 (-1.473%))
testNonJackpotWinClaimable() (gas: -25706 (-2.328%))
testExecuteDraw() (gas: -26012 (-3.400%))
testBuyTicket() (gas: -25706 (-4.051%))
testBuyClaimFinalize(uint256[],address[],uint256) (gas: -3374 (-0.020%))
testSuccessfulSwapSource() (gas: -39929 (-39.209%))
testRandomnessRequestEmitsFailedEventWithRequire() (gas: -19306 (-39.600%))
testRandomnessRequestEmitsFailedEventWithCustomError() (gas: -19306 (-40.062%))
testRandomnessRequestEmitsSuccessEvent() (gas: -19306 (-45.037%))
testRetryFailsBeforeRequestDelayThresholdIsReached() (gas: -19266 (-45.938%))
testRequestRandomNumberFailsWhenPendingRequestExists() (gas: -19289 (-47.642%))
testSwapSourceFailsWithInsufficientFailedAttempts() (gas: -42983 (-50.195%))
testSwapSourceFailsWithInsufficientFailedAttemptsWhenNotEnoughTimeSinceReachingMaxFailedAttempts() (gas: -62698 (-54.879%))
testRetryIncrementsFailedSequentialAttempts() (gas: -40893 (-58.462%))
Overall gas change: -374635 (-0.566%)
```
```solidity
File: src/RNSourceController.sol
11:    IRNSource public override source;
12:
13:    uint256 public override failedSequentialAttempts;
14:    uint256 public override maxFailedAttemptsReachedAt;
15:    uint256 public override lastRequestTimestamp;
16:    bool public override lastRequestFulfilled = true;
```
```diff
diff --git a/src/RNSourceController.sol b/src/RNSourceController.sol
index 2bd8cfc..738677b 100644
--- a/src/RNSourceController.sol
+++ b/src/RNSourceController.sol
@@ -10,10 +10,11 @@ import "src/interfaces/IRNSourceController.sol";
 abstract contract RNSourceController is Ownable2Step, IRNSourceController {
     IRNSource public override source;

-    uint256 public override failedSequentialAttempts;
-    uint256 public override maxFailedAttemptsReachedAt;
-    uint256 public override lastRequestTimestamp;
+    uint8 public override failedSequentialAttempts;
+    uint40 public override lastRequestTimestamp;
+    uint40 public override maxFailedAttemptsReachedAt;
     bool public override lastRequestFulfilled = true;
```

## Structs can be packed to use fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if values combined are <= 32 bytes). If the variables packed together are retrieved together in functions (more likely with structs) we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/interfaces/IReferralSystemDynamic.sol#L24-L28

### We are able to pack `minimumTicketsSold` and `factor` into a single storage slot to save 1 SLOT (~2000 gas).

Given that [`drawId`](https://github.com/code-423n4/2023-03-wenwin/blob/main/src/interfaces/ITicket.sol#L11-L18) is of type `uint128` and would theoretically represents all unique tickets, It should be sufficient to also give `minimumTicketsSold` a type of `uint128`. `factor` is stated to represent the percentage of a previous draw and therefore `uint128` should also be sufficient for this value.

```solidity
File: src/interfaces/IReferralSystemDynamic.sol
24: struct MinimumReferralsRequirement {
25:    uint256 minimumTicketsSold;
26:    uint256 factor;
27:    ReferralRequirementFactorType factorType;
28: }
```
```diff
diff --git a/src/interfaces/IReferralSystemDynamic.sol b/src/interfaces/IReferralSystemDynamic.sol
index d638c68..6636510 100644
--- a/src/interfaces/IReferralSystemDynamic.sol
+++ b/src/interfaces/IReferralSystemDynamic.sol
@@ -22,8 +22,8 @@ enum ReferralRequirementFactorType {
 ///                 is PERCENT or fixed amount if @param factorType is FIXED
 /// @param factorType Different factor types - PERCENT and FIXED
 struct MinimumReferralsRequirement {
-    uint256 minimumTicketsSold;
-    uint256 factor;
+    uint128 minimumTicketsSold;
+    uint128 factor;
     ReferralRequirementFactorType factorType;
 }
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/interfaces/ILotterySetup.sol#L63-L78

### We are able to rearrange the struct below to pack `token`,  `selectionSize`, and `selectionMax` into a single storage slot to save 1 SLOT (~2000 gas).

```solidity
File: src/interfaces/ILotterySetup.sol
63: struct LotterySetupParams {
64:    /// @dev Token to be used as reward token for the lottery
65:    IERC20 token;
66:    /// @dev Parameters of the draw schedule for the lottery
67:    LotteryDrawSchedule drawSchedule;
68:    /// @dev Price to pay for playing single game (including fee)
69:    uint256 ticketPrice;
70:    /// @dev Count of numbers user picks for the ticket
71:    uint8 selectionSize;
72:    /// @dev Max number user can pick
73:    uint8 selectionMax;
74:    /// @dev Expected payout for one ticket, expressed in `rewardToken`
75:    uint256 expectedPayout;
76:    /// @dev Array of fixed rewards per each non jackpot win
77:    uint256[] fixedRewards;
78: }
```
```diff
diff --git a/src/interfaces/ILotterySetup.sol b/src/interfaces/ILotterySetup.sol
index 780d1a3..3e304e1 100644
--- a/src/interfaces/ILotterySetup.sol
+++ b/src/interfaces/ILotterySetup.sol
@@ -63,14 +63,14 @@ struct LotteryDrawSchedule {
 struct LotterySetupParams {
     /// @dev Token to be used as reward token for the lottery
     IERC20 token;
-    /// @dev Parameters of the draw schedule for the lottery
-    LotteryDrawSchedule drawSchedule;
-    /// @dev Price to pay for playing single game (including fee)
-    uint256 ticketPrice;
     /// @dev Count of numbers user picks for the ticket
     uint8 selectionSize;
     /// @dev Max number user can pick
     uint8 selectionMax;
+    /// @dev Parameters of the draw schedule for the lottery
+    LotteryDrawSchedule drawSchedule;
+    /// @dev Price to pay for playing single game (including fee)
+    uint256 ticketPrice;
     /// @dev Expected payout for one ticket, expressed in `rewardToken`
     uint256 expectedPayout;
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/interfaces/ILotterySetup.sol#L53-L60

### We are able to pack `firstDrawScheduledAt`,  `drawPeriod`, and `drawCoolDownPeriod` into a single storage slot to save 2 SLOTs (~4000 gas).

`firstDrawScheduledAt`, `drawPeriod`, and `drawCoolDownPeriod` represent timestamps and therefore can be packed as `uint80` values without fear of overflowing. See [reference 1](https://twitter.com/PaulRBerg/status/1591832937179250693) and [reference 2](https://twitter.com/RareSkills_io/status/1632033211609137156).

```solidity
File: src/interfaces/ILotterySetup.sol
53: struct LotteryDrawSchedule {
54:    /// @dev First draw is scheduled to take place at this timestamp
55:    uint256 firstDrawScheduledAt;
56:    /// @dev Period for running lottery
57:    uint256 drawPeriod;
58:    /// @dev Cooldown period when users cannot register tickets for draw anymore
59:    uint256 drawCoolDownPeriod;
60: }
```
```diff
diff --git a/src/interfaces/ILotterySetup.sol b/src/interfaces/ILotterySetup.sol
index 780d1a3..3e304e1 100644
--- a/src/interfaces/ILotterySetup.sol
+++ b/src/interfaces/ILotterySetup.sol
@@ -52,11 +52,11 @@ error TicketRegistrationClosed(uint128 drawId);
 /// @dev Lottery draw schedule parameters
 struct LotteryDrawSchedule {
     /// @dev First draw is scheduled to take place at this timestamp
-    uint256 firstDrawScheduledAt;
+    uint80 firstDrawScheduledAt;
     /// @dev Period for running lottery
-    uint256 drawPeriod;
+    uint80 drawPeriod;
     /// @dev Cooldown period when users cannot register tickets for draw anymore
-    uint256 drawCoolDownPeriod;
+    uint80 drawCoolDownPeriod;
 }
```

## State variables can be cached instead of re-reading them from storage
Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read.

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L112-L118

### Cache `source` as a stack variable to save 1 SLOAD.

*Gas Savings as shown via `forge snapshot --diff`*:
```
testMultiClaim() (gas: -101 (-0.006%))
testNonJackpotWinClaimable() (gas: -101 (-0.009%))
testBuyTicketDuringInitialPotRaise() (gas: -1211 (-0.014%))
testFinalizeInitialPot(uint256,uint256) (gas: -1211 (-0.014%))
testBuyTicket() (gas: -101 (-0.016%))
testExecuteDraw() (gas: -202 (-0.026%))
testZeroClaimableAfterDeadline() (gas: -5353 (-0.069%))
testJackpotReturnToPot() (gas: -5353 (-0.072%))
testRandomnessRequestEmitsFailedEventWithRequire() (gas: -104 (-0.213%))
testRandomnessRequestEmitsFailedEventWithCustomError() (gas: -104 (-0.216%))
testRandomnessRequestEmitsSuccessEvent() (gas: -101 (-0.236%))
testRetryFailsBeforeRequestDelayThresholdIsReached() (gas: -101 (-0.241%))
testRequestRandomNumberFailsWhenPendingRequestExists() (gas: -101 (-0.249%))
testRetryIncrementsFailedSequentialAttempts() (gas: -202 (-0.289%))
testSwapSourceFailsWithInsufficientFailedAttemptsWhenNotEnoughTimeSinceReachingMaxFailedAttempts() (gas: -404 (-0.354%))
testSwapSourceFailsWithInsufficientFailedAttempts() (gas: -303 (-0.354%))
testSuccessfulSwapSource() (gas: -404 (-0.397%))
```
```solidity
File: src/RNSourceController.sol
106:    function requestRandomNumberFromSource() private {
107:        lastRequestTimestamp = block.timestamp;
108:       lastRequestFulfilled = false;
109:
110:        // slither-disable-start uninitialized-local
111:        // See Slither issue: https://github.com/crytic/slither/issues/511
112:        try source.requestRandomNumber() { // @audit: 1st sload
113:            emit SuccessfulRNRequest(source); // @audit: 2nd sload
114:        } catch Error(string memory reason) {
115:            emit FailedRNRequest(source, bytes(reason)); // @audit: 2nd sload (1st catch case)
116:        } catch (bytes memory reason) {
117:            emit FailedRNRequest(source, reason); // @audit: 2nd sload (2nd catch case)
118:        }
119:        // slither-disable-end uninitialized-local
120:    }
```
```diff
diff --git a/src/RNSourceController.sol b/src/RNSourceController.sol
index 2bd8cfc..b72ca05 100644
--- a/src/RNSourceController.sol
+++ b/src/RNSourceController.sol
@@ -109,13 +110,16 @@ abstract contract RNSourceController is Ownable2Step, IRNSourceController {

         // slither-disable-start uninitialized-local
         // See Slither issue: https://github.com/crytic/slither/issues/511
-        try source.requestRandomNumber() {
-            emit SuccessfulRNRequest(source);
+        IRNSource _source = source;
+        try _source.requestRandomNumber() {
+            emit SuccessfulRNRequest(_source);
         } catch Error(string memory reason) {
-            emit FailedRNRequest(source, bytes(reason));
+            emit FailedRNRequest(_source, bytes(reason));
         } catch (bytes memory reason) {
-            emit FailedRNRequest(source, reason);
+            emit FailedRNRequest(_source, reason);
         }
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L271-L277

### Cache `currentDraw` as a stack variable to save to save 1 SLOAD.

*Gas Savings as shown via `forge snapshot --diff`*:
```
testZeroClaimableAfterDeadline() (gas: -32 (-0.000%))
testJackpotReturnToPot() (gas: -32 (-0.000%))
testBuyTicketDuringInitialPotRaise() (gas: -3619 (-0.042%))
testFinalizeInitialPot(uint256,uint256) (gas: -3619 (-0.042%))
```
```solidity
File: src/Lottery.sol
271:    function returnUnclaimedJackpotToThePot() private {
272:        if (currentDraw >= LotteryMath.DRAWS_PER_YEAR) { // @audit: 1st sload
273:            uint128 drawId = currentDraw - LotteryMath.DRAWS_PER_YEAR; // @audit: 2nd sload
274;            uint256 unclaimedJackpotTickets = unclaimedCount[drawId][winningTicket[drawId]];
275:            currentNetProfit += int256(unclaimedJackpotTickets * winAmount[drawId][selectionSize]);
276:        }
277:    }
```
```diff
diff --git a/src/Lottery.sol b/src/Lottery.sol
index 28d3477..fe184cb 100644
--- a/src/Lottery.sol
+++ b/src/Lottery.sol
@@ -269,8 +270,9 @@ contract Lottery is ILottery, Ticket, LotterySetup, ReferralSystem, RNSourceCont
     }

     function returnUnclaimedJackpotToThePot() private {
-        if (currentDraw >= LotteryMath.DRAWS_PER_YEAR) {
-            uint128 drawId = currentDraw - LotteryMath.DRAWS_PER_YEAR;
+        uint128 _currentDraw = currentDraw;
+        if (_currentDraw >= LotteryMath.DRAWS_PER_YEAR) {
+            uint128 drawId = _currentDraw - LotteryMath.DRAWS_PER_YEAR;
             uint256 unclaimedJackpotTickets = unclaimedCount[drawId][winningTicket[drawId]];
             currentNetProfit += int256(unclaimedJackpotTickets * winAmount[drawId][selectionSize]);
         }
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L159-L168

### Cache `ticketInfo.drawId` as a stack variable to save 2 SLOADs.  

*Gas Savings as shown via `forge snapshot --diff`*:
```
testMultiClaim() (gas: -26 (-0.001%))
testNonJackpotWinClaimable() (gas: -26 (-0.002%))
testExecuteDraw() (gas: -26 (-0.003%))
testZeroClaimableAfterDeadline() (gas: -686 (-0.009%))
testBuyTicketDuringInitialPotRaise() (gas: -1011 (-0.012%))
testFinalizeInitialPot(uint256,uint256) (gas: -1011 (-0.012%))
testJackpotReturnToPot() (gas: -1372 (-0.018%))
```
```solidity
File: src/Lottery.sol
159:    function claimable(uint256 ticketId) external view override returns (uint256 claimableAmount, uint8 winTier) {
160:        TicketInfo memory ticketInfo = ticketsInfo[ticketId];
161:        if (!ticketInfo.claimed) {
162:            uint120 _winningTicket = winningTicket[ticketInfo.drawId]; // @audit: 1st sload
163:            winTier = TicketUtils.ticketWinTier(ticketInfo.combination, _winningTicket, selectionSize, selectionMax);
164:            if (block.timestamp <= ticketRegistrationDeadline(ticketInfo.drawId + LotteryMath.DRAWS_PER_YEAR)) { // @audit: 2nd sload
165:                claimableAmount = winAmount[ticketInfo.drawId][winTier]; // @audit: 3rd sload
166:            }
167:       }
168:    }
```
```diff
diff --git a/src/Lottery.sol b/src/Lottery.sol
index 28d3477..fe184cb 100644
--- a/src/Lottery.sol
+++ b/src/Lottery.sol
@@ -159,10 +159,11 @@ contract Lottery is ILottery, Ticket, LotterySetup, ReferralSystem, RNSourceCont
     function claimable(uint256 ticketId) external view override returns (uint256 claimableAmount, uint8 winTier) {
         TicketInfo memory ticketInfo = ticketsInfo[ticketId];
         if (!ticketInfo.claimed) {
-            uint120 _winningTicket = winningTicket[ticketInfo.drawId];
+            uint128 _drawId = ticketInfo.drawId;
+            uint120 _winningTicket = winningTicket[_drawId];
             winTier = TicketUtils.ticketWinTier(ticketInfo.combination, _winningTicket, selectionSize, selectionMax);
-            if (block.timestamp <= ticketRegistrationDeadline(ticketInfo.drawId + LotteryMath.DRAWS_PER_YEAR)) {
-                claimableAmount = winAmount[ticketInfo.drawId][winTier];
+            if (block.timestamp <= ticketRegistrationDeadline(_drawId + LotteryMath.DRAWS_PER_YEAR)) {
+                claimableAmount = winAmount[_drawId][winTier];
             }
         }
     }
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/ReferralSystem.sol#L136-L154

### Cache `_unclaimedTickets.referrerTicketCount` and `_unclaimedTickets.playerTicketCount` as stack variables to avoid 2 SLOADs. **Note that we can also create a storage pointer for `unclaimedTickets[drawId][msg.sender]` since the mapping is accessed multiple times. The c4udit tool caught only one line of this issue. I am including it here to showcase the parts that the c4udit tool missed**

*Gas Savings as shown via `forge snapshot --diff`*:
```
testBuyTicketDuringInitialPotRaise() (gas: -32897 (-0.383%))
testFinalizeInitialPot(uint256,uint256) (gas: -32897 (-0.385%))
```
```solidity
File: src/ReferralSystem.sol
136:    function claimPerDraw(uint128 drawId) private returns (uint256 claimedReward) {
137:        requireFinishedDraw(drawId);
138:
139:        UnclaimedTicketsData memory _unclaimedTickets = unclaimedTickets[drawId][msg.sender]; // @audit: can create a storage pointer and use it for *both* if statments
140:        if (_unclaimedTickets.referrerTicketCount >= minimumEligibleReferrals[drawId]) { // @audit: 1st sload
141:            claimedReward = referrerRewardPerDrawForOneTicket[drawId] * _unclaimedTickets.referrerTicketCount; // @audit: 2nd sload
142:            unclaimedTickets[drawId][msg.sender].referrerTicketCount = 0; // @audit: re-accessing the mapping
143:        } 
144:
145:        _unclaimedTickets = unclaimedTickets[drawId][msg.sender]; // @audit: can use previously created storage pointer
146:        if (_unclaimedTickets.playerTicketCount > 0) { // @audit: 1st sload
147:            claimedReward += playerRewardsPerDrawForOneTicket[drawId] * _unclaimedTickets.playerTicketCount; // @audit: 2nd sload
148:            unclaimedTickets[drawId][msg.sender].playerTicketCount = 0; // @audit: re-accessing the mapping
149:        }
150:
151:        if (claimedReward > 0) {
152:            emit ClaimedReferralReward(drawId, msg.sender, claimedReward);
153:        }
154:    }
```
```diff
diff --git a/src/ReferralSystem.sol b/src/ReferralSystem.sol
index d74ced7..585db42 100644
--- a/src/ReferralSystem.sol
+++ b/src/ReferralSystem.sol
@@ -136,16 +137,18 @@ abstract contract ReferralSystem is IReferralSystem {
     function claimPerDraw(uint128 drawId) private returns (uint256 claimedReward) {
         requireFinishedDraw(drawId);

-        UnclaimedTicketsData memory _unclaimedTickets = unclaimedTickets[drawId][msg.sender];
-        if (_unclaimedTickets.referrerTicketCount >= minimumEligibleReferrals[drawId]) {
-            claimedReward = referrerRewardPerDrawForOneTicket[drawId] * _unclaimedTickets.referrerTicketCount;
-            unclaimedTickets[drawId][msg.sender].referrerTicketCount = 0;
+        UnclaimedTicketsData storage _unclaimedTickets = unclaimedTickets[drawId][msg.sender];
+
+        uint128 referrerTicketCount = _unclaimedTickets.referrerTicketCount;
+        if (referrerTicketCount >= minimumEligibleReferrals[drawId]) {
+            claimedReward = referrerRewardPerDrawForOneTicket[drawId] * referrerTicketCount;
+            _unclaimedTickets.referrerTicketCount = 0;
         }

-        _unclaimedTickets = unclaimedTickets[drawId][msg.sender];
-        if (_unclaimedTickets.playerTicketCount > 0) {
-            claimedReward += playerRewardsPerDrawForOneTicket[drawId] * _unclaimedTickets.playerTicketCount;
-            unclaimedTickets[drawId][msg.sender].playerTicketCount = 0;
+        uint128 playerTicketCount = _unclaimedTickets.playerTicketCount;
+        if (playerTicketCount > 0) {
+            claimedReward += playerRewardsPerDrawForOneTicket[drawId] * playerTicketCount;
+            _unclaimedTickets.playerTicketCount = 0;
         }
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceBase.sol#L33-L46

### Cache `requests[requestId].status` as a stack variable to save 1 SLOAD. **Note that we can also create a storage pointer for `requests[requestId]` since the mapping is accessed multiple times**

*Gas Savings as shown via `forge snapshot --diff`*:
```
testFulfillRandomNumber() (gas: -225 (-0.375%))
testRawFulfillRandomWords() (gas: -379 (-0.518%))
Overall gas change: -604 (-0.001%)
```
```solidity
File: src/RNSourceBase.sol
33:    function fulfill(uint256 requestId, uint256 randomNumber) internal {
34:        if (requests[requestId].status == RequestStatus.None) { // @audit: 1st sload, 1st access of mapping
35:            revert RequestNotFound(requestId);
36:        }
37:        if (requests[requestId].status == RequestStatus.Fulfilled) { // @audit: 2nd sload, 2nd access of mapping
38:            revert RequestAlreadyFulfilled(requestId);
39:        }
40:
41:        requests[requestId].randomNumber = randomNumber; // @audit: 3rd access of mapping
42:        requests[requestId].status = RequestStatus.Fulfilled; // @audit: 4th access of mapping
43:        IRNSourceConsumer(authorizedConsumer).onRandomNumberFulfilled(randomNumber);
44:
45:        emit RequestFulfilled(requestId, randomNumber);
46:    }
```
```diff
diff --git a/src/RNSourceBase.sol b/src/RNSourceBase.sol
index 616fba8..124a013 100644
--- a/src/RNSourceBase.sol
+++ b/src/RNSourceBase.sol
@@ -31,15 +31,17 @@ abstract contract RNSourceBase is IRNSource {

     /// @dev Must be called in the deriving contract's `requestRandomnessFromUnderlyingSource` function
     function fulfill(uint256 requestId, uint256 randomNumber) internal {
-        if (requests[requestId].status == RequestStatus.None) {
+        RandomnessRequest storage request = requests[requestId];
+        RequestStatus _status = request.status;
+        if (_status == RequestStatus.None) {
             revert RequestNotFound(requestId);
         }
-        if (requests[requestId].status == RequestStatus.Fulfilled) {
+        if (_status == RequestStatus.Fulfilled) {
             revert RequestAlreadyFulfilled(requestId);
         }

-        requests[requestId].randomNumber = randomNumber;
-        requests[requestId].status = RequestStatus.Fulfilled;
+        request.randomNumber = randomNumber;
+        request.status = RequestStatus.Fulfilled;
         IRNSourceConsumer(authorizedConsumer).onRandomNumberFulfilled(randomNumber);

         emit RequestFulfilled(requestId, randomNumber);
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/ReferralSystem.sol#L52-L72 

### Cache `unclaimedTickets[currentDraw][referrer].referrerTicketCount` as a stack variable to save at least 1 SLOAD and at most 3 SLOADs. 

*Gas Savings as shown via `forge snapshot --diff`*:
```
testBuyTicketDuringInitialPotRaise() (gas: -20058 (-0.234%))
testFinalizeInitialPot(uint256,uint256) (gas: -20058 (-0.235%))
```
```solidity
File: src/ReferralSystem.sol
52:    function referralRegisterTickets(
53:        uint128 currentDraw,
54:        address referrer,
55:        address player,
56:        uint256 numberOfTickets
57:    )
58:        internal
59:    {
60:        if (referrer != address(0)) {
61:            uint256 minimumEligible = minimumEligibleReferrals[currentDraw];
62:            if (unclaimedTickets[currentDraw][referrer].referrerTicketCount + numberOfTickets >= minimumEligible) { // @audit: 1st sload
63:                if (unclaimedTickets[currentDraw][referrer].referrerTicketCount < minimumEligible) { // @audit: 2nd sload
64:                    totalTicketsForReferrersPerDraw[currentDraw] +=
65:                        unclaimedTickets[currentDraw][referrer].referrerTicketCount; // @audit: 3rd sload
66:                }
67:                totalTicketsForReferrersPerDraw[currentDraw] += numberOfTickets;
68:            }
69:            unclaimedTickets[currentDraw][referrer].referrerTicketCount += uint128(numberOfTickets); // @audit: 4th sload
70:        }
71:        unclaimedTickets[currentDraw][player].playerTicketCount += uint128(numberOfTickets);
72:    }
```
```diff
diff --git a/./src/ReferralSystem.sol b/./src/ReferrelSystemNew.sol
index d74ced7..a11d92b 100644
--- a/./src/ReferralSystem.sol
+++ b/./src/ReferrelSystemNew.sol
@@ -59,14 +59,15 @@ abstract contract ReferralSystem is IReferralSystem {
     {
         if (referrer != address(0)) {
             uint256 minimumEligible = minimumEligibleReferrals[currentDraw];
-            if (unclaimedTickets[currentDraw][referrer].referrerTicketCount + numberOfTickets >= minimumEligible) {
-                if (unclaimedTickets[currentDraw][referrer].referrerTicketCount < minimumEligible) {
+            uint128 referrerUnclaimedTickets = unclaimedTickets[currentDraw][referrer].referrerTicketCount;
+            if (referrerUnclaimedTickets + numberOfTickets >= minimumEligible) {
+                if (referrerUnclaimedTickets < minimumEligible) {
                     totalTicketsForReferrersPerDraw[currentDraw] +=
-                        unclaimedTickets[currentDraw][referrer].referrerTicketCount;
+                        referrerUnclaimedTickets;
                 }
                 totalTicketsForReferrersPerDraw[currentDraw] += numberOfTickets;
             }
-            unclaimedTickets[currentDraw][referrer].referrerTicketCount += uint128(numberOfTickets);
+            unclaimedTickets[currentDraw][referrer].referrerTicketCount = referrerUnclaimedTickets + uint128(numberOfTickets);
         }
         unclaimedTickets[currentDraw][player].playerTicketCount += uint128(numberOfTickets);
     }
```

## Avoid emitting storage values

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/RNSourceController.sol#L60-L75 **Get rid of this one!!!**

### Use already cached `failedAttempts` stack variable instead of emitting a storage value

*Gas Savings as shown via `forge snapshot --diff`*:
```
testBuyTicketDuringInitialPotRaise() (gas: -600 (-0.007%))
testFinalizeInitialPot(uint256,uint256) (gas: -600 (-0.007%))
testRetryIncrementsFailedSequentialAttempts() (gas: -103 (-0.147%))
testSwapSourceFailsWithInsufficientFailedAttempts() (gas: -206 (-0.241%))
testSuccessfulSwapSource() (gas: -247 (-0.243%))
testSwapSourceFailsWithInsufficientFailedAttemptsWhenNotEnoughTimeSinceReachingMaxFailedAttempts() (gas: -309 (-0.270%))
```
```solidity
60:    function retry() external override {
61:        if (lastRequestFulfilled) {
62:            revert CannotRetrySuccessfulRequest();
63:        }
64:        if (block.timestamp - lastRequestTimestamp <= maxRequestDelay) {
65:            revert CurrentRequestStillActive();
66:        }
67:
68:        uint256 failedAttempts = ++failedSequentialAttempts; // @audit: 1st sload
69:        if (failedAttempts == maxFailedAttempts) {
70:            maxFailedAttemptsReachedAt = block.timestamp;
71:        }
72:
73:        emit Retry(source, failedSequentialAttempts); // @audit: 2nd sload
74:        requestRandomNumberFromSource();
75:    }
```
```diff
diff --git a/src/RNSourceController.sol b/src/RNSourceController.sol
index 0d607cd..33508be 100644
--- a/src/RNSourceController.sol
+++ b/src/RNSourceController.sol
@@ -70,7 +71,7 @@ abstract contract RNSourceController is Ownable2Step, IRNSourceController {
             maxFailedAttemptsReachedAt = block.timestamp;
         }

-        emit Retry(source, failedSequentialAttempts);
+        emit Retry(source, failedAttempts);
         requestRandomNumberFromSource();
     }
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/LotterySetup.sol#L147

### Use already cached `raised` stack variable instead of re-reading from storage to save 1 SLOAD.

```solidity
File: src/LotterySetup.sol
132:    function finalizeInitialPotRaise() external override {
133:       if (initialPot > 0) { // @audit: 1st sload
134:            revert JackpotAlreadyInitialized();
135:        }
136:        // slither-disable-next-line timestamp
137:        if (block.timestamp <= initialPotDeadline) {
138:            revert FinalizingInitialPotBeforeDeadline();
139:        }
140:        uint256 raised = rewardToken.balanceOf(address(this));
141:        if (raised < minInitialPot) {
142:            revert RaisedInsufficientFunds(raised);
143:        }
144:        initialPot = raised;
145:
146:        // must hold after this call, this will be used as a check that jackpot is initialized
147:        assert(initialPot > 0); // @audit: 2nd sload, use `raised`
148:
149:        emit InitialPotPeriodFinalized(raised);
150:    }
```
```diff
diff --git a/src/LotterySetup.sol b/src/LotterySetupsol
index 9c70086..6e6492f 100644
--- a/src/LotterySetup.sol
+++ b/src/LotterySetup.sol
@@ -144,7 +144,7 @@ contract LotterySetup is ILotterySetup {
         initialPot = raised;

         // must hold after this call, this will be used as a check that jackpot is initialized
-        assert(initialPot > 0);
+        assert(raised > 0);

         emit InitialPotPeriodFinalized(raised);
     }
```

## Multiple accesses of a mapping/array should use a storage pointer
Caching a mapping's value in a storage pointer when the value is accessed multiple times saves ~40 gas per access due to not having to perform the same offset calculation every time. Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it.

To achieve this, declare a storage pointer for the variable and use it instead of repeatedly fetching the reference in a map or an array. As an example, instead of repeatedly calling `ticketsInfo[ticketId]`, save its reference via a storage pointer: `TicketInfo storage ticketInfo = ticketsInfo[ticketId]` and use the pointer instead. **Note that this issue may also be seen in combination with caching storage variables, when applicable**

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L266

### Cache `ticketsInfo[ticketId]` as a storage pointer.

```solidity
File: src/Lottery.sol
259:    function claimWinningTicket(uint256 ticketId) private onlyTicketOwner(ticketId) returns (uint256 claimedAmount) {
260:        uint256 winTier;
270:        (claimedAmount, winTier) = this.claimable(ticketId);
271:        if (claimedAmount == 0) {
272:            revert NothingToClaim(ticketId);
273:        }
274:
275:        unclaimedCount[ticketsInfo[ticketId].drawId][ticketsInfo[ticketId].combination]--; // @audit: mapping accessed twice
276:        markAsClaimed(ticketId);
277:        emit ClaimedTicket(msg.sender, ticketId, claimedAmount);
278:    }
```
```diff
diff --git a/src/Lottery.sol b/src/Lottery.sol
index 28d3477..a519426 100644
--- a/src/Lottery.sol
+++ b/src/Lottery.sol
@@ -262,8 +263,9 @@ contract Lottery is ILottery, Ticket, LotterySetup, ReferralSystem, RNSourceCont
         if (claimedAmount == 0) {
             revert NothingToClaim(ticketId);
         }
-
-        unclaimedCount[ticketsInfo[ticketId].drawId][ticketsInfo[ticketId].combination]--;
+
+        TicketInfo storage ticketInfo = ticketsInfo[ticketId];
+        unclaimedCount[ticketInfo.drawId][ticketInfo.combination]--;
         markAsClaimed(ticketId);
         emit ClaimedTicket(msg.sender, ticketId, claimedAmount);
```

## `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L271-L277 

*Gas Savings as shown via `forge snapshot --diff`*:
```
testZeroClaimableAfterDeadline() (gas: -16 (-0.000%))
testJackpotReturnToPot() (gas: -16 (-0.000%))
testBuyTicketDuringInitialPotRaise() (gas: -1211 (-0.014%))
testFinalizeInitialPot(uint256,uint256) (gas: -1211 (-0.014%))
```
```solidity
File: src/Lottery.sol
271:    function returnUnclaimedJackpotToThePot() private {
272:        if (currentDraw >= LotteryMath.DRAWS_PER_YEAR) {
273:            uint128 drawId = currentDraw - LotteryMath.DRAWS_PER_YEAR;
274:            uint256 unclaimedJackpotTickets = unclaimedCount[drawId][winningTicket[drawId]];
275:            currentNetProfit += int256(unclaimedJackpotTickets * winAmount[drawId][selectionSize]);
276:        }
277:    }
```
```diff
diff --git a/src/Lottery.sol b/src/Lottery.sol
index 28d3477..dd062bb 100644
--- a/./src/Lottery.sol
+++ b/src/Lottery.sol
@@ -272,7 +272,7 @@ contract Lottery is ILottery, Ticket, LotterySetup, ReferralSystem, RNSourceCont
         if (currentDraw >= LotteryMath.DRAWS_PER_YEAR) {
             uint128 drawId = currentDraw - LotteryMath.DRAWS_PER_YEAR;
             uint256 unclaimedJackpotTickets = unclaimedCount[drawId][winningTicket[drawId]];
-            currentNetProfit += int256(unclaimedJackpotTickets * winAmount[drawId][selectionSize]);
+            currentNetProfit = currentNetProfit + int256(unclaimedJackpotTickets * winAmount[drawId][selectionSize]);
         }
     }
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/staking/StakedTokenLock.sol#L24-L48

*Gas Savings as shown via `forge snapshot --diff`*:
```
testWithdrawAfterDeadlineBeforeDurationEnd() (gas: -12 (-0.011%))
testDepositOwnerOnly() (gas: -13 (-0.012%))
testDepositPullsStakedTokens() (gas: -13 (-0.012%))
testWithdrawOwnerOnly() (gas: -25 (-0.019%))
testWithdrawSendsStakedTokens() (gas: -25 (-0.020%))
testWithdrawAfterDeadlineAfterDurationEnd() (gas: -26 (-0.021%))
testWithdrawBeforeDeadline() (gas: -26 (-0.021%))
Overall gas change: -140 (-0.000%)
```
```solidity
File: src/staking/StakedTokenLock.sol
24:    function deposit(uint256 amount) external override onlyOwner {
25:        // slither-disable-next-line timestamp
26:        if (block.timestamp > depositDeadline) {
27:            revert DepositPeriodOver();
28:        }
29:
30:        depositedBalance += amount;
31:
32:        // No need for SafeTransferFrom, only trusted staked token is used.
33:        // slither-disable-next-line unchecked-transfer
34:        stakedToken.transferFrom(msg.sender, address(this), amount);
35:    }
36:
37:    function withdraw(uint256 amount) external override onlyOwner {
38:        // slither-disable-next-line timestamp
39:        if (block.timestamp > depositDeadline && block.timestamp < depositDeadline + lockDuration) {
40:            revert LockPeriodOngoing();
41:        }
42:
43:        depositedBalance -= amount;
44:
45:        // No need for SafeTransfer, only trusted staked token is used.
46:        // slither-disable-next-line unchecked-transfer
47:        stakedToken.transfer(msg.sender, amount);
48:    }
```
```diff
diff --git a/src/staking/StakedTokenLock.sol b/src/staking/StakedTokenLock.sol
index 0d607cd..33508be 100644
--- a/src/staking/StakedTokenLock.sol
+++ b/src/staking/StakedTokenLock.sol
@@ -27,7 +27,7 @@ contract StakedTokenLock is IStakedTokenLock, Ownable2Step {
             revert DepositPeriodOver();
         }

-        depositedBalance = + amount;
+        depositedBalance = depositedBalance + amount;

         // No need for SafeTransferFrom, only trusted staked token is used.
         // slither-disable-next-line unchecked-transfer
@@ -40,7 +40,7 @@ contract StakedTokenLock is IStakedTokenLock, Ownable2Step {
             revert LockPeriodOngoing();
         }

-        depositedBalance -= amount;
+        depositedBalance = depositedBalance - amount;

         // No need for SafeTransfer, only trusted staked token is used.
         // slither-disable-next-line unchecked-transfer
@@ -55,3 +55,4 @@ contract StakedTokenLock is IStakedTokenLock, Ownable2Step {
         rewardsToken.transfer(owner(), rewardsToken.balanceOf(address(this)));
     }
 }
```

## Expressions that are guaranteed to not under/overflow due to previous `if` or `require` statement can be unchecked

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L272-L273

*Gas Savings as shown via `forge snapshot --diff`*:
```
testZeroClaimableAfterDeadline() (gas: -124 (-0.002%))
testJackpotReturnToPot() (gas: -124 (-0.002%))
testBuyTicketDuringInitialPotRaise() (gas: -10439 (-0.122%))
testFinalizeInitialPot(uint256,uint256) (gas: -10439 (-0.122%))
```
```solidity
File: src/Lotter.sol
271:    function returnUnclaimedJackpotToThePot() private {
272:       if (currentDraw >= LotteryMath.DRAWS_PER_YEAR) {
273:            uint128 drawId = currentDraw - LotteryMath.DRAWS_PER_YEAR;
274:            uint256 unclaimedJackpotTickets = unclaimedCount[drawId][winningTicket[drawId]];
275:            currentNetProfit += int256(unclaimedJackpotTickets * winAmount[drawId][selectionSize]);
276:        }
277:    }
```
```diff
diff --git a/src/Lottery.sol b/src/Lottery.sol
index 28d3477..dd062bb 100644
--- a/src/Lottery.sol
+++ b/src/Lottery.sol
@@ -270,7 +270,10 @@ contract Lottery is ILottery, Ticket, LotterySetup, ReferralSystem, RNSourceCont

     function returnUnclaimedJackpotToThePot() private {
         if (currentDraw >= LotteryMath.DRAWS_PER_YEAR) {
-            uint128 drawId = currentDraw - LotteryMath.DRAWS_PER_YEAR;
+            uint128 drawId;
+            unchecked {
+                drawId = currentDraw - LotteryMath.DRAWS_PER_YEAR;
+            }
             uint256 unclaimedJackpotTickets = unclaimedCount[drawId][winningTicket[drawId]];
             currentNetProfit += int256(unclaimedJackpotTickets * winAmount[drawId][selectionSize]);
         }
```

## Pre-increments and pre-decrements are cheaper than post-increments and post-decrements
**Note that these are instances the c4udit tool missed**

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol#L193-L194

```solidity
File: src/Lottery.sol
181:    function registerTicket(
182:        uint128 drawId,
183:        uint120 ticket,
184:        address frontend,
185:        address referrer
186:    )
187:        private
188:        beforeTicketRegistrationDeadline(drawId)
189:        requireValidTicket(ticket)
190:        returns (uint256 ticketId)
191:    {
192:        ticketId = mint(msg.sender, drawId, ticket);
193:        unclaimedCount[drawId][ticket]++;
194:        ticketsSold[drawId]++;
195:        emit NewTicket(currentDraw, ticketId, drawId, msg.sender, ticket, frontend, referrer);
196:    }
```
```diff
diff --git a/src/Lottery.sol b/src/Lottery.sol
index 28d3477..dd062bb 100644
--- a/src/Lottery.sol
+++ b/src/Lottery.sol
@@ -190,8 +190,8 @@ contract Lottery is ILottery, Ticket, LotterySetup, ReferralSystem, RNSourceCont
         returns (uint256 ticketId)
     {
         ticketId = mint(msg.sender, drawId, ticket);
-        unclaimedCount[drawId][ticket]++;
-        ticketsSold[drawId]++;
+        ++unclaimedCount[drawId][ticket];
+        ++ticketsSold[drawId];
         emit NewTicket(currentDraw, ticketId, drawId, msg.sender, ticket, frontend, referrer);
     }
```
