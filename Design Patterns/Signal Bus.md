# Concept of SignalBus in Godot

In Godot, a SignalBus is a design pattern used to decouple communication between different parts of your game by using signals. This pattern helps in keeping your code modular and reduces dependencies between nodes. Essentially, a SignalBus acts as a centralized place to define and emit signals, allowing various parts of your game to communicate without directly referencing each other.

## Concept of SignalBus

1. **Centralized Signal Management**: Instead of each node managing its own signals, you define all signals in a single script (SignalBus).
2. **Decoupling**: Nodes do not need to know about each other to communicate. They only need to know about the SignalBus.
3. **Flexibility**: Easily add, remove, or modify signals without changing the nodes themselves.

## Example of SignalBus

Let's create a simple example to demonstrate how to use a SignalBus in Godot. In this example, we will have a player node that emits a signal when it collects an item, and a UI node that listens to this signal and updates the score.

### Step 1: Create the SignalBus

Create a new script `SignalBus.gd`:

```gdscript
extends Node

# Define signals
signal item_collected(item_name)

# Singleton pattern to access SignalBus globally
func _init():
    Singleton = self
```

### Step 2: Set up the Singleton

In your project settings, go to **Project > Project Settings > AutoLoad** and add the `SignalBus.gd` script. Name it `SignalBus` so it can be accessed globally.

### Step 3: Emitting Signals from Player Node

Create a player script `Player.gd`:

```gdscript
extends KinematicBody2D

# Assuming you have a method to collect items
func collect_item(item_name: String):
    # Emit the signal through SignalBus
    SignalBus.emit_signal("item_collected", item_name)
    print("Collected item: ", item_name)
```

### Step 4: Listening to Signals in UI Node

Create a UI script `UI.gd`:

```gdscript
extends Control

var score: int = 0

func _ready():
    # Connect to the item_collected signal from SignalBus
    SignalBus.connect("item_collected", self, "_on_item_collected")

# Define the callback function to handle the signal
func _on_item_collected(item_name: String):
    # Update the score or perform any UI update
    score += 1
    $ScoreLabel.text = "Score: " + str(score)
    print("Item collected: ", item_name, " New score: ", score)
```

In this example:
- The `SignalBus` script defines a signal `item_collected`.
- The `Player` script emits the `item_collected` signal whenever an item is collected.
- The `UI` script listens for the `item_collected` signal and updates the score.

By using the SignalBus, the player and UI scripts are decoupled; they do not need to know about each other directly. This makes your game architecture cleaner and more modular.

## Additional Tips

- You can define multiple signals in the SignalBus for different events.
- Other nodes, such as enemies, power-ups, or any other game entities, can also use the SignalBus to emit and listen to signals.
- Make sure to handle the case when nodes are added or removed dynamically by connecting and disconnecting signals appropriately.

By following this pattern, you ensure that your game's architecture remains scalable and maintainable.