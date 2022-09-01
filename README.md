## Gas optimizations tips found in open source audits 💡

*REQUIRE()/REVERT()* STRINGS LONGER THAN 32 BYTES COST EXTRA GAS
```python
def utf8len(s):
    return len(s.encode('utf-8'))

utf8len('FCash to redeem must be an index component')
```
---
⬜ * PRIVATE * FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS
---*
⬜ ADDING * UNCHEKED * DIRECTIVE CAN SAVE GAS
> For arithmetic operations that will not owerflow/underflow anyone, use unchecked, this will help save gas from unnecessary > owerflow/underflow checks in solidity version starting with 0.8
---
⬜ *array.length* SHOULD NOT BE LOOKED UP IN EVERY LOOP OF FOR-LOOP
> Reading array length at each iteration of the loop takes 6 gas
---
⬜ *++i* COSTS LESS GAS COMPARED TO *i++* OR *i += 1* (SAME FOR *--i* VS *i--* OR *i -= 1*)
**Increment:**
- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)
**Decrement:**
- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `-i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)
---
⬜ USE 'address(0)' INSTEAD OF '0x00000000000000000000000000000000000000000000000000000000000000'
---*
⬜ CHECK IF *AMOUNT > 0* BEFORE TOKEN TRANSFER CAN SAVE GAS
> IT IS WORTH CHECKING THE *AMOUNT ≠ 0*, BEFORE THE TRANSFER CAN CREATE AN EXTERNAL CALL AND SPEND UNNECESSARY GAS COST
---
⬜ *> 0* CAN BE REPLACED WITH *!= 0* FOR GAS OPTIMIZATION
> *!= 0* IS A CHEAPER OPERATION COMPARED TO *> 0*, WHEN DEALING WITH UINT
---
⬜ CONSIDER MAKING SOME CONSTANTS AS NON-PUBLIC TO SAVE GAS
> Switching from public to private/internal can save gas when the constant is not used outside od its contract
