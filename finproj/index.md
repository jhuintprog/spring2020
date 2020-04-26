---
layout: default
title: "Final project"
---

*Update 4/21* — Contributions to the public test repository will be considered for extra credit.  See the [Sharing unit tests](#sharing-unit-tests) section.

*Update 4/26* — Clarified that for `ChaseHero` and `AStarChaseHero`, "closest hero" means "closest hero as measured by the `distanceFrom` member function of the `Position` class"

**Due**: Monday, May 4th by 11pm

*Important*: It is likely that we will be updating this project description to add additional information, hints, and clarifications.

# Hero vs. Minotaur

In this project, your team will implement a maze game in which the Hero ("@") tries to escape and avoid being captured by the Minotaur ("M").

Here is a video showing the basic version of the game:

<center>
<video width="658" height="462" controls>
  <source src="media/demo3-cut.webm" type="video/webm">
Your browser does not support the video tag.
</video>
</center>

Here is a video showing a fancier version of the game using the [curses](#curses-driver-program) user interface library:

<center>
<video width="658" height="462" controls>
  <source src="media/demo4.webm" type="video/webm">
Your browser does not support the video tag.
</video>
</center>

<!--
Best of all, here is a [playable demo](play/tminotaur.html) of the curses UI.  The WASD keys move up/left/down/right.
-->

## Getting started

Create a bitbucket repository for your team, and make sure that all of the team members have write permission.  Make sure you create a *private* repository.  One team member should clone the bitbucket repository to create a local repository.

Pull from the public repository.  You will find a directory called `final-project/starter-code`.  Copy the files in this directory into the clone of your team repository (only one team member needs to do this).  Commit the starter code, and then push.  Now the other team members can clone the team repository to create their own local repositories.

Note that the starter code includes a `Makefile`.  You are allowed to make changes to it, but you might not need to.  The main requirement of the `Makefile` that you submit is that the following targets build correctly:

* `minotaur` (the [driver program](#driver-program))
* `positiontest`
* `tiletest`
* `mazetest`
* `gametest`

You may add additional targets as long as the above targets work.

Note that if you run the command `make depend`, the makefile will generate correct header file dependencies for each `.o` file. (Note that this target won't work unless all of the expected `.cpp` files exist.)

## Tasks

Your tasks are:

* Implement the specified classes
* Test the specified classes thoroughly, including unit testing
* Implement a driver program that allows the user to play the game

There is also one optional task:

* Implement a [curses](https://en.wikipedia.org/wiki/Curses_(programming_library)) user interface and driver program

There will be a *small* amount of extra credit for the optional curses UI. However, you shouldn't work on it until you have the required features working.

There will also be a small amount of extra credit for contributing tests to the public test repository. See the [Sharing unit tests](#sharing-unit-tests) section for details.

This may seem like a pretty complex project. Don't panic! The [recommended approach](#recommended-approach) section outlines an approach to getting started, making progress, and finishing the project.

### Classes to implement

You will need to implement the following classes:

Class             | Filename (.h and .cpp)
----------------- | ----------------------
`AStarChaseHero`  | `astarchasehero`
`BasicGameRules`  | `basicgamerules`
`ChaseHero`       | `chasehero`
`Entity`          | `entity`
`Floor`           | `floor`
`Game`            | `game`
`Goal`            | `goal`
`Inanimate`       | `inanimate`
`Maze`            | `maze`
`TextUI`          | `textui`
`UIControl`       | `uicontrol`
`Wall`            | `wall`

Each of the above classes is provided as just a header (`.h`) file. You will need to create the corresponding source file (`.cpp`).

**Important**: You must implement the public member functions *exactly* as specified.  Do not add any additional public member functions.  You may add fields and private member functions to any of the classes above.

Each public member function has a detailed comment describing what the public member function should do.  Additional details are below in the [hints and specifications](#hints-and-specifications) section.

The following classes are provided (you should not modify them):

Class                     | Filename (.h and .cpp)
------------------------- | ----------------------
`EntityControllerFactory` | `ecfactory`
`EntityController`        | `entitycontroller`
`GameRules`               | `gamerules`
`MysteryController`       | `mysterycontroller`
`MysteryTile`             | `mysterytile`
`Position`                | `position`
`ScriptedControl`         | `scriptedcontrol`
`TileFactory`             | `tilefactory`
`Tile`                    | `tile`
`UI`                      | `ui`

In order to develop a high-level understanding of all these classes and the interplay among them, [this](media/cd.jpg) UML class diagram might be helpful. Reading UML class diagrams is quite straightforward, see [here](https://www.guru99.com/uml-class-diagram.html).

### Unit testing

One challenge that arises in creating a complex program is that there are many components that need to work correctly in order for the overall program to work correctly.  In the case of a program written in an object-oriented language (like C++), these components are classes.  One important and useful way to make progress on the implementation of a complex program is to develop and test classes separately.  If the functionality of each class is tested thoroughly, you can have some confidence that the overall program will work correctly once all of the individual classes are combined.

*Unit testing* is a testing technique in which a software component (such as a class) is tested in isolation from the rest of the program.  In a unit test, the *test fixture* is a collection of objects (primarily instances of the class being tested.)  *Test functions* call member functions on the objects in the test fixture, and use *assertions* to verify that correct behavior was observed.  Unit test frameworks such as [JUnit](https://junit.org) can make it easy to write unit test programs.  For this project, we will use a very simple framework called "TCTest++", which is inspired by JUnit.

As an example of a unit test program, we have provided `positiontest.cpp`, which is a unit test program for the `Position` class.  We recommend that you read the code of this unit test program, since it demonstrates the essential features of a TCTest++ test program.  Here is a brief overview of `positiontest.cpp` (which is representative of TCTest++ unit test programs more generally):

* The `TestObjs` struct type defines the type of the test fixture.  For `positiontest.cpp`, this type has a constructor which initializes a number of `Position` objects.
* The `setup` function creates an instance of `TestObjs`, and (if necessary) creates instances of test objects. For `positiontest.cpp`, there's not much to do, since the `TestObjs` constructor does all of the initialization. For other unit test programs, the `setup` function might dynamically create the test objects, and store pointers to them in fields of a `TestObjs` instance.
* The `cleanup` function deletes an instance of `TestObjs`. Anything allocated in `setup` should be deallocated by `cleanup`.
* The declarations of the test functions (`testGetX`, `testGetY`) specify which unit test functions the unit test program contains. One way to organize your test functions is to dedicate one test function to each public member function of the class being tested, and this is what `positiontest.cpp` does. However, there is no inherently right or wrong way to organize your test functions.
* The `main` function is responsible for executing the test functions and reporting their outcomes. The most important requirement is that there should be one call to the `TEST` macro for each test function.
* Each *test function* calls member functions on the objects in the test fixture, and uses *assertions* to verify that each member function call produced a correct outcome.  The `ASSERT` macro implements an assertion, which is simply a boolean condition which, if true, indicates that a tested function call produced a correct outcome.

To run a unit test, use `make` to compile the unit test program, and then run the resulting unit test executable.  For example (user input in **bold**):

<div class="highlighter-rouge"><pre>
$ <b>make positiontest</b>
<i>...lots of compiler output...</i>
$ <b>./positiontest</b>
testGetX...passed!
testGetY...passed!
testDistanceFrom...passed!
testEquality...passed!
testInequality...passed!
testDisplace...passed!
testAssign...passed!
testInBounds...passed!
testLessThan...passed!
All tests passed!
</pre></div>

As you can see, invoking the unit test program without any command line argument results in all of the test functions being executed.  You can execute a specific test function by specifying its name on the command line:

<div class="highlighter-rouge"><pre>
$ <b>./positiontest testLessThan</b>
testLessThan...passed!
All tests passed!
</pre></div>

The `setup` and `cleanup` functions are called automatically to give each test function its own freshly-created test fixture.

However: because C++ is a memory-unsafe language (memory bugs can corrupt the program state), test function executions are not truly independent of each other, and a bug triggered by an earlier test function can affect the results of a later test function.  For that reason, using a command line argument to specify a specific test function ensures the most accurate execution for that test function.

### Sharing unit tests

We would like everyone in the course to contribute to a public collection of unit tests!  This should help everyone complete the project: your group can benefit from unit tests written by other groups, and other groups can benefit from unit tests written by your group.

We have created a public Github repository containing the public tests: <https://github.com/jhuintprog/finproj-tests>

Initially, they will be the same as what is included in the project starter code. The repository's README.md file has instructions on how to use and contribute to the public unit tests.

We will be awarding a small amount of extra credit to groups who contribute tests to the public repository.

### Driver program

The driver program should be called with a single game file as a command line argument, and allow the user to play the game, using an instance of `TextUI` as the `UI` object.

Here is a possible `main` function you could use for your driver program:

```cpp
int main(int argc, char *argv[]) {
  const char *filename;
  if (argc < 2) {
    std::cerr << "Error: Need filename" << std::endl;
    return 1;
  }
  filename = argv[1];

  std::ifstream in(filename);
  if (!in.is_open()) {
    std::cerr << "Error: Could not open maze file" << std::endl;
    return 1;
  }

  Game *game = Game::loadGame(in);
  if (!game) {
    std::cerr << "Error: Invalid game file" << std::endl;
    return 1;
  }

  game->setUI(new TextUI());
  game->setGameRules(new BasicGameRules());
  game->gameLoop();

  delete game;

  return 0;
}
```

### Curses driver program

**Important**: This part is completely optional, and just for fun! Don't work on it until you've completed all of the required features.

The [curses](https://en.wikipedia.org/wiki/Curses_(programming_library)) library allows full control over the cursor and keyboard input for programs running in a terminal.  You can use curses to created a "screen-oriented" version of the user interface.

To do this, create a `CursesUI` class that derives from `UI`.  Create a new driver program that creates a `CursesUI` object rather than a `TextUI` object as the `Game`'s user interface object.

Use the [ncurses](https://invisible-island.net/ncurses/announce.html) variant of the curses library. This is available on the ugrad machines.  Your `cursesui.cpp` file should `#include <ncurses.h>` in order to use ncurses.

## Hints and specifications

To a significant extent, comments in the header files describe the correct behavior of each member function you will need to implement.  This section provides additional details.

### No value semantics

One thing you will notice about most of the classes in this project is that the copy constructor and assignment operator are private and not implemented.  This is because for classes intended to exhibit polymorphism, value semantics — the ability to copy, assign, pass, and return objects by value — are usually not useful.  For this reason, nearly every object in this assignment will be created dynamically and accessed via a pointer.  The one exception is `Position`, which does have value semantics.

Because most objects will be dynamically allocated, you will need to think carefully about how to ensure that the program has no memory leaks.  Pay careful attention to member functions which are documented as assuming responsibility for deleting an object.  For example, this is the declaration of the `addTile` member function in the `Maze` class:

```cpp
// Set a Tile at the specified Position.  The Maze assumes responsibility
// for deleting the Tile.
void setTile(const Position &pos, Tile *tile);
```

This means that the `Maze` object should delete its `Tile` objects when its destructor is called.

### Forward declarations

In many of the header files, you will notice *forward declarations* like the following:

```cpp
class Tile;
```

A forward declaration indicates that a class exists, but does not reveal anything about its implementation. A forward declaration is sufficient to allow pointers and references to the referred-to type to be declared.  However, actual *uses* of the type, such as instantiating an object or calling a member function, require the definition of the class to be known.

When you are implementing the `.cpp` files for each class, you will need to `#include` the appropriate header files for all types known only as forward declartions.

### Reading maze data

The `Maze` class's `read` static member function reads maze file data from an `istream`.  A maze file consists of a series of lines.  The first line will have two integers representing the maze's width and height.  Based on the maze height, a number of lines will follow, each line specifying one row of the maze.  The number of characters of each line should be exactly equal to the maze width.  Each character represents one tile of the maze.  A tile character can be converted into a `Tile` object by calling the `createFromChar` member function on the singleton instance of `TileFactory`, which you can get by calling `TileFactory::getInstance()`.  If the `TileFactory`'s `createFromChar` member function returns a null pointer, it means the tile character is not valid, in which case the maze is not valid, and `Maze::read` should return a null pointer.

Here is an example maze file:

```
20 10
####################
#................<.#
#..................#
#...###............#
#.....#............#
#.....#............#
#...###............#
#..................#
#..................#
####################
```

### Reading game data

The `Game::loadGame` static member function reads game data from an `istream`.  A game file consists of maze data, followed by a single line containing one or more *entity descriptors*.

An entity descriptor is a character string encoding an `Entity`, followed by integers specifying x and y coordinates defining the `Entity`'s initial position.  The string consists of at least two characters:

* The first character is the entity's *glyph*, which is the character used to represent the entity in the user interface.
* The second character specifies the entity's `EntityController`; this character can be passed to the `EntityControllerFactory`'s `createFromChar` member function to instantiate the `EntityController` that the entity should use to determine its movement.

Any remaining character in the entity descriptor are the entity's *properties*, which should be set using the `setProperties` member function.

Here is an example game file:

```
20 10
####################
#................<.#
#..................#
#...###............#
#.....#............#
#.....#............#
#...###............#
#..................#
#..................#
####################
@uh 3 5 Mcm 17 5
```

In the example game file above, there are two entities:

* The first is represented by the "@" glyph, uses the "u" `EntityController` implementation (`UIControl`), has the property "h" ("Hero"), and its initial position is x=3, y=5
* The second is represented by the "M" glyph, uses the "c" `EntityController` implementation (`ChaseHero`), has the property "m" ("Minotaur"), and its initial position is x=17, y=5

The `Game::loadGame` function should return a null pointer if the game data is not valid.

### Entity properties

There are several entity properties having meanings that are significant for gameplay.

The Hero entity has the "h" property, and the Minotaur has the "m" property.  If an entity with the "m" property captures an entity having the "h" property, it means the Hero has been captured, and loses the game.  If an entity with the "h" property reaches a goal `Tile`, the Hero wins the game.

An entity with the "v" property ("mo**v**eable") can be pushed.

The `getEntitiesWithProperty` member function of the `Game` class is useful for getting the entities having a specific property.  Your game should *not* assume that there is any specific number of entities with any property.  For example:

* There could be a game with multiple Minotaurs (entities with the "m" property)
* There could be a game with no Minotaur
* There could be a game with multiple Heroes (entities with the "h" property)

### Entity, EntityController classes

Instances of the `Entity` class represent entities in the game that can (potentially) act.  Each `Entity` has a position (x/y coordinates specifying a position within the maze.)

Each `Entity` will have an `EntityController` object which controls it.  The `EntityController` proposes how the controlled `Entity` should move.  The `UIControl` implementation of `EntityController` gets proposed moves from the user interface (represented by a `UI` object.)  Other implementations of `EntityController` (`ChaseHero`, `AStarChaseHero`, and `Inanimate`) use code to compute proposed moves for the controlled `Entity`.

The `UIControl` implementation of `EntityController` should use the singleton instance of the `UI` class to get a proposed move for the controlled `Entity`.  The code to get a suggested move from the `UI` object should look something like this:

```cpp
UI *ui = UI::getInstance();
Direction dir = ui->getMoveDirection();
```

The `ChaseHero` implementation of `EntityController` should move the controlled entity towards the closest `Entity` object with the "h" (Hero) property.  Specifically, it should determine the horizontal distance and vertical distance to the closest Hero (as determined by the `distanceFrom` member function in the `Position` class.)  If the Hero is farther horizontally than vertically, the proposed move should be horizontal, otherwise the proposed move should be vertical. If the Hero is equally far both horizontally and vertically, horizontal movement is preserved.  However, if the controlled `Entity` is blocked along the preferred axis, but movement is possible along the other axis, then the proposed move should be along the unblocked axis.  `ChaseHero` is essentially a simple AI to control the Minotaur, but it's not very smart, and allows the Minotaur to become blocked on dead-end paths in the maze.

The `AStarChaseHero` implementation of `EntityController` should use the [A\* search algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm) to move the controlled `Entity` towards the closest entity with the "h" (Hero) property (as determined by the `distanceFrom` member function in the `Position` class.)  As the heuristic function, use the total number of horizontal and vertical to the Hero (this can be found easily using the `distanceFrom` member function of the `Position` class.)  Note that implementing this algorithm is fairly challenging, but is very satisfying when you get it to work.  The A\* algorithm produces a complete path to the goal, but the `getMoveDirection` method should just propose a move in the direction of the first "hop" of the path.  When the Minotaur is controlled by `AStarChaseHero`, its movement will be considerably more "intelligent" than when controlled by `ChaseHero`.  (Please resist the temptation to simply copy an implementation of A\* from a web page.)

### The Game class

An instance of the `Game` class encapsulates all of the game objects.  It has two important member functions for allow game progress.

To some degree, a `Game` object is just a container for `Maze`, `UI`, `GameRules`, and `Entity` objects.  However, it has two important member functions for orchestrating the overall game progress.

The `takeTurn` member function allows one `Entity` object to take a turn.  This means that the entity's `EntityController` proposes a move direction, and then the `GameRules` object evaluates whether the move is allowed, and if so, carries it out.  If the entity is controlled by the user (its `EntityController`'s `isUser` member function returns true), and the proposed move was not allowed, the message "`Illegal Move`" should be displayed using the `UI` object's `displayMessage` member function.

The `gameLoop` member function repeatedly allows `Entity` objects to take turns.  The `Entity` objects should take turns one at a time, in round robin fashion, in the order in which they were added to the `Game`.  For example, if the Hero entity was added first, and then the Minotaur entity, then their turns should be taken in the order Hero, Minotaur, Hero, Minotaur, etc.  The game ends when the `GameRules` object determines that, after an entity has taken a turn, the `GameResult` is either `GameResult::HERO_WINS` or `GameResult::HERO_LOSES`.

The `gameLoop` function should also implement the following additional behavior:

* Just before an entity takes a turn, if the entity is controlled by the user (its `EntityController`'s `isUser` member function returns true), the `UI` object's `render` member function should be called. This allows the user to see the current game state.
* When the game ends, the `UI` object's `displayMessage` should be called to display either "`Hero wins`" or "`Hero loses`".

### GameRules, BasicGameRules classes

The `GameRules` class defines the member functions which

* determine which moves are legal (requests by entity objects to move from their current position in the maze to another position)
* carries out legal moves
* determines whether the game has been won or lost by the Hero, or is still in progress

`GameRules` is an abstract class, and the actual behavior of its member functions is only specified at the level of a concrete derived class.

The `BasicGameRules` is the "standard" implementation of `GameRules`.  It behaves as follows.

The `allowMove` member function should only allow an `Entity` to make a move if either

1. it is onto an adjacent unoccupied `Tile`, and the `Tile`'s `checkMoveOnto` member function allows the move, or
2. it is onto an adjacent `Tile` occupied by an entity with the "v" (moveable) property, and the moveable entity is permitted to move onto an unoccupied adjacent `Tile` in the same direction that the original entity is moving

Moves out of bounds, or moves by more than 1 unit of distance, are not allowed under any circumstances.

Case 2 of `allowMove` allows an entity to "push" a moveable entity.  Here's an example where the Hero pushes an inanimate entity represented by the `*` glyph:

<center>
<video width="658" height="462" controls>
  <source src="media/demo5.webm" type="video/webm">
Your browser does not support the video tag.
</video>
</center>

**Important**: the `allowMove` member function should not change any game data, such as the position of any `Entity` objects. It just determines whether or not a proposed move would be legal.

The `enactMove` member function carries out a move approved by a prior call to `allowMove`.  Usually, this just means changing the current position of an `Entity`.  In the case of an entity pushing a moveable entity, the positions of both entities will need to be updated.

The `checkGameResult` member function should return a `GameResult` value based on the current game state.  If any entity with the "h" property has reached a `Goal` tile, it should return `GameResult::HERO_WINS`.  If any entity with the "m" property occupies a position where an entity with the "h" property is located, it should return `GameRules::HERO_LOSES`.  Otherwise, it should return `GameResult::UNKNOWN`.

### TextUI class

The `TextUI` class is an implementation of `UI` which interacts with the user using `cin` and `cout`.  Its member functions should behave as follows.

The `getMoveDirection` should print the prompt `Your move (u/d/l/r): `, and then read a single input token.  If the token is `u`, `d`, `l`, or `r`, meaning "up", "down", "left", or "right", then the appropriate `Direction` value should be returned. Otherwise, the message "`Unknown direction`" should be printed, and the function should try again to read a valid direction.  The function should continue until the user enters a valid direction.

The `displayMessage` member function should save the parameter string value in a field.

The `render` member function should print a representation of the maze, one line of text per row of the maze, using the appropriate glyph character for each `Tile` and `Entity`.  As a special case, if two entities occupy the same position, the glyph of the entity that was added later should take priority. (For example, if the Minotaur entity was added after the Hero entity, and the Minotaur has captured the Hero, the Minotaur's glyph should be printed, rather than the Hero's glyph.)

Also, after printing the representation of the maze, the `render` member function should check to see if a message (from a prior call to `displayMessage`) is waiting to be displayed.  If so, it should be printed on a single line after the maze representation, preceeded by a colon and space.  Once a message has been printed by a call to `render`, it should be cleared.  (I.e., a message should only be displayed by a single call to `render`.)

## Recommended approach

This section explains our recommendations to making progress on this project.

*Start with the simplest classes.* The `Wall`, `Floor`, and `Goal` classes are fairly simple. The `tiletest.cpp` test program has some useful tests for them, and you can add more of your own.  `Maze` is also a fairly straightforward class to implement, and has some tests in `mazetest.cpp`.

*Create stub implementations of classes that are required for compilation and linking, but which you aren't ready to work on.* The `EntityControllerFactory` class won't work until you have implemented *all* of the classes which derived from `EntityController`.  One of these, `AStarChaseHero`, is fairly challenging to implement. However, you won't need a working version of this class until relatively late in your development work. So, you can create a *stub* implementation whose member functions throw an exception or deliberately fail an assertion if called. For example, your stub implementation of `AStarChaseHero`'s `getMoveDirection` member function might look like this:

```cpp
Direction AStarChaseHero::getMoveDirection(Game *game, Entity *entity) {
  assert(false);
  return Direction::NONE;
}
```

*Write unit tests*. Unit tests will help you get individual classes working. We encourage you to use and contribute to the shared repository of unit tests: see the [Sharing unit tests](#sharing-unit-tests) section.

## Deliverables, submitting

Before you submit:

* Create a `gitlog.txt` file using the command `git log > gitlog.txt`
* Create a README file with the names and JHED IDs of each team member, describing each team member's main contributions to the project
* Make sure that each source file has a comment at the top with each team member's name and JHED ID

To submit, create a zipfile containing your `Makefile`, code, README, and `gitlog.txt`:

```
zip -9r finproj-submission.zip Makefile *.h *.cpp gitlog.txt README
```

Upload the zipfile to Gradescope.  Only one team member should submit.

<!--
vim:wrap linebreak nolist:
-->
