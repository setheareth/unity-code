using UnityEngine;
using UnityEngine.SceneManagement;

public class LevelController : MonoBehaviour
{
    public Spawner spawner;
    public static int level = 1;
    public static bool finished = false;
    public GameObject victoryPanel;
    public GameObject defeatPanel;

    private void Awake()
    {
        level = PlayerPrefs.GetInt("level", 1);
    }
    void Start()
    {
        finished = false;
    
    }

    void Update()
    {
        if (!finished)
        {
            Check();
        }
    }
    public void Check()
    {
        if (spawner.spawnCounter <= 0)
        {
            Enemy[] enemies = FindObjectsByType<Enemy>(FindObjectsSortMode.None);

            if (enemies.Length <= 0)
            {
                Victory();
            }
        }

        if (Corn.singleton.health <= 0)
        {
            Defeat();
        }
    }

    public void Victory()
    {
        finished = true;
        print("Victory");
        level += 1;
        victoryPanel.SetActive(true);

        PlayerPrefs.SetInt("level", level);
        GameController.SaveLevel();
    }

    public void Defeat()
    {
        finished = true;
        print("Defeat");
        level = 1;
        defeatPanel.SetActive(true);
    }
    public void RestartLevel()
    {
        int index = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadSceneAsync(index);

    }
    public void LoadMenu()
    {
        SceneManager.LoadSceneAsync("MainMenu");
        
    }
}
