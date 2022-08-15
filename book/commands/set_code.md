---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.13.8
kernelspec:
  display_name: 'Python 3'
  name: python3
---

(set_code)=
# set code
Set the iteration mode that will be used in the next iterations of SHOP. Full iterations decide unit commitment and should be performed first. Switching to incremental iterations after some full iterations will refine the solution for the final unit commitment decisions

|   |   |
|---|---|
|Options|/head, /full, /incremental, /junction, /on, /off|
|License|SHOP_OPEN|
|Release version|13.0.0.a|

```{contents}
:local:
:depth: 1
```

## Defining the model type
In the optimization, either full or incremental mode can be used. In the full mode, the complete plant will be described from minimum to the maximum plant output, and the unit commitment will be set. If start and stop costs are included, they will be taken into account in the full mode. In the incremental mode the optimal unit combination at the current power production level will be used, and the plant description will be based on a successive linearization of this model.

At least one full iteration must be performed before switching to incremental iterations, and two full iterations are necessary to activate the head optimization. Since the solution in the full mode is used as operation point in the incremental mode, it is important to let this solution converge before switching models. General guiding lines are difficult to implement as the amount of mismatch tolerance depends on the circumstances: case data, problem size, constraints, degree of flexibility in the model and so on.

To switch between models and turn on/off head optimization, the command below is used:
```
Command: set code /<option1> (/<option2>)
```

|<option1>|<option2>|Comment|
|---|---|---|
|full||Full mode|
|incremental||Incremental mode|
|head|||
||on|Turn head optimization on|
||off|Turn head optimization off|

If command not set: Full mode with head optimization turned on will be used.



