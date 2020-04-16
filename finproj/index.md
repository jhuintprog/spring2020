---
layout: default
title: "Final project"
---

*Preliminary content, not official*

# Hero vs. Minotaur

In this project, your team will implement a maze game in which the Hero ("@") tries to escape and avoid being captured by the Minotaur ("M").

TODO: videos and playable demo.

## Getting started

Create a bitbucket repository for your team, and make sure that all of the team members have write permission.  Make sure you create a *private* repository.  One team member should clone the bitbucket repository to create a local repository.

Pull from the public repository.  You will find a directory called `final-project/starter-code`.  Copy the files in this directory into the clone of your team repository (only one team member needs to do this).  Commit the starter code, and then push.  Now the other team members can clone the team repository to create their own local repositories.

## Tasks

Your tasks are

* Implement the specified classes
* Test the specified classes thoroughly, including unit testing
* Implement a driver program that allows the user to play the game

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

### Unit testing

One challenge that arises in creating a complex program is that there are many components that need to work correctly in order for the overall program to work correctly.  In the case of a program written in an object-oriented language (like C++), these components are classes.  One important and useful way to make progress on the implementation of a complex program is to develop and test classes separately.  If the functionality of each class is tested thorougly, you can have some confidence that the overall program will work correctly once all of the individual classes are combined.

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

However: because C++ is a memory-unsafe language (memory bugs can corrupt the program state), test function executions are not truly independent of each other, and a bug triggered by an earlier test function can affect the results of a later test function.  For that reason, using a command line argument to specify a specific test function ensures the most accurate testing for that test function.

## Driver program

TODO: discuss expectations for what the driver program should do

<!--
 | `tctestpp`
-->


## Hints and specifications

TODO

## Recommended approach

This section explains our recommendations to making progress on this project.

*Start with the simplest classes.* The `Wall`, `Floor`, and `Goal` classes are fairly simple. The `tiletest.cpp` test program has some useful tests for them, and you can add more of your own.

*Create stub implementations of classes that are required for compilation and linking, but which you aren't ready to work on.* The `EntityControllerFactory` class won't work until you have implemented *all* of the classes which derived from `EntityController`.  One of these, `AStarChaseHero`, is fairly challenging to implement. However, you won't need a working version of this class until relatively late in your development work. So, you can create a *stub* implementation whose member functions throw an exception or deliberately fail an assertion if called. For example, your stub implementation of `AStarChaseHero` might look like this:

```cpp
Direction AStarChaseHero::getMoveDirection(Game *game, Entity *entity) {
  assert(false);
  return Direction::NONE;
}
```

<!--
vim:wrap linebreak nolist:
-->
