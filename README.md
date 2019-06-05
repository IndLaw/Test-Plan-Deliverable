# 1632-Deliverable-1

## Test Plan for Super Connect Four

In developing the test plan for Super Connect Four, the initial focus was on cases where the game is started, and so created tests that checked initialization. From there, I focused on input validation tasks like the parsing of command line arguments. I then tested game functionality, including the flip and rotate commands, placement of the checkers, and generation of the game board. Further, I also checked in-game rules and ensured that the players’ checker pieces interacted correctly with each other and the board.

I identified several edge cases where multiple conditions need to come together in order to verify a requirement. One example of such a case is a combination of requirements two and eight. Requirement two states that if a valid integer x is entered, the game shall consist of a game board of x rows and x columns, and requirement eight states that the first player to make a "connect four" shall be the winner. In a test that combines these two requirements, starting the game with valid integer “1” makes it impossible to achieve a win state and results in a tie. I also considered more complicated edge cases, such as when one player enters the flip command that results in a connect four with the other player’s checkers. In this case, the test needed to provide a game board pre-set with checkers for reproducibility. My last edge case was similar, it verifies that requirements 7 and 8 execute correctly when they occur at the same time.

Testing uncovered several defects with the game. As expected, most of the issues stemmed from input. There were validation issues with input from the command line and user input. Upper right to lower left diagonal win conditions were not detected by the game. Additionally, a logic-related error was uncovered where entering ‘Rot’ would result in a checker being inserted into the first column. The test cases proved to be effective in identifying these defects. The corrected defects are Unexpected tie, Full column number are being displayed, and Unexpected exception from rotate command.


## Test Cases

IDENTIFIER: NO_INIT_INPUT
DESCRIPTION: The game will fail to initiate when no user input is given.
PRECONDITIONS: The game is not running.
EXECUTION STEPS: Run ‘ruby connect_four.rb’ in terminal.
POSTCONDITIONS: Game will present a message explaining how the game should be initialized.

IDENTIFIER: NEGATIVE_INIT_INPUT
DESCRIPTION: The game will fail to initiate when a negative input is given.
PRECONDITIONS: The game is not running.
EXECUTION STEPS: Run ‘ruby connect_four.rb -5’ in terminal.
POSTCONDITIONS: Game will present a message explaining how the game should be initialized.

IDENTIFIER: STRING_INIT_INPUT
DESCRIPTION: The game will fail to initiate when a string input is given.
PRECONDITIONS: The game is not running.
EXECUTION STEPS: Run ‘ruby connect_four.rb ABC’ in terminal.
POSTCONDITIONS: Game will present a message explaining how the game should be initialized.

IDENTIFIER: POS_INIT_INPUT
DESCRIPTION: The game will initialize when a positive integer input is given.
PRECONDITIONS: The game is not running.
EXECUTION STEPS: Run ‘ruby connect_four.rb 5’ in terminal.
POSTCONDITIONS: Game will start.

IDENTIFIER: MINIMUM_VALID_INTEGER
DESCRIPTION: Initialize the game with the smallest positive signed 32-bit integer. 
PRECONDITIONS: The game has not been started.
EXECUTION STEPS: Begin the game with an argument of ‘1’.
POSTCONDITIONS: The argument is accepted without error and the program begins to run.

IDENTIFIER: MAXIMUM_VALID_INTEGER
DESCRIPTION: Initialize the game with the largest positive signed 32-bit integer. 
PRECONDITIONS: The game has not been started.
EXECUTION STEPS: Begin the game with an argument of ‘2147483647’.
POSTCONDITIONS:The argument is accepted without error and the program begins to run.

IDENTIFIER: X_STARTS
DESCRIPTION: Player X has the first turn.
PRECONDITIONS: Game is not running.
EXECUTION STEPS: Enter ‘ruby connect_four.rb 5’ in terminal.
POSTCONDITIONS: Last printed message should be asking for Player X’s input.

IDENTIFIER: O_FOLLOWS_X
DESCRIPTION: Player O goes after X.
PRECONDITIONS: Game has just started and X has not played.
EXECUTION STEPS: Enter ‘0’.
POSTCONDITIONS: It is now O’s turn.

IDENTIFIER: X_FOLLOWS_O
DESCRIPTION: Player X goes after O.
PRECONDITIONS: X has played once in the game.
EXECUTION STEPS: Enter ‘0’.
POSTCONDITIONS: It is now X’s turn.

IDENTIFIER: COLUMN_INPUT_VALID
DESCRIPTION: Entering a valid column number causes the program to insert a checker at the chosen column and switch to the next player.
PRECONDITIONS: The game has started with an argument of 4 and it is Player X’s turn.
EXECUTION STEPS: Enter ‘1’.
POSTCONDITIONS: An ‘x’ checker is inserted into column 1 and Player O’s turn begins.

IDENTIFIER: COLUMN_INPUT_INVALID
DESCRIPTION: Entering an invalid column number causes the program to display an error message and continue the game without changing players.
PRECONDITIONS: The game has started with an argument of 4 and it is Player X’s turn.
EXECUTION STEPS: Enter ‘4’.
POSTCONDITIONS: An ‘x’ checker is not inserted, an error message is displayed, and Player X’s turn has restarted.

IDENTIFIER: FLIP_COMMAND
DESCRIPTION: During each turn, users can enter 'flip' to flip the game board. This command is accepted by the game and is case-insensitive. 
PRECONDITIONS: Ensure the game has started.
EXECUTION STEPS: Enter the command ‘flip,’ without regard to case.
POSTCONDITIONS: The command is accepted without error.

IDENTIFIER: ROTATE_COMMAND
DESCRIPTION: During each turn, users can enter 'rot' to rotate the game board. This command is case-insensitive. 
PRECONDITIONS: Ensure the game has started with an argument of ‘5’ and that player input has resulted in this printed game board:
````
01234
.....
.....
.....
.....
XOXOX
````
EXECUTION STEPS: Enter the command ‘rot,’ without regard to case.
POSTCONDITIONS: The output after this command should appear as:
````
01234
X....
O....
X....
O....
X....
````

IDENTIFIER: X_PLACES_PIECE
DESCRIPTION: After player X enters a valid column, an X appears in the lowest empty space of the corresponding column.
PRECONDITIONS: It is X’s turn, the board size is 5 and the board looks like this:
````
01234
.....
.....
.....
.....
XO...
````
EXECUTION STEPS: Enter ‘0’.
POSTCONDITIONS: The board looks like this:
````
01234
.....
.....
.....
X....
XO...
````

IDENTIFIER: O_PLACES_PIECE
DESCRIPTION: After player O enters a valid column, an O appears in the lowest empty space of the corresponding column.
PRECONDITIONS: It is X’s turn, the board size is 5 and the board looks like this:
````
01234
.....
.....
....X
....O
XOXOX
````
EXECUTION STEPS: Enter ‘4’.
POSTCONDITIONS: The board looks like this:
````
01234
.....
....O
....X
....O
XOXOX
````

IDENTIFIER: FULL_COLUMN_INVALID
DESCRIPTION: A player cannot place a piece in a full column
PRECONDITIONS: It is O’s turn, the board size is 5 and the board looks like this:
````
01234
X....
O....
X....
O....
X....
````
EXECUTION STEPS: Enter ‘0’.
POSTCONDITIONS: Invalid move error message is displayed and the board looks like this:
````
01234
X....
O....
X....
O....
X....
````

IDENTIFIER: FLIP_FUNCTIONALITY
DESCRIPTION: During each turn, users can enter 'flip' to flip the game board. This command is case-insensitive. 
PRECONDITIONS: Ensure the game has started with an argument of ‘5’ and that player input has resulted in this printed game board:
````
01234
.....
.....
.....
OXOXO
XOXOX
````
EXECUTION STEPS: Enter the command ‘flip.’
POSTCONDITIONS: The output after this command should appear as:
````
01234
.....
.....
.....
XOXOX
OXOXO
````

IDENTIFIER: FLIP_WIN
DESCRIPTION:  A player can flip the board and trigger a win by creating a connect four.
PRECONDITIONS: It is X’s turn, the board has a size of 5 and appears as:
````
01234
.....
.....
..X..
X.OX.
OXOOX
````
EXECUTION STEPS: Enter ‘flip’.
POSTCONDITIONS: Player X wins and the board appears as:
````
01234
.....
.....
..O..
O.OO.
XXXX.
````

IDENTIFIER: FLIP_OTHER_PIECE_WIN
DESCRIPTION: A player can flip the board and trigger a win by connecting the other player’s pieces.
PRECONDITIONS: It is X’s turn, the board has a size of 5 and appears as:
````
01234
.....
.....
..O..
O.XO.
XOXXO
````
EXECUTION STEPS: Enter ‘flip’.
POSTCONDITIONS: Player X wins and the board appears as:
````
01234
.....
.....
..X..
X.XX.
OOOO.
````

IDENTIFIER: ROT_NO_WIN_FALL
DESCRIPTION: A player can rotate the board and it does not cause a win or pieces to fall.
PRECONDITIONS: It is O’s turn, the board size is 5 and the board looks like this:
````
01234
....X
...OO
..XXX
.XOOO
XOXOX
````
EXECUTION STEPS: Enter ‘rot’.
POSTCONDITIONS: It is X’s turn and the board looks like this:
````
01234
X....
OX...
XOX..
OOXO.
XOXOX
````

IDENTIFIER: ROT_FALL_NO_WIN
DESCRIPTION: A player can rotate the board and it causes pieces to fall but does not cause a win.
PRECONDITIONS: It is X’s turn, the board size is 5 and the board looks like this:
````
01234
X....
OX...
XOX..
OOXO.
XOXOX
````
EXECUTION STEPS: Enter ‘rot’.
POSTCONDITIONS: It is O’s turn and the board looks like this:
````
01234
X....
OO...
XOX..
OXOO.
XOXXX
````

IDENTIFIER: ROT_FALL_WIN
DESCRIPTION: A player can rotate the board and it causes pieces to fall creating a connect four.
PRECONDITIONS: It is X’s turn, the board size is 5 and the board looks like this:
````
01234
X....
OX...
OXX..
OOX..
XOOOX
````
EXECUTION STEPS: Enter ‘rot’.
POSTCONDITIONS: The game is over with a X win and the board looks like this:
````
01234
X....
O....
OOO..
OOXO.
XXXXX
````

IDENTIFIER: ROT_OTHER_PIECE_WIN
DESCRIPTION: A player can rotate the board and it causes a win by connecting the other player’s pieces.
PRECONDITIONS: It is X’s turn, the board size is 5 and the board looks like this:
````
01234
O....
OX...
XOX..
XOX..
XOXOO
````
EXECUTION STEPS: Enter ‘rot’.
POSTCONDITIONS: The game is over with a X win and the board looks like this:
````
01234
X....
O....
XXX..
OOOO.
OXXXO
````

IDENTIFIER: HORZONTAL_WIN_CONDITION
DESCRIPTION: A player with four checkers of the same type touching each other in a single file horizontally shall be the winner. The game shall indicate which player has won the game and exit.
PRECONDITIONS: The game has been initialized with an argument of 5, it is Player X’s turn, and the game board appears as:
````
01234
.....
X....
OO...
XXX..
XOOO.
````
EXECUTION STEPS: Enter ‘3’.
POSTCONDITIONS: The game will indicate that Player X has won and the program will exit.

IDENTIFIER: VERTICAL_WIN_CONDITION
DESCRIPTION: A player with four checkers of the same type touching each other in a single file vertically shall be the winner. The game shall indicate which player has won the game and exit.
PRECONDITIONS: The game has been initialized with an argument of 5, it is Player X’s turn, and the game board appears as:
````
01234
.....
.....
X....
X..O.
XOOXO
````
EXECUTION STEPS: Enter ‘0’.
POSTCONDITIONS: The game will indicate that Player X has won and the program will exit.

IDENTIFIER: DIAGONAL_WIN_CONDITION
DESCRIPTION: A player with four checkers of the same type touching each other in a single file diagonally shall be the winner. The game shall indicate which player has won the game and exit.
PRECONDITIONS: The game has been initialized with an argument of 5, it is Player X’s turn, and the game board appears as:
````
01234
.....
.....
.OX..
.OXX.
XOOOX
````
EXECUTION STEPS: Enter ‘1’.
POSTCONDITIONS: The game will indicate that Player X has won and the program will exit.

IDENTIFIER: COL_NUM_LSD
DESCRIPTION: Displayed column number should always be the least significant digit.
PRECONDITIONS: Game has not started yet.
EXECUTION STEPS: Enter ‘ruby connect_four.rb 25’.
POSTCONDITIONS: The game starts and the header for the board should look like this:
````
0123456789012345678901234
````

IDENTIFIER: NEED_FULL_COL_NUM
DESCRIPTION: Full column number needs to be used to put piece in desired column.
PRECONDITIONS: Game board is empty with size 25 and it is player X’s turn.
EXECUTION STEPS: Enter ‘23’.
POSTCONDITIONS: The board looks like this:
````
0123456789012345678901234
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.......................X.
````

IDENTIFIER: SINGLE_DIG_INPUT
DESCRIPTION: Full column number needs to be used to put piece in desired column.
PRECONDITIONS: Game board is empty with size 25 and it is player X’s turn.
EXECUTION STEPS: Enter ‘3’.
POSTCONDITIONS: The board looks like this:
````
0123456789012345678901234
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
.........................
...X.....................
````

##Traceability Matrix

Requirement #: List of test cases
1:    NO_INIT_INPUT, STRING_INIT_INPUT, NEGATIVE_INIT_INPUT, POS_INIT_INPUT
2:    MINIMUM_VALID_INTEGER, MAXIMUM_VALID_INTEGER
3:    X_STARTS, O_FOLLOWS_X, X_FOLLOWS_O
4:    FLIP_COMMAND, ROTATE_COMMAND, COLUMN_INPUT_VALID, COLUMN_INPUT_INVALID
5:    X_PLACES_PIECE, O_PLACES_PIECE, FULL_COLUMN_INVALID
6:    FLIP_FUNCTIONALITY, FLIP_WIN_CONDITION_PLAYER, FLIP_WIN_CONDITION_OPPONENT
7:    ROT_NO_WIN_FALL, ROT_FALL_NO_WIN, ROT_FALL_WIN, ROT_OTHER_PIECE_WIN
8:    HORZONTAL_WIN_CONDITION, VERTICAL_WIN_CONDITION, DIAGONAL_WIN_CONDITION
9:    COL_NUM_LSD, NEED_FULL_COL_NUM, SINGLE_DIG_INPUT

##Defects

SUMMARY: Unexpected exception from rotate command.
DESCRIPTION: When attempting to rotate the game board, the input ‘Rot’ results in a NumberFormatException.
REPRODUCTION STEPS: Ensure the game has started with an argument of ‘4’ and that player input has resulted in this printed game board:
````
0123
....
....
....
OXOX
````
When prompted for a command, enter ‘Rot’
EXPECTED BEHAVIOR: The game board rotates, resulting in:
````
0123
O...
X...
O...
X...
````
OBSERVED BEHAVIOR: The program instead throws an exception:
````
Exception in thread "main" java.lang.NumberFormatException: For input string: "Rot"
        at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
        at java.lang.Integer.parseInt(Integer.java:580)
        at java.lang.Integer.parseInt(Integer.java:615)
        at Game.makeMove(Game.java:295)
        at Game.play(Game.java:43)
        at ConnectFour.main(ConnectFour.java:13)
 ````


SUMMARY: Unexpected tie.
DESCRIPTION: If Super Connect Four is initialized with a ‘1’ and Player X enters a move, the program will end in a tie.
REPRODUCTION STEPS: Run connect_four.rb with the argument ‘1.’
EXPECTED BEHAVIOR: One player will not be able to instigate a tie within the first move.
OBSERVED BEHAVIOR: Game gives the output ‘It’s a tie” and the program terminates.

SUMMARY: Full column number are being displayed.
DESCRIPTION: Full column numbers are being displayed but only the least significant digit should be displayed. Found in test case COL_NUM_LSD.
REPRODUCTION STEPS: Initiate the game using ‘java ConnectFour 25’. 
EXPECTED BEHAVIOR: Board header should be:
0123456789012345678901234
OBSERVED BEHAVIOR: Board header is:
````
0123456789101112131415161718192021222324
````

SUMMARY: Upper right to lower left diagonal win not detected.
DESCRIPTION: Upper right to lower left diagonal win not detected while upper left to lower right diagonal is detected.
REPRODUCTION STEPS: Play game board with size 5 and looks like this:
````
01234
.....
.....
.OXX.
.XOO.
XOXO.
````
Have it be X’s turn and enter 3.
EXPECTED BEHAVIOR:
````
01234
.....
...X.
.OXX.
.XOO.
XOXO.
Player X won!
````
OBSERVED BEHAVIOR: 
````
01234
.....
...X.
.OXX.
.XOO.
XOXO.
Player O, enter move
````
