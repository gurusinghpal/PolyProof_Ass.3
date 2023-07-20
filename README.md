# ZARDKAT

Logical Gate Circom Circuit with ZK-SNARK Proof

## Description

This project implements a logical gate circuit using the circom programming language. The circuit implements the following truth table:

```
A   B   X   Y   Q
0   0   0   1   1
0   1   0   0   0
1   0   0   1   1
1   1   1   0   1
```
The goal of the project is to prove that you know the inputs A=0 and B=1 that yield a 0 output. 

## Prerequisites

Before proceeding, make sure you have the following installed on your system:

Node.js: [https://nodejs.org]
Circom: [https://github.com/iden3/circom]
SnarkJS: [https://github.com/iden3/snarkjs]
A test wallet with some Mumbai Faucet (MATIC) for contract deployment.

## Getting Started

### Installing

To get started with the project, follow these steps:

1. clone the circom repository :
    `git clone [https://github.com/gmchad/zardkat]`
   
2. Install the required dependencies :
   `npm i`

### Executing program

```
pragma circom 2.0.0;

/*This circuit template checks that c is the multiplication of a and b.*/  

template MyCircuit () {  
    // signal inputs
        signal input a;
        signal input b;

    // signals from gates
        signal x;
        signal y;

    // final signal output
        signal output q;

    // component gates used to create custom circuit
        component andGate = AND();
        component notGate = NOT();
        component orGate = OR();

    // circuit logic
        andGate.a <== a;
        andGate.b <== b;
        notGate.in <== b;
        x <== andGate.out;
        y <== notGate.out;

        orGate.a <== x;
        orGate.b <== y;
        q <== orGate.out;


}

template AND() {
    signal input a;
    signal input b;
    signal output out;

    out <== a*b;
}

template NOT() {
    signal input in;
    signal output out;

    out <== 1 + in - 2*in;
}

template OR() {
    signal input a;
    signal input b;
    signal output out;

    out <== a + b - a*b;
}

component main = MyCircuit();

```

`npx hardhat circom` 
This will generate the **out** file with circuit intermediaries and geneate the **MyCircuitVerifier.sol** contract

`npx hardhat run scripts/deploy.ts --network mumbai`
This script does 4 things  
1. Deploys the MyCircuitVerifier.sol contract at mumbai network
2. Generates a proof from circuit intermediaries with `generateProof()`
3. Generates calldata with `generateCallData()`
4. Calls `verifyProof()` on the verifier contract with calldata

## Author

[@gurusinghpal](https://www.linkedin.com/in/guru-singh-pal-99a305254/)


## License

This code is released under the MIT License. Feel free to use, modify, and distribute it as per the terms of the license.
