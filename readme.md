# Lines 18

Code written by [tado-mi](https://github.com/tado-mi), in summer of 2018.<br/><br/>

Included is a set of Java source code files for a game heavily inspired by the classical [Lines98](https://www.lines98.com/).<br/><br/>

The game is a 9 x 9 grid, containing balls of 5 different colors. Horizontal, vertical or diagonal alignment of 5 or more balls of the same color results in them disappearing, and the user's score is incremented. There are 3 balls randomly generated and added to the grid after each move that does not result in a hit. Objective of the game is to gain as higher score as possible before the grid fills up with balls and no more movement is possible.<br/><br/>

The user is allowed to move balls if there is a path from a ball's current location to the desired location, and the desired location is empty.<br/><br/>

Some of the supported features include:

* viewing the locations and / or the color of the 'future' balls
* undo the last user move, acheived by maintaing (1) an array of two Points, representing the last user's latest move and switching it back.
- an array of BallPaths representing the balls that disappeared in the result of the previous move (requested from the GUI)

#### user control not listed on GUI:

- move balls around with the mouse

- increment/decrement the visual size of the grid by pressing **+** / **-**

- move the grid with a/w/s/d controls

# Files:

## BallPath.java:

Essentially an ArrayList of Points that represent the location of a ball on the grid. Maintains an int indicating the color which all balls share

Hardcoded to aid the implementation of the *Highlighter.java*

## Highlighter.java:

Essentially an ArrayList of BallBaths. Maintains a *java* ArrayList of Colors, corresponding to the Colors of the game

### non-trivial methods:

#### draw(Graphics g, int xZero, int yZero, int gridSize):

visually highlights all the BallPaths with their Color

- initialises a size of a side of the square, proportional to the provided gridSize
- locates the upper leftmost corner of each rectangle as (xZero + x * gridSize, yZero + y * gridSize), where xZero, yZero and gridSize are provided parameters, and x, y are taken from BallPaths
- draws the square

repeats for all the BallPaths

## HighScoreList.java:

Creates an external .txt file that maintains 6 of the highest scores with respect to a device in decreasing order

Each time Lines18 is called, HighScoreList reads from the external file to create an array of usernames and their correpsonding high scores. If the external file is absent, it creates an empty file to store the high scores as they generate during the game.

### non-trivial methods:

#### update(int score, String username):

if the offered score is greater than the lowest recorded score, or there is available capacity, the offered score is recorded st the array remains sorted

the updated arrays are recorded in the external .txt file at the end

## Lines18.java:

the core class. Implements a MouseListener and a KeyListener

### non-trivial methods:

described in a sequance that reflects how a typical user experience would invoce them

#### init():

called by the constructor

- initialises the GUI

- calls a method to initialise the grid

#### initGrid():

- intitialies a 2D int arr and ihabits it with randomly generated balls

- generates the future locations and colors as well, reflective of where balls would be added after user's move

#### mouseClicked(MouseEvent e):

- return if the click happened outside the grid

- otherwise

- if selected cell is occupied with a ball updates the global current (: selected) ball coordinates (xCurr, yCurr)

- otherwise

- if the current ball coordinates are non-negative and there is a path from the current ball location to the clicked location, and moves the selected ball there by updating the grid

#### isReachable(int x, int y):

treats the grid as a graph, with each cell corresponding to a vertex, connected to its horizontal and vertical (i.e. non-diagonal) neighbors if they do not contain a ball

performs a [DFS](https://en.wikipedia.org/wiki/Depth-first_search) rooted at the vertex corresponding to the current (: selected) ball and reports if the vertex corresponding to the provided (x, y) cell is reachable or not.

#### isFutureConflict():

reports whether or not the cell corresponding to current (: selected) ball would have been occupied by a new ball (to be added after the move)

#### futureConflict():

regenereates random locations for balls to be added to so that there is no conflict between a prohected location

#### updateScore():

reports whether or not the user's move has resulted in one or more alignments

if so, erases the balls and increments the score by the number of erased balls

#### addBalls(int n):

- adds the projected future balls to the grid and calls updateScore for each of them.

- calls SetFuture(n) to prepare for the next call

#### setFuture(int n):

updates the global array of locations and colors of future balls by randomly generating and ensuring that there are no conflicts with existing allignments

# Run

For more information on the magic of makefiles, see [here](https://www.cs.swarthmore.edu/~newhall/unixhelp/javamakefiles.html). For compiling, hit on the terminal `make`. To run, run `make run`.
