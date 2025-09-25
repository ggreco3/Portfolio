# README
## Bunny's Big Adventure
### [Play here!](https://ggreco.itch.io/bunnys-big-adventure) 
<br/>

# Project Summary 
Through Unity and Visual Studio Code, Bunny's Big Adventure was developed. This is a game I made as a final project for my Game Development class, featuring the topics covered throughout the semester.

It is a three-level platformer game where each level increases in difficulty and ends in a boss level. The player must avoid enemies and collect all the stars to get through the door to the next level. 


<br/>

## Instruction to Run Locally
The game can be played online at itch.io
<br/>

# Code Structure Breakdown 

## Enemies
There are 4 main enemies in the first two levels:
#### - Chaser:
The chaser sticks to its name, it follows the player at varying speeds depending on difficulty. The chaser is identified as a bee in this game and once it hits you, you lose a life. 
The final boss also follows this code but at a faster speed.

```
 if (state == "idle") {
            // var distance = Math.Abs(transform.position.x - player.transform.position.x);
            var distance = Vector3.Distance(transform.position, player.transform.position);

            if (distance < threshold) {
                state = "attacking";
            } 
        } else if (state == "attacking") {
            var direction = player.transform.position - transform.position;
            direction.Normalize(); //shrink so magnitude of directions adds to 1
            rb.linearVelocity = direction * speed;

            var distance = Vector3.Distance(transform.position, player.transform.position);
            if (distance > stopIt) {                    // stop chasing 
                state = "idle";
                rb.linearVelocity = Vector2.zero;
            } 
        }
    }
```
#### - Dropper:
The droppers fall from the ceiling when the player comes into a certain radius. They are identified as the red and blue orbs but are meant to be more hidden on the ceilings and surprise attack.
```
 if (state == "idle") {
            var distance = Math.Abs(transform.position.x - player.transform.position.x);
            if (distance < threshold) {
                state = "attacking";
                rb.gravityScale = 1.0f;
                Destroy(this.gameObject, 5);
            }
        }
    }
```
#### - Patroller:
The patrollers are set to walk back and forth between a set point at a certain speed. They are walking red figures that are typically moving fast so one has to jump over them. 
```
lerpTime += Time.deltaTime * lerpSpeed * lerpDirection;

        if (lerpTime >= 1 && lerpDirection > 0) {
            lerpDirection = -1;
            sr.flipX = !sr.flipX;

        } else if (lerpTime <= 0 && lerpDirection < 0) {
            lerpDirection = 1;
            sr.flipX = !sr.flipX;

        }
        transform.position = Vector3.Lerp(startPos, endPos, lerpTime);

    }
```
#### - Walker:
The walkers move when the player enters a certain radius and keep walking in a direction till it hits the wall--they are always moving. 
```
 if (state == "idle") {
            var distance = Math.Abs(transform.position.x - player.transform.position.x);

            if (distance < threshold) {
                state = "attacking";
            } 
        } else if (state == "attacking") {
            rb.linearVelocityX = speed;
        }
    }
```

## Player Controller
The player controller includes all of the standard code needed to create a player with its jump, jump height, speed, and basics like `RigidBody2D`. 
It also includes animation for walking and jumping when certain keys are pressed, then character switches between standing and "walking" sprite and faces the direction of the key pressed

Method `Dead` updates the heart sprites to reflect number of lives and if the lives are 0, then it ends the game
- Updating the heart sprites to reflect number of lives
- 
```
public void Dead() {
        GameData.lives--;
        if (GameData.lives == 0) {
            UnityEngine.SceneManagement.SceneManager.LoadScene("GameOver");
        } else if (GameData.lives == 2){
             heart3.SetActive(false);
           
        } else if (GameData.lives == 1) {
            heart2.SetActive(false);
        }
    }
```
`OnTriggerEnter2D` Detects the collision of the player with the enemies and makes `lives--`, when it gets to 0, then the game ends and lives are reset
- Detects collision of player with coins

```
void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.CompareTag("AI")) {
            Dead();
        }
        if (collision.CompareTag("poison")) {
            Dead();
        }
        if (collision.CompareTag("coin")) {           
            Destroy(collision.gameObject);
            src.PlayOneShot(star);
        }
    }
```


### Other Controls
Minor controllers include 
- Camera: follow character, not show the "ends" of the map
- Door: if all the stars are collected, when collided with door -> move to next level; if not, display text "Not all stars are collected"
- Posion: making it also harm the player and decrease lives

#### Design structure 
The final boss level was designed to be played as a speed run, meaning each jump is reached while continuously moving while the boss is chasing after the player. 


## Challenges/Learnings from the Project
When test running my game to the class and having other people play, it made me realize that some of the gameplay was not as obvious to others as it was to me since I made the game. 

## Future Plans

## FAQs or Additional Notes
**What keys do I use?** <br/>
S and D to move left and right, space to jump



## AI Attestation
