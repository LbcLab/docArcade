cpp_arcade project documentation
=============================

Resume
-----------
This project consists in a modular game plateform. This plateform includes at least two games and three graphic libraries. Theses libraries should be shared and be dynamically handled. Because we should validate the modularity of this project, it will be independently developped by two distinct teams. Because we must be able to share games and graphic libraries within our teams, we decided some conventions. Theses conventions are defines by several shared interfaces, structures and enumerations that will be exposed in this document.

----------

Teams
---------

This documentation belongs to two teams of the Epitech Nantes Tek 2nd year.

Team 1 :  Let's play a game
:	Charles Paulet
:	Emmanuel Robert
:	Léo Paol

Team 2 :  Insert coin
:	Clovis Péridy
:	Théophile Champion
:	Hugo Ailleres


----------

Mode.hh
------------

This file defines the different games modes that are available and mandatory.


```cpp
#ifndef MODE_HH_
# define MODE_HH_

namespace arc
{
  enum mode : int
  {
    MODE_MENU,
    MODE_PLAY
  };
};

#endif /* !MODE_HH_ */
```

The **enum Mode** defines the different mode available that the program can handle


**MODE_MENU**
:	An menu screen pauses the games and furnish the user some game related options

**MODE_PLAY**
:	The game is running

----------

ICore.hh
-----------
This interface defines the way to implement the **Core**
The **Core** is the abstract class that possesses and inner link managers (**LibManager, GameManager, EventManager**).

```cpp
#pragma once
#ifndef ICORE_HH_
# define ICORE_HH_
# include "mode.hh"

class IEventManager;
class IGameManager;

class ICore
{
  public:
    virtual ~ICore() {}
    
    virtual void run(void) = 0;
    virtual void handleEvent(void) = 0;
    
    virtual IEventManager *getEventManager(void) const = 0;
    virtual IGameManager *getGameManager(void) const = 0;
    virtual ILibManager *getLibManager(void) const = 0;
    
    virtual arc::mode getGameMode(void) const = 0;
    virtual bool isRunning(void) const = 0;

    virtual void mutexLock(void) = 0;
    virtual void mutexUnlock(void) = 0;

    virtual std::vector<arc::Tile> *getTiles(void) const = 0;
    virtual void setTiles(std::vector<arc::Tile> *tiles) = 0;
};


#endif /* !ICORE_HH_ */
```


**virtual void run(void) = 0;**
:	Take care of the program process. It links input and output informations comming from the managers

**virtual void handleEvent(void) const = 0;**
:	Call libraries and games routines by handling events

**virtual arc::mode getGameMode(void) const = 0;**
:	Accessor one the Mode enumeration

**virtual IEventManager *getEventManager(void) const = 0;**
:	Accessor on the events manager

**virtual IGameManager *getGameManager(void) const = 0;**
:	Accessor on the game manager


**virtual IGameManager *getLibManager(void) const = 0;**
:	Accessor on the library manager

**virtual bool isRunning(void) const = 0;**
:	Accessor on the main loop condition


**virtual void mutexLock(void) = 0;**
:	Lock ressources by using mutex(es)

**virtual void mutexUnlock(void) = 0;**
:	Unlock ressources by using mutex(es)

**virtual std::vector<arc::Tile> *getTiles(void) const = 0;**
:	Accessor on tiles structure,

**virtual void setTiles(std::vector<arc::Tile> *tiles) = 0;**
:	Setter on tiles structure

-----------------

eventType.hh
------------------

This is the file that defines all the different events that can occur during the program (including games).

```cpp
#pragma once
#ifndef EVENTTYPE_HH_
# define EVENTTYPE_HH_

namespace arc
{
  enum eventType : int
  {
    MY_KEY_UP,
    MY_KEY_DOWN,
    MY_KEY_LEFT,
    MY_KEY_RIGHT,
    MY_KEY_PREV_GRAPH,
    MY_KEY_NEXT_GRAPH,
    MY_KEY_PREV_GAME,
    MY_KEY_NEXT_GAME,
    MY_KEY_RESTART_GAME,
    MY_KEY_GO_BACK_MENU,
    MY_KEY_ESCAPE,
    MY_NONE
  };
};

#endif
```

-----------------

IEventManager.hh
------------------------

The **eventManager** gets and stores several events that have been sent through the graphic library.

```cpp
#pragma once
#ifndef IEVENTMANAGER_HH
# define IEVENTMANAGER_HH

#include "event.hh"

class IEventManager
{
  public:
    virtual ~IEventManager() {}

	virtual const arc::eventType pullAnEvent() = 0;
    virtual void notifyAnEvent(const arc::eventType &event) = 0;
};

#endif /* IEVENTMANAGER_HH */
```

**virtual const arc::eventType pullAnEvent() = 0;**
:	Retrieve, extract the last occured event

**virtual void notifyAnEvent(const arc::eventType &event) = 0;**
:	Notify the event manager that an event occured and type it.

-------------

IGameManager.hh
------------------------

The interface that describes the games manager.
The constructor of the abstract must open and instanciate at least one game.

```cpp
#pragma once
#ifndef IGAMEMANAGER_HH_
# define IGAMEMANAGER_HH_

class IGameManager
{
  public:
    virtual ~IGameManager() {}

    virtual IGame * getCurrentGame(void) const = 0;
    
    virtual void nextGame() = 0;
    virtual void prevGame() = 0;
};

#endif /* IGAMEMANAGER_HH_ */
```

**virtual IGame * getCurrentGame(void) const = 0;**
:	Accessor on the actual game

**virtual void nextGame() = 0;**
:	Load next game in order to run it directly

**virtual void prevGame() = 0;**
:	Load previous game in order to run it directly

--------------------

ILibManager.hh
---------------------

The interface that describes the libraries manager.
The constructor of the abstract must open and instanciate at least one graphic library.

```cpp
#pragma once
#ifndef ILIBMANAGER_HH_
# define ILIBMANAGER_HH_

class IGraph;

class ILibManager
{
public:
  virtual ~ILibManager() {}

  virtual IGraph * getCurrentGFX(void) const = 0;
  
  virtual void nextGFX() = 0;
  virtual void prevGFX() = 0;
};

#endif /* ILIBMANAGER_HH_ */
```


**virtual IGraph * getCurrentGFX(void) const = 0;**
:	Accessor on the actual graphic library

**virtual void nextGFX() = 0;**
:	Load next graphic library in order to run it directly

**virtual void prevGFX() = 0;**
:	Load previous graphic library in order to run it directly

---------------

Tile.hpp
------------

This file describes the way that a tile should be implemented.

```cpp
#pragma once
#ifndef TILE_HPP_
# define TILE_HPP_

#include <string>

namespace arc
{
  struct Tile
  {
    Tile(int x, int y, const std::string &texture) : 
    x(x), y(y), texture(texture) {}
    
    Tile() : x(0), y(0), texture("") {}
    ~Tile() {}
    
    int           x;
    int           y;
    std::string       texture;
  };
};

#endif /* TILE_HPP_ */
```

A tile possesses a position (int x, int y) and the name of a texture.
This texture is manipulated as a string in order to search an asset with the corresponding name in the file system. These manipulations will not be described in this document because it is not strictly related to the games and graphics libraries that must be shared within the teams.

---------------

IGame.hh
-------------

The interface that every game must implement.
**eventType.hh** and **Tile.hpp** are closely related to the game workflow

```cpp
#pragma once
#ifndef IGAME_HH_
# define IGAME_HH_

# include <vector>
# include "eventType.hh"

# include "Tile.hpp"

class IGame
{
  public:
    virtual ~IGame() {}
    
    virtual void restart() = 0;
    virtual std::vector<arc::Tile> * generateTiles(const bool all = false) = 0;
    virtual void handleEvent(const arc::eventType event) = 0;
    virtual const std::string & getName() const = 0;
    virtual int getMapWidth(void)  const = 0;
    virtual int getMapHeight(void) const = 0;
    virtual int getTXTSize(void) const = 0;
    virtual int getPNGSize(void) const = 0;
    virtual int getScore(void) const = 0;
    virtual int getNbLive(void) const = 0;
};

#endif /* !IGAME_HH_ */
```

**virtual void restart() = 0;**
:	Restarts the game

**virtual std::vector<arc::Tile> * generateTiles(const bool all = false) = 0;**
:	Generates tiles. The Core should transfer them to the graphic library in order to proceed rendering

**virtual void handleEvent(const arc::eventType event) = 0;**
:	Events entry point for the game. The game should use the events that should be provided by the core
:	(which are transfered from the graphic library to the event manager)

**const std::string & getName() const = 0;**
:	Accessor for the game name

**virtual int getMapWidth(void)  const = 0;**
:	Accessor for the map width

**virtual int getMapHeight(void) const = 0;** 
: Accessor to the map height

**virtual int getTXTSize(void) const = 0;**
: Accessor to the size of ncurses asset

**virtual int getPNGSize(void) const = 0;**
: Accessor to the size of .png asset

**virtual int getScore(void) const = 0;**
: Accessor to the score game

**virtual int getNbLive(void) const = 0;**
: Accessor to the number of lifes in a game

-----------------

IGraph.hh
-------------

**IGraph**  must be the interface of every graphic library

```cpp
#pragma once
#ifndef IGRAPH_HH_
# define IGRAPH_HH_

# include <vector>
# include "Tile.hpp"

class IGraph
{
  public:
    virtual ~IGraph() {}

    virtual void openGFXContext(void) = 0;
    virtual void closeGFXContext(void) = 0;
  
    virtual void notifyEventManager(void) = 0;

    virtual void clearScreen(void) = 0;

    virtual void renderByFrame(std::vector<Tile> *) = 0;
    virtual void renderByLoop(void) = 0;

    virtual void renderMenu(void) = 0;
	
	virtual void renderGameOver(void) = 0;
};

#endif /* !IGRAPH_HH_ */
```

**virtual void openGFXContext(void) = 0;**
:	Initialize graphic library context.

**virtual void closeGFXContext(void) = 0;**
:	Terminate graphic library context.

**virtual void notifyEventManager(void) = 0;**
:	Notify events manager with shared events

**virtual void clearScreen(void) = 0;**
:	Clears the screen while switching modes/games 

**virtual void renderByFrame(std::vector<Tile> *) = 0;**
:	Render the tiles provided by the game frame by frame

**virtual void renderByLoop(void) = 0;**
:	Render the tiles provided by using the graphic library main loop

**virtual void renderMenu(void) = 0;**
:	Pauses the game by rendering the game menu

**virtual void renderGameOver(void) = 0;**
: Rendering the game over screen

-----------------

displayMode.hh
-------------

**displayMode** defines the different methods that can be used by the graphic library to render tiles.

```cpp
#ifndef DISPLAYMODE_HH_
# define DISPLAYMODE_HH_

namespace arc
{
  enum displayMode : int
  {
    RENDER_FRAME,
    RENDER_LOOP
  };
};

#endif
```

**RENDER_FRAME**
:	Classical way to render tiles. This mode is printing frame by frame

**RENDER_LOOP**
:	This mode uses the graphic library main loop with a callback function to render tiles

-----------------

Asset file
-------------

```
CHAR
COLOR_CHAR
COLOR_BACK
```

Exemple :

```
c
123
255
```