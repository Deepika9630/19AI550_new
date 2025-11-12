# Ex.No: 10  Implementation of 2D/3D game 
### DATE: 12.11.2025                                                                           
### REGISTER NUMBER : 212224230054
### AIM: 
To develop a game 2D Flappy Bird–style game in Unity 
### Algorithm:
```
1.Load all game objects (Bird, Pipe Spawner, Logic Manager, Canvas).

2.Initialize Rigidbody2D for the bird.

3.Set player score to 0.

4.Hide the Game Over screen.

5.Wait for player input (spacebar or click).

6.On input, apply upward force to the bird.

7.Let gravity pull the bird down.

8.Spawn pipes at regular time intervals.

9.Randomize the vertical position of pipes.

10.Move pipes continuously to the left.

11.Destroy pipes when they move off-screen.

12.Detect when the bird passes between pipes.

13.Increase the score by 1.

14.Update score text on screen.

15.Check for collision with pipe or ground.

16.If collision occurs, stop bird movement.

17.Stop spawning new pipes.

18.Show Game Over screen.

19.Wait for player to press “Play Again”.

20.Reload the scene and reset the game.
```  
### Program:
```
1. BirdControl.cs

Handles jump, fall, and collision.

using UnityEngine;
using UnityEngine.SceneManagement;

public class BirdControl : MonoBehaviour
{
    public float jumpForce = 5f;
    public Rigidbody2D rb;
    public GameManager gm;
    bool isAlive = true;

    void Start()
    {
        gm = GameObject.FindGameObjectWithTag("Logic").GetComponent<GameManager>();
    }

    void Update()
    {
        if (isAlive && Input.GetKeyDown(KeyCode.Space))
            rb.velocity = Vector2.up * jumpForce;
    }

    private void OnCollisionEnter2D(Collision2D other)
    {
        isAlive = false;
        gm.GameOver();
    }
}

2. GameManager.cs

Handles score and restart.

using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    public int score = 0;
    public Text scoreText;
    public GameObject gameOverPanel;

    public void AddScore()
    {
        score++;
        scoreText.text = score.ToString();
    }

    public void GameOver()
    {
        gameOverPanel.SetActive(true);
    }

    public void RestartGame()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
}

3. PipeMove.cs

Moves the pipes to the left and destroys them when off-screen.

using UnityEngine;

public class PipeMove : MonoBehaviour
{
    public float speed = 3f;

    void Update()
    {
        transform.position += Vector3.left * speed * Time.deltaTime;

        if (transform.position.x < -10f)
            Destroy(gameObject);
    }
}

4. PipeSpawner.cs

Spawns pipes at random heights.

using UnityEngine;

public class PipeSpawner : MonoBehaviour
{
    public GameObject pipePrefab;
    public float spawnRate = 2f;
    float timer = 0f;

    void Update()
    {
        timer += Time.deltaTime;
        if (timer >= spawnRate)
        {
            float yPos = Random.Range(-1f, 2f);
            Instantiate(pipePrefab, new Vector3(6f, yPos, 0), Quaternion.identity);
            timer = 0f;
        }
    }
}

5. ScoreZone.cs

Adds score when the bird passes between pipes.

using UnityEngine;

public class ScoreZone : MonoBehaviour
{
    public GameManager gm;

    void Start()
    {
        gm = GameObject.FindGameObjectWithTag("Logic").GetComponent<GameManager>();
    }

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
            gm.AddScore();
    }
}
```
### Output:
<img width="1919" height="942" alt="Screenshot 2025-10-29 151325" src="https://github.com/user-attachments/assets/0dbc547d-441d-41c4-bac5-46b621d03e82" />

### Result:
Thus the game was developed using Unity and adopted basic AI techniques for object movement and collision detection.
