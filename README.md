## Gas optimizations tips found in open source audits 💡

⬜   `REQUIRE()/REVERT()` STRINGS LONGER THAN 32 BYTES COST EXTRA GAS
```python
def utf8len(s):
    return len(s.encode('utf-8'))

utf8len('FCash to redeem must be an index component')
```

---

⬜  `PRIVATE` FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS

---

⬜ `PUBLIC` FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED `EXTERNAL` INSTEAD

---

⬜   ADDING `UNCHEKED` DIRECTIVE CAN SAVE GAS
> For arithmetic operations that will not owerflow/underflow anyone, use unchecked, this will help save gas from unnecessary > owerflow/underflow checks in solidity version starting with 0.8

---

⬜   `array.length` SHOULD NOT BE LOOKED UP IN EVERY LOOP OF FOR-LOOP
> Reading array length at each iteration of the loop takes 6 gas

---

⬜   `++i` COSTS LESS GAS COMPARED TO *i++* OR *i += 1* (SAME FOR `--i` VS `i--` OR `i -= 1`)
    Increment:
- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)
    Decrement:
- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `-i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)
---

⬜   USE `address(0)` INSTEAD OF `0x00000000000000000000000000000000000000000000000000000000000000`

---

⬜   CHECK IF `AMOUNT > 0` BEFORE TOKEN TRANSFER CAN SAVE GAS
> It worth cheking the `AMOUNT ≠ 0`, before the transfer can create an external call and spend unnecessary gas cost

---

⬜   `> 0` CAN BE REPLACED WITH `!= 0` FOR GAS OPTIMIZATION
> `!= 0` is a cheaper operation compared to `> 0`, when dealing with uint

---

⬜   CONSIDER MAKING SOME CONSTANTS AS NON-PUBLIC TO SAVE GAS
> Switching from public to private/internal can save gas when the constant is not used outside od its contract
---

⬜ UNUSED NAMED RETURN VARIABLES - 3 gas per instance

---

⬜  CONSTRUCTOR PARAMETERS SHOULD BE AVOIDED WHEN POSSIBLE
> Constructor parameters are expensive. The contract deployment will be cheaper in gas if they are hard coded instead of using constructor parameters

---

⬜  CALLDATA INSTEAD OF MEMORY FOR READ-ONLY FUNCTION PARAMETERS
> If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. (Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory)
--- 

⬜  CACHING STORAGE VARIABLES IN MEMORY TO SAVE GAS
> Anytime you are reading from storage more than once, it is cheaper in gas cost to cache the variable in memory: a SLOAD cost 100gas, while MLOAD and MSTORE cost 3 gas.
--- 

⬜  `uint256` instead of `uint8`
> The EVM works with 256bit/32byte words. Every operation is based on these base units. If the data used is smaller, further operation are needed to downscale from 256 bits to 8 bits.
--- 

⬜  `X = X + Y` IS CHEAPER THAN `X += Y` (Same for `X = X - Y` is cheaper than `X -= Y`)
> `X += Y` costs 3,more gas than `X = X + Y`.
---

⬜ UNNECESSARY COMPUTATION
> Gas can be saved by avoiding using local variables when they are not necessary.
---

⬜ AVOID A SLOAD OPTIMISTICALLY - 2.1k gas saved

---

⬜ CHECK msg.value FIRST - 1 gas per instance
> Reading msg.value costs 2 gas, while reading from memory costs 3, this will save 1 gas with no downside
> `if (weth == asset && msg.value > 0)` change to `if (msg.value > 0 && weth == asset)`
---

⬜ MIXING RETURN AND NAMED RETURNS
> Using both named returns and a return statement isn't necessary. Removing unused named return variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. 
> To save gas and improve code quality: consider using only one of those.
---

⬜ INLINE A MODIFIER THAT’S ONLY USED ONCE
> If modifier is only used once in this contract, it should get inlined to save gas
---

⬜  USE abi.encodePacked() not abi.encode()

---

⬜ USE NESTED IF AND, AVOID MULTIPLE CHECK COMBINATIONS 
> Using `nested if` cheaper then using `&&` multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

---

⬜ UNINITIALIZED VARIABLES ARE ASSIGNED WITH THE TYPES DEFAULT VALUE
> Explicitly initializing a variable with it's default value costs unnecesary gas

---
⬜ USE CUSTOM ERRORS INSTEAD OF REVERT STRINGS TO SAVE GAS
> Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met) while providing the same amount of information
> 
