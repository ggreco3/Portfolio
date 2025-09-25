# README
## Bunny's Big Adventure 
Gia Greco
### [Play game here!](https://ggreco.itch.io/bunnys-big-adventure) 
<br/>

# Project Summary 
Through Unity and Visual Studio Code, Bunny's Big Adventure was developed. This is a game I made as a final project for my Game Development class, featuring the topics covered throughout the semester. It is a three-level platformer game where each level increases in difficulty and ends in a boss level. The player must avoid enemies and collect all the stars to get through the door to the next level. 
<br/>

# Code Structure Breakdown 

## Enemies
There are 4 main enemies in the first two levels:
#### - Chaser:
The chaser sticks to its name; it follows the player at varying speeds depending on difficulty. The chaser is identified as a bee in this game and once it hits you, you lose a life. 
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
The droppers fall from the ceiling when the player comes into a certain radius. They are identified as the red and blue orbs but are meant to be more hidden on the ceilings and surprise attack. Since it falls into the void, dropper deletes five seconds after detecting player to clear space in game, so it does not fall for infinity. 
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
The player controller includes all the standard code needed to create a player with its jump, jump height, speed, and basics like `RigidBody2D`.  It also includes animation for walking and jumping. When certain keys are pressed, then character switches between standing and "walking" sprite and faces the direction of the key pressed.
<br/>

Method `Dead` updates the heart sprites to reflect number of lives; If the lives are 0, then it ends the game.
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

`OnTriggerEnter2D` Detects the collision of the player with the enemies and makes `lives--`. It also detects the collision of player with the coins

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
<br/>

### Other Controls
Minor controller programs include 
- Camera: follow character, not show the "ends" of the map
- Door: if all the stars are collected, when collided with door -> move to next level; if not, display text "Not all stars are collected"
- Poison: making it also harm the player and decrease lives
<br/>

### Design structure 
The final boss level was designed to be played as a speed run, meaning each jump is reached while continuously moving while the boss is chasing after the player. The main player and certain enemies have walking or jumping animations on them, along with sound effects for collecting coins and jumping. The entire game was designed by me, but the sprites were from a pack online at Kenny.nl


## Challenges/Learnings from the Project
When developing the game, I tried new things that we did not learn directly or I took what we did and altered it in a way for my game. For instance, using all of the enemies was a bit of an adjustment trying to implement into the game on my own and adjust the code to fit my game. Also, working with the animations was a struggle because I wanted to include them as it made the game look more professional and interesting. 

I took what I learned in class and applied it for jumping and walking. The hardest part was getting the hearts to work because they had to reflect the lives and carry over from each level. It was a struggle, especially with getting the hearts to reset when the player lost and had to start over, I could not get them to reset. However, I found that putting the same if-else statements in the `Update` method, it would apply the according number of lives to hearts left. It was really cool to think deeply about how these seemingly simple elements look in a game compared to how they are actually made. The hearts were simply sprites that would switch to an empty looking heart and cover the full then disappearing.

When test running my game to the class and having other people play, it made me realize that some of the gameplay was not as obvious to others as it was to me since I made the game. For instance, in the first level there is a platform the player has to jump up to and go backwards, but many people did not notice it and walked right past it. <br/>

## Future Plans
In the future, I do plan to add shooting to the game for the enemies and allowing the player to jump on the enemies' heads to also stun them or kill, like in Super Mario Bros. I also want to clean up each level and make them run smoother because right now the player cannot continuously speed run the map (besides the final level). Instead, they have to stop and then jump to the next platform. I think it would make the game more continuous and enjoyable to play, also making it more professional looking. <br/>


## FAQs or Additional Notes
**What keys do I use?** <br/>
S and D to move left and right, space to jump


## AI Attestation
I attest that I did NOT use ChatGPT or any other automated writing system for ANY portion of this assignment
