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

**Important**: You must implement the public member functions *exactly* as specified.  Do not add any additional public member functions.  You may add fields and private member functions.

Each public member function has a detailed comment describing what the public member function should do.  Additional details are below in the [hints and specifications](#hints-and-specifications) section.

The following classes are provided (you should not modify them):

Class                     | Filename (.h and .cpp)
------------------------- | ----------------------
`EntityControllerFactory` | `ecfactory`
`EntityController`        | `entitycontroller`
`GameRules`               | `gamerules`
`Position`                | `position`
`ScriptedControl`         | `scriptedcontrol`
`TileFactory`             | `tilefactory`
`Tile`                    | `tile`
`UI`                      | `ui`

### Unit testing

TODO: discuss writing unit tests, running unit tests, collaborating on unit tests

## Driver program

TODO: discuss expectations for what the driver program should do

<!--
 | `tctestpp`
-->


## Hints and specifications

TODO
