+++
title = "Godot autoload script nil"
tags = [
    "godot",
    "game development",
]
date = 2025-03-25
+++

I wanted to make a script in my Godot project autoload, so I added it to the `Project Settings` > `Globals` > `Autoload`.

{{< image src=autoload-settings.png alt="Autoload settings" >}}

But when I ran my project the value was always `nil`.

{{< image src=autoload-script-nil.png alt="Autoload script nil" >}}

According to the [documentation](https://docs.godotengine.org/en/stable/tutorials/scripting/singletons_autoload.html#autoload), a node should automatically be created for this script, but that does not seem to happen when I check the remote scene tree during the run.

{{< image src=scene-tree-no-autoload.png alt="Scene tree without autoload" >}}

When I updated the autoloaded script to extend Node it worked.

``` gdscript
extends Node

var amount: int = 0
...
```

{{< image src=scene-tree-autoload.png alt="Scene tree autoload" >}}