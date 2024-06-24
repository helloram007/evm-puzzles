# EVM puzzles

A collection of EVM puzzles. Each puzzle consists on sending a successful transaction to a contract. The bytecode of the contract is provided, and you need to fill the transaction data that won't revert the execution.

## How to play

Clone this repository and install its dependencies (`npm install` or `yarn`). Then run:

```
npx hardhat play
```

And the game will start.

In some puzzles you only need to provide the value that will be sent to the contract, in others the calldata, and in others both values.

You can use [`evm.codes`](https://www.evm.codes/)'s reference and playground to work through this.

# Solutions

## Puzzle - 1
### Solution: 
00      34      CALLVALUE
01      38      CODESIZE
02      03      SUB
03      56      JUMP
04      FD      REVERT
05      FD      REVERT
06      5B      JUMPDEST
07      00      STOP
08      FD      REVERT
09      FD      REVERT

    CALLVALUE is the amount of Wei we send in.
    So in the stack the first opcode is CALLVALUE. next OPCODE is JUMP, JUMP takes a value to JUMPDEST, so the JUMPDEST is at location 8. so CALLVALUE takes 8 and stores in stack, JUMP reads from stack the location it has to jump, it is 8.

############
## Puzzle 2 #
############
00      34      CALLVALUE
01      38      CODESIZE
02      03      SUB
03      56      JUMP
04      FD      REVERT
05      FD      REVERT
06      5B      JUMPDEST
07      00      STOP
08      FD      REVERT
09      FD      REVERT
### Solution: 
1) CALLVALUE is added to the stack first, then comes CODESIZE. Then next opcode is SUB, which takes top - (top-1), The output of SUB is the location to JUMP.
in this case CODESIZE = 10 , so if JUMP has to work successfully, it should take 06 value, so it can JUMP to JUMPDeST. so pass 4 as CALLVALUE which equals 10-4=6 , JUMP takes 6 from stack and moves to JUMPDEST.
