The Subtraction Game
====================

Beginning with a pile of 21 stones, players alternate removing
stones until none are left. On her turn, a player may take 1, 2,
or 3 stones. The player who takes the last stone is the winner.

A version of this game was played on an episode of Survivor,
where they called it Thai 21.


Steps:

0. Play a few games with your partner.

1. Finish implementing the minimax algorithm using the file I
   sent you as a starting point. You’ll need to fill in the
   stopping condition, the loop for the max player, and all
   the code for the min player.

2. Play a few rounds against the computer with a low depth
   limit, like 2. Can you win?

3. Now increase the depth limit to 5 and then to 10. How does
   the computer’s play change?

4. What’s the winning strategy for this subtraction game?

5. Now increase the depth limit to 20. What happens?

6. Modify your code to use alpha-beta pruning instead of the
   basic minimax algorithm. Verify that this allows efficient
   play even with higher depth limits.
