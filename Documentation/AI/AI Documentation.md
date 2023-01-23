# Character
The character for the AI is simple UE5 mannequin with player animations applied. It is engine default player copied over. The material was slightly changed to show different hue color.

# Gameplay
The gameplay relies on player trying to escape the AI for duration of given time on screen. When the time runs out and player wasn’t caught by the AI, they win. AI will deal damage in 25% marks meaning that the player can be caught 3 times and still win, as they will lose on the 4th time.

# Logic
The AI follows simple logic from behaviour tree. 

![image](https://media.github.falmouth.ac.uk/user/759/files/d51edb2e-3930-4154-9d81-61189a09880a)

The AI rotates towards player upon sensing them, then uses chase task to go after them. When the player is no longer sensed, the AI continues on patrol tasks that gets random location within radius. Random waits between 0 and 4 seconds allow for slight differentiation. 
# Sensing

![image](https://media.github.falmouth.ac.uk/user/759/files/903cecd0-912d-4415-9fcb-9a25ddae08c0)

AI can sense the player in two ways, either by seeing them or by hearing them. This means that if the player is in front of the AI they can be spotted from a long range, while moving behind or on the side of the AI, means that the player can be herd at a shorter range. 

# Big problem
There was a big problem where player jumping between obstacles caused the AI to move to patrol behaviour despite the fact that player was sensed, and the AI would continue to ignore the player for the duration of patrol timer. This issues was caused by the player being in the air, where AI couldn’t find the path and couldn’t detect the player properly.
 
![image](https://media.github.falmouth.ac.uk/user/759/files/3bce0ee1-6647-496a-943e-3e4eca5a917a)

This was fixed by adding composite decorator to behaviour tree that uses abort both and conditional loop to stop the AI from failing.  

![image](https://media.github.falmouth.ac.uk/user/759/files/99a5b4be-46d6-48c9-b29b-b2c34276c0e4)

The AI still detects the player and continues to try to get to them. Unsuccessfully as there isn’t available path, but at least it’s trying.

# Attack
In AI controller, there is a check running for distance between sensed pawn(Player) and possessed pawn(AI). If the distance is less or equal to, the ‘isInRange’ bool will turn true. The behaviour tree then checks for InRange bool and decide if AI should initiate chase or attack. 
![image](https://media.github.falmouth.ac.uk/user/759/files/2c7aee66-6948-4ee1-838a-85c350bc4457)

