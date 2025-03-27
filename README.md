# Hill Driver Game

Youtube Demo Link: https://youtu.be/1PoNiBWeCGY

A physics-based 2D hill driving game built with Unity, featuring dynamic terrain, realistic vehicle controls, and engaging gameplay elements.

## Game Controls

- **Move Forward:** Press `D` or `Right Arrow` Key
- **Move Backward:** Press `A` or `Left Arrow` Key

## Features

### 🎮 Physics-Based Movement
- Uses Unity's `Rigidbody2D` to simulate realistic car controls.

### 🏞️ Dynamic Terrain Generation
- Procedural terrain ensures a unique driving experience every time.

### ⛽ Fuel System
- Fuel depletes over time, requiring players to collect fuel tanks to keep driving.

### 🎵 Background Music & Sound Effects
- Immersive audio enhances the overall gameplay experience.

## Code Highlights

### 🌍 World Generator
```csharp
using UnityEngine;
using UnityEngine.U2D;

public class WorldGenerator : MonoBehaviour
{
    [SerializeField] private SpriteShapeController spriteShapeController;
    [SerializeField, Range(3, 100)] private int terrainSegments = 50;
    [SerializeField, Range(1f, 50f)] private float horizontalScale = 2f;
    [SerializeField, Range(1f, 50f)] private float verticalScale = 2f;
    [SerializeField, Range(0f, 1f)] private float smoothingFactor = 0.5f;
    [SerializeField] private float noiseFrequency = 0.5f;
    [SerializeField] private float terrainDepth = 10f;
    private Vector3 lastPosition;

    private void OnValidate()
    {
        if (spriteShapeController == null)
        {
            Debug.LogError("Terrain SpriteShapeController is not assigned!");
            return;
        }

        spriteShapeController.spline.Clear();

        for (int i = 0; i < terrainSegments; i++)
        {
            lastPosition = transform.position + new Vector3(i * horizontalScale, Mathf.PerlinNoise(0, i * noiseFrequency) * verticalScale, 0);
            spriteShapeController.spline.InsertPointAt(i, lastPosition);

            if (i != 0 && i != terrainSegments - 1)
            {
                spriteShapeController.spline.SetTangentMode(i, ShapeTangentMode.Continuous);
                spriteShapeController.spline.SetLeftTangent(i, Vector3.left * horizontalScale * smoothingFactor);
                spriteShapeController.spline.SetRightTangent(i, Vector3.right * horizontalScale * smoothingFactor);
            }
        }
    }
}
```

### 🚗 Car Driving Controls
```csharp
using UnityEngine;

public class DriveCar : MonoBehaviour
{
    [SerializeField] private Rigidbody2D frontTireRB;
    [SerializeField] private Rigidbody2D backTireRB;
    [SerializeField] private Rigidbody2D carRB;
    [SerializeField] private float speed = 150f;
    [SerializeField] private float rotationSpeed = 300f;
    private float moveInput;

    private void Update()
    {
        moveInput = Input.GetAxisRaw("Horizontal");
    }

    private void FixedUpdate()
    {
        frontTireRB.AddTorque(-moveInput * speed * Time.fixedDeltaTime);
        backTireRB.AddTorque(-moveInput * speed * Time.fixedDeltaTime);
        carRB.AddTorque(moveInput * rotationSpeed * Time.fixedDeltaTime);
    }
}
```

### 🔄 Game Over & Restart
```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManagerController : MonoBehaviour
{
    public static GameManagerController instance;
    [SerializeField] private GameObject gameOverCanvas;

    private void Awake()
    {
        if (instance == null)
        {
            instance = this;
        }
        Time.timeScale = 1.0f;
    }

    public void GameOver()
    {
        gameOverCanvas.SetActive(true);
        Time.timeScale = 0f;
    }

    public void RestartGame()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }
}
```

## 📦 Installation & Usage

1. Clone the repository:
2. Open the project in Unity.
3. Play the game in the Unity Editor or build it for your target platform.


## 👤 Author
**Anupam Pandey**  
*Student ID: 000921043*
