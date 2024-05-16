# mm-q

To describe the Mastermind game using a system of qubits and define the game states and state vectors, we need to represent the sequence of colored pins, the guesses, and the feedback within the quantum framework.

# 2.Estimate the complexity of the move search in different stages of the game

To estimate the complexity, I will split it  into steps.
Fisrt guesser says an option, it take contant time to calculate the score for the keeper.
Then the guesser uses this score to caluclate the scores for all 2^4 = 16 numbers.
Since in future steps the number of remaining candidates is going to decrease we don't need the complexity of the rest operations, we will take the highest order. => 2^4, or in general case it is 2^n.

If we calculate for general case n

In First Stage

At the beginning, the guesser knows nothing about the sequence. The search space consists of all possible sequences of n pins with n colors, => n^n
Then each guess needs to be compared with the hidden sequence, which is O(n) in terms of time complexity since each pin needs to be checked.
Initially, the complexity is  O(n6n) for the search space and O(n) for evaluating each guess.

In the Middle Stage
After the first guess, the feedback helps reduce the search space. For instance, if the feedback is k , the guesser knows that k pins are correct and in the right position.
The search space is now reduced to combinations that fit the given feedback. The size of the search space reduction is not linear but depends on the feedback provided.
If the search space reduces significantly, say to m possible sequences, the complexity becomes  O(mn) for generating and evaluating each new guess.

Int the Final Stage
As the game progresses and more feedback is accumulated, the guesser has significantly narrowed down the possible sequences.
In the final stages, the number of possible sequences is very small, and the guesser is almost certain of the positions and colors.
The complexity in the end game is much lower, possibly O(1) if the guesser is confident of the correct sequence, making only one final guess to confirm.

# Task 2

### 1. Initializing the Superposition State

For 4 positions and 2 colors (0 and 1), we can use Hadamard gates to create a superposition of all possible guesses. Each position can be represented by one qubit.

The initial state of the guess register |g> can be created by applying the Hadamard gate (H) to each qubit:
![image](https://github.com/tatevvvv/mm-q/assets/54271006/33ab866a-184a-4541-ae8d-8059762a6a0f)

This results in the superposition:

![image](https://github.com/tatevvvv/mm-q/assets/54271006/74808089-5bef-4c67-9d3c-a7bc016a28bf)

### 2. Secret Sequence Representation

The secret sequence |s> is fixed and encoded in the circuit. For simplicity, let's assume the secret sequence is |s> = |s1s2s2s4>, where each |si> is a qubit.

### 3. Applying the Quantum Feedback Function
We need a unitary operator U(f) Unitary that computes the feedback based on the guess and the secret sequence and stores it in ancillary qubits.

1. **Initialization of Feedback Register**:
   The feedback register is initialized to |0>

2. **Quantum Feedback Calculation**:
   We use CNOT gates to compare each guess qubit with the corresponding secret qubit and Toffoli gates to update the feedback register.

### 4. Oracle Marking and Amplification

Grover's algorithm is used to amplify the probability amplitude of the correct guess states.

1. **Oracle Marking**: We apply a phase flip to the states where the feedback register indicates all correct positions.
2. **Diffusion Operator**: We apply the diffusion operator to amplify the marked states.

### Full Quantum Circuit Example

Let's design the quantum circuit for the specific case with 4 pins and 2 colors (0 and 1).

#### Step 1: Initialize Superposition

Apply Hadamard gates to all guess qubits.

Circuit:

```plaintext
|0> --H--| g1 |
|0> --H--| g2 |
|0> --H--| g3 |
|0> --H--| g4 |
```

#### Step 2: Encode Secret Sequence

Assume the secret sequence is |s> = |1010> This is fixed and encoded in the circuit.

#### Step 3: Quantum Feedback Function U(f)

The feedback function compares the guess with the secret and updates the feedback register. We'll use controlled operations to do this.

Circuit:

```plaintext
|s1> --•-----•-----|   |   |   |   | 
       |     |     |   |   |   |   |
|g1> --X--•  |     |   |   |   |   |
         |  |     |   |   |   |   |
|s2> --•--X--•-----•--•--•--|--•--| 
       |     |     |  |  |  |  |  |
|g2> --X-----X--•  |  |  |  |  |  |
         |     |  |  |  |  |  |  |
|s3> --•-----•--X--•--|--|--|--•--| 
       |     |     |  |  |  |  |  |
|g3> --X--•  |     |  |  |  |  |  |
         |  |     |  |  |  |  |  |
|s4> --•--X--•-----•--•--|--|--•--| 
       |     |     |  |  |  |  |  |
|g4> --X-----X-----|--|--|--|--X--|
                   |  |  |  |     |
|0> ---| CCNOT |---|--|--|--|-----|
|0> ---| CCNOT |---|--|--|--|-----|
|0> ---| CCNOT |---|--|--|--|-----|
|0> ---| CCNOT |---|--|--|--|-----|
```

#### Step 4: Oracle Marking and Diffusion

Apply the oracle marking and diffusion operator from Grover's algorithm.

Circuit:

```plaintext
|g1> --| O_f |--| D |
|g2> --| O_f |--| D |
|g3> --| O_f |--| D |
|g4> --| O_f |--| D |
```

### Conclusion

By following these steps, we can parallelize the move search in Mastermind using a quantum circuit. The circuit initializes the guess register in a superposition state, applies the feedback function using controlled operations, and uses Grover's algorithm to amplify the correct guesses. This approach leverages quantum parallelism to evaluate multiple guesses simultaneously, providing a significant speedup over classical methods.
