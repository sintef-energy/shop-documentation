## Printing marginal cost curves
Marginal cost curves can be printed to an xml file with the command:
```
Command: print mc_curves <filename.xml> (<start_t> <end_t>) [/<options>]
```

|<option>|Comment|
|---|---|
|up|Curves are also printed for combinations where generators are added|
|down|Curves are also printed for combinations where generators are removed|
|pq|Only production and discharge are printed in the tables. The plant summary tables are also omitted|
|mod|Will make curves that are originally non-convex convex|

Note that it is not possible to give the start (or end) time step by itself, the time steps must be given as a pair. If they are not provided, the marginal cost curves will be printed for all time steps. Also note that any of the option combinations are valid. For instance:
```
print mc_curves mc.xml 12 24 /up /down
```
will print curves for all time steps from 12 to 24 to the file "mc.xml". Curves for combinations with added/removed generators relative to the given combination will also be printed.

Marginal cost data for Pelton plants is not printed by default. See the command [set pelton_mc_print](set_pelton_mc_print).

### Releated functionality
- [](set_resolve_mc_conflict)
- [](set_mc_pelton)
- [](set_pelton_mc_print)
- [](set_mc_ref_prod)