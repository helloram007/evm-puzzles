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
1) CALLVALUE is added to the stack first, then comes CODESIZE. Then next opcode is SUB, which takes top - (top-1), The output of SUB is the location to JUMPDEST as JUMP instruction needs to offset to jump to JUMPDEST.
in this case CODESIZE = 10 , so if JUMP has to work successfully, it should take 06 value, so it can JUMP to JUMPDeST. so pass 4 as CALLVALUE which equals 10-4=6 , JUMP takes 6 from stack and moves to JUMPDEST.
############
# Puzzle 3 #
############
00      36      CALLDATASIZE
01      56      JUMP
02      FD      REVERT
03      FD      REVERT
04      5B      JUMPDEST
05      00      STOP

? Enter the calldata: 0xFFFFFFFF

Puzzle solved!
### Solution:
In This case, we need to send CALLDATA. the first opcode computes its length(in bytes).So if we are sending in "0xff", that is 1 byte and CALLDATASIZE is 1. but the JUMP instruction(second opcode), needs to jump to the JUMPDEST(at location 4 in memory). So we need to pass in CALLDATA that has size 4 bytes. e.g. 0xffffffff.

############
# Puzzle 4 #
############
00      34      CALLVALUE
01      38      CODESIZE
02      18      XOR
03      56      JUMP
04      FD      REVERT
05      FD      REVERT
06      FD      REVERT
07      FD      REVERT
08      FD      REVERT
09      FD      REVERT
0A      5B      JUMPDEST
0B      00      STOP

############
# Puzzle 4 #
############

00      34      CALLVALUE
01      38      CODESIZE
02      18      XOR
03      56      JUMP
04      FD      REVERT
05      FD      REVERT
06      FD      REVERT
07      FD      REVERT
08      FD      REVERT
09      FD      REVERT
0A      5B      JUMPDEST
0B      00      STOP

? Enter the value to send: 6

Puzzle solved!

Run it in evm.codes: https://www.evm.codes/playground?callValue=6&unit=Wei&callData=&codeType=Bytecode&code=%2734381856FDFDFDFDFDFD5B00%27_
### Solution:
We Need to pass a value(CALLVALUE), so that the JUMP instruction can jump to JUMPDEST at 0xA location.
CODESIZE of the above table is 0xC, so if we XOR 0xC with 0xA(location of JUMPDEST, as XOR is reversed ). so using the xor.pw calculator. 
i do the below math
0xC ^ 0xA = 6.
so reversing this with 0xC ^ 6 will be equal to 0xA, which is the JUMPDEST.

############
# Puzzle 5 #
############
00      34          CALLVALUE
01      80          DUP1
02      02          MUL
03      610100      PUSH2 0100
06      14          EQ
07      600C        PUSH1 0C
09      57          JUMPI
0A      FD          REVERT
0B      FD          REVERT
0C      5B          JUMPDEST
0D      00          STOP
0E      FD          REVERT
0F      FD          REVERT

? Enter the value to send: 16

Puzzle solved!

Run it in evm.codes: https://www.evm.codes/playground?callValue=16&unit=Wei&callData=&codeType=Bytecode&code=%2734800261010014600C57FDFD5B00FDFD%27_
### Solution:
In this Instruction set, pass a value 16 (CALLVALUE), it then duplicates the value in the stack , so now stack contains 16 and 16. multiply(MUL) and then 0x100 is pushed into the stack, which then executes EQ.if it is successful, then executes PUSH1 0c, which is later used by JUMPI to jump to JUMPDEST.
############
# Puzzle 6 #
############

00      6000      PUSH1 00
02      35        CALLDATALOAD
03      56        JUMP
04      FD        REVERT
05      FD        REVERT
06      FD        REVERT
07      FD        REVERT
08      FD        REVERT
09      FD        REVERT
0A      5B        JUMPDEST
0B      00        STOP

? Enter the calldata: 0x000000000000000000000000000000000000000000000000000000000000000A

Puzzle solved!

Run it in evm.codes: https://www.evm.codes/playground?callValue=0&unit=Wei&callData=0x000000000000000000000000000000000000000000000000000000000000000A&codeType=Bytecode&code=%2760003556FDFDFDFDFDFD5B00%27_

### Solution:
CALLDATALOAD needs to enter as 0xA but in full 32 bytes(0x000000000000000000000000000000000000000000000000000000000000000A).
So it is loaded as "a" in the stack and jump takes it and moves to the location.

############
# Puzzle 7 #
############

00      36        CALLDATASIZE
01      6000      PUSH1 00
03      80        DUP1
04      37        CALLDATACOPY
05      36        CALLDATASIZE
06      6000      PUSH1 00
08      6000      PUSH1 00
0A      F0        CREATE
0B      3B        EXTCODESIZE
0C      6001      PUSH1 01
0E      14        EQ
0F      6013      PUSH1 13
11      57        JUMPI
12      FD        REVERT
13      5B        JUMPDEST
14      00        STOP

? Enter the calldata: 0x60016000f3

Puzzle solved!

Run it in evm.codes: https://www.evm.codes/playground?callValue=0&unit=Wei&callData=0x60016000f3&codeType=Bytecode&code=%2736600080373660006000F03B600114601357FD5B00%27_

### Solution:
In this case, we need to send calldatavalue, so the created contract returns something which is of 1 byte. so when the EXTCODESIZE returns 1 which in the next step equals to 1 from push. that will be stored as an offset for JUMPI.
answer is "0x60016000f3"
