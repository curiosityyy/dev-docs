# Risk management

### Allowed token list
Allowed tokens list is key CreditManager parameter, which contains tokens used to compute collateral. It's limited with 256 tokens, and if token added once, it could not be removed from this list. Underlying token is always has `0` index in this list.

Credit account could be shown as list of allowed tokens with balances, our risk models based on three parameters:

![Opening credit account](</images/credit/creditAccount.jpg>)


### Total value
Represents how much money in underlying balance could be got if user swap all assets into underlying one using chainlink oracle based prices.

### Threshold wighted value
To make position overcollaterized, Gearbox uses another parameter which is called `Threshold wighted value`, which has one more multiplier called
Liquidation Threshold (LTi). It represents maximum expected price drop during liquidation time between i-asset and underlying one. 


he

### Collateral check
After each transaction, Gearbox checks that the account has enough collateral, otherwise (if hf< 1) it's reverted. 

### Enabled token mask
To reduce gas usage, each credit account has enabledTokenMask paramter. Each bit of `uint256` value representa that corresponding token in allowed token list array has non-zero balance and should be computed during collateral check. To get `enabledTokenMask` programatically, you should call:

``


Enabling token is resposibility of adapter developer, and it should be done, otherwise threshold total value wouldn't be computed properly.

To enable token, developers should call `checkAndEnableToken(address creditAccount, address tokenOut)` in `CreditManager` from adapter contract. If during checking collateral, protocol recognises that i-asset has zero balance, this token would be automatically disabled.