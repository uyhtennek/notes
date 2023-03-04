[Game Manager - Controlling the flow of your game [Unity Tutorial] - YouTube](https://www.youtube.com/watch?v=4I0vonyqMi8)
___

```c#
public class GameManager : MonoBehavioura {
	
	public static GameManager Instance;
	
	public GameState state;
	
	public static event Action<GameState> OnGameStageChanged;
	
	void Awake() {
		Instance = this;
	}
	
	void Start() {
		UpdateGameState(GameState.SelectColor);
	}
	
	public void UpdateGameState(GameState newState) {
		State = newState;
		
		switch (newState) {
			case GameState.SelectColor;
				break;
			case GameState.PlayerTurn:
				break;
			default:
				throw new ArgumentOutOfRangeException(nameof(newState), newState, null);
		}
		
		OnGameStateChanged?.(newState);
	}
}

public enum GameState {
	SelectColor,
	PlayerTurn,
	EnemyTurn,
	Victory,
	Lose
}
```

```c#
public class MenuManager : MonoBehaviour {
	[SerializeField] private GameObject _colorSelectPanel, _attackButton;
	[SerializeField] private TextMeshProUGUI _stateText;
	
	void Awake() {
		GameManager.OnGameStateChanged += OnEnterState;
	}
	
	void OnDestroy() {
		GameManager.OnGameStateChanged -= OnEnterState;
	}
	
	private void OnEnterState(GameState state) {
		_colorSelectPanel.SetActive(true);
	}
	
	public void AttackPressed() {
		UnitManager.Instance.Attack(Faction.Player);
	}
}
```
___
