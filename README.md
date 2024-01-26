# GameCommunicationSpecification
The Game Communication Specification GCS. tries to fromalies a huge number of possible games to make it possible for bots and other tools to interact with them.

## Instalation

## Integration
Both parties the host game and the user (bot, analysis tool ...) need to follow this specification to work.
They get called by a host program that links the game with users.
### Host
A simple example for host construction
```c++
#include "GCS.h"
//extends the GCS class for interfacing 
class MyGame : class GCS::Game{
//...
MyGame() {
//...
int nStates = 7;
int nPositions = 8 * 8;
int changesPerMove = 1; //Setting it to 0 is equiv. to n moves per "tick"
int nPlayers = 2;
int playerIdx = 0; //Selects the player that makes the first move, when set to -1 any player can move at a given "tick"
GCS::State() state = new GCS::State(nStates, nPositions, changesPerMove, nPlayers, playerIdx);
GCS::registerGame("MyGameName", state, rules); //now rules and state cant be set
//...
}
//...
//needed functions as game
void playerAction(GCS::Move() playerMove, int playerIdx) {
  //...
}
//...
}
```
#### GCS::Game
GCS::EnvRules are rules that change the environment of the game like how long can a player take to decide a next move, much time each player has.
#### Methods
- registerGame(std::String gameName, GCS::State state, GCS::Rules rules, GCS::EnvRules)
- playerAction(GCS::Move playerMove, int playerIdx)
- static updateState()
  
  Can be called to changed the state and inform players that there is a changed state
- static setScore(GCS::Score score)
  
  Can end the game and gives the players a score through the Score Object


### User
Simple example for user (player) construction
```c++
#include "GCS.h"
class MyBot : class GCS::Player {
//...
MyBot() {
//...
GCS::State state = GCS::getState();
GCS::Rules rules = GCS::getRules();
GCS::EnvRules envRules = GCS::getEnvRules();
//...
}
//...
//needed functions as player
void gameUpdated() {
//react to game changed
}
std::String gameEnded() {
//Every player has to return a good game phrase at the end of the game, (if its not nice they can get disqualified)
  return "Danke für das Spiel";
}
}
```

#### GCS::Player
#### Methods
- gameUpdated()
- gameEnded() -> std::String
- takeAction(GCS::Move move)
- giveUp()
