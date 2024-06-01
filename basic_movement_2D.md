1. Go to Project > Project settings, open Imput map tab and add key mapping:

<img src="https://github.com/miljon/Godot-Tutorials/assets/8971419/c03e4748-b8ec-4257-9008-664c78db1bf5" width="50%" height="50%">


2. Create CharacterBody2D and attach script to it. 


```GDScript
extends CharacterBody2D

@export var speed = 400

func get_input():
	var input_direction = Input.get_vector("move_left", "move_right", "move_up", "move_down")
	velocity = input_direction * speed
	
func _physics_process(delta):
	get_input()
	move_and_slide()
```


---
Issues:
1. Make sure to add camera nad sprite under your CharacterBody2D

<img src="https://github.com/miljon/Godot-Tutorials/assets/8971419/9b3cceb0-f964-4c42-9ec3-12fc1eed37ed" width="30%" height="30%">
