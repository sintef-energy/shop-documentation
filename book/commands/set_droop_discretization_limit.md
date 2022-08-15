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

(set_droop_discretization_limit)=
# set droop_discretization_limit
Fixes the droop value of all units that have a resulting droop below the specified limit. The droop is fixed to the closest legal discrete value below the given limit, which is the closest integer below unless the unit has a defined discrete_droop_values attribute

|   |   |
|---|---|
|Options|nan|
|License|SHOP_DISCRETE_DROOP|
|Release version|14.0.0.9|

```{contents}
:local:
:depth: 1
```





