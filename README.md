# mm-q

First I want to mention that I used this paper as a refernce to understand some details and possible ways to go with.

(https://arxiv.org/abs/2207.09356)

# 2.Estimate the complexity of the move search in different stages of the game

To estimate the complexity..
Fisrt guesser says an option, it takes constant time to calculate the score for the keeper.
Then the guesser uses this score to caluclate the scores for all 2^4 = 16 numbers.
Since in future steps the number of remaining candidates is going to decrease we don't need the complexity of the rest operations, we will take the highest order. => 2^4, or in general case it is 2^n.

If we calculate for general case n for all intermediate stages as well

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
```

### Conclusion

By following these steps, we can parallelize the move search in Mastermind using a quantum circuit. The circuit initializes the guess register in a superposition state, applies the feedback function using controlled operations, and uses Grover's algorithm to amplify the correct guesses.
