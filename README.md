The-Bunker-Fell: Gold Collectable System

Project Component: Implementation of a persistent Gold Collectable System
Environment: Godot Engine 4.3 (GDScript)
Project Type: SIG-Game Official Project 3

Expectation:

Gold items are spawned correctly via the rooms.json definition.

The Player collides with and collects the gold.

The gold object is immediately deleted from the scene.

The Player's internal score counter is updated and displayed on the UI.

[[Link to Video Demonstration with Audio Explanation]](https://docs.google.com/videos/d/15K_VQC2wIMwpkRtvsUmuSea9aNCjG9Jti9PAOPJfI7I/edit?usp=sharing)

Step-by-Step Technical Implementation

The Gold Collectable System was designed to be modular and scalable, leveraging Godot's built-in physics and signaling architecture. The core feature is integrating the collectable into the existing rooms.json spawning pipeline.

1. The Gold.tscn Scene Structure
  
    The gold collectable is defined by a dedicated scene to ensure reusability and encapsulation of its logic. The scene is a root Area3D with a CollisionShape3D and a MeshInstance3D visual.

2. GDScript Logic (Gold.gd)
  
    The script attached to the Gold (Area3D) root node handles the collection event.

3. Score Management and Player Integration

    The collected gold value must be tracked and persisted.
    
    Global Score: A Singleton/Autoload script (e.g., GameManager.gd) is used to store the persistent gold_count. This script listens for the "collected_gold" signal emitted by the Gold.gd scene.
    
    UI Update: The GameManager.gd updates the internal gold_count and immediately signals the main HUD/UI scene to refresh the on-screen display.

4. Integration with rooms.json
  
    1. The system utilizes the existing room generation pipeline to dynamically place gold items.
  
        Scene Reference: The path to the Gold.tscn scene is registered in the game's asset loading system.
  
    2. JSON Definition: In the rooms.json file, new entries are created using the defined structure:
  
            {
              "type": "Gold", 
              "position": [5.0, 1.0, -3.0], 
              "rotation": [0.0, 0.0, 0.0],
              "custom_vars": {
                "value": 5 
              }
            }
  
  
    3. Spawning Logic: The room loader script parses this JSON entry.
        
            It uses type: "Gold" to load the Gold.tscn scene.
            
            It applies the specified position and rotation.
            
            It checks for custom_vars and, if present, calls the initialize() method on the new Gold node: gold_node.initialize(vars.custom_vars). This sets the gold_value to 5 for that specific instance
