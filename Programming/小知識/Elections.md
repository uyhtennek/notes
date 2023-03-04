In the UK: it uses [General elections](https://www.parliament.uk/about/how/elections-and-voting/general/)
> The Prime Minister (總理) is officially appointed by the monarch (英女皇), who generally chooses the leader of the political party that wins the most seats in the House of Commons (英國下議院).

In the US: it uses [Electoral College](https://www.archives.gov/electoral-college/about)
> Citizens vote on how each state should allocate Electors (選舉人; 有選舉權的人) who then elect the President (總統)
___

Perhaps the simplest way to hold an election, though, is *plurality vote*
> Every voter gets to vote for one candidate. At the end of the election, whichever candidate has the greatest number of votes is declared the winner of the election.
___

but *plurality vote* allows a *tie* outcome
there is a *ranked-choice voting system*[^1] that is less likely to have this problem
> In a ranked-choice system, voters can vote for more than one candidate. Instead of just voting for their top choice, they can rank the candidates in order of preference.

also this system reveals not just the candidate who get the most votes, but also the candidate that the majority choose (= the *Condorcet winner*[^2]). like:
```
4 voters ranked 'Charlie' as no. 1
but 5 voters ranked 'Alice' over 'Charlie'
although only 2 voters ranked 'Alice' as no. 1

if the election had been just 'Alice' and 'Bob', or 'Alice' and 'Charlie'
'Alice' would have won
```

[^1]: Tideman voting method (also known as "ranked pairs") is a ranked-choice voting method that's guaranteed to produce the Condorcet winner[^2] of the election if one exists

[^2]: Condorcet winner: the person who would have won any head-to-head matchup against another candidate

However it's possible to have cases like:
```
'Alice' wins over 'Bob'
'Bob' wins over 'Charlie'
'Charlie' wins over 'Alice'
```
how to declare a winner in this case?

1. Firstly we find the one who has the "biggest" victory (= get the most votes against another candidate). eg.
```
'Alice' wins over 'Bob' by a 7-2 margin
'Charlie' wins over 'Alice' by a 6-3 margin
'Bob' wins over 'Charlie' by a 5-4 margin

So 'Alice' has the "biggest" win in this case
```
2. Next, who is winning over *Alice*? *Charlie*.
3. Again, who is winning over *Charlie*? *Bob*.
    but *Bob* is already won over by *Alice*, it would be a cycle if we consider him again
4. So *Charlie* is the winner of this election.

Put more formally, the *Tideman voting method* consists of 3 parts:
* *Tally*
>For each pair of candidates, who the preferred candidate is and by what margin they are preferred.
* *Sort*
>Sort the pairs of candidates in decreasing order of the strength of victory (= number of voters who prefer the preferred candidate).
* Lock
>Starting with the strongest pair, go through the pairs of candidates in order and "lock in" each pair, so long as one of the candidate in the pair is not the one who has the strongest margin

Then the winner in the last pair is the winner of the election.
___
