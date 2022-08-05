---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.13.8
kernelspec:
  display_name: 'Python 3.7.9 64-bit (''SHOP_env'': conda)'
  name: python3
---

+++

(water-value-descriptions)=
# Water value descriptions
## Introduction
The valuation of the water saved in the reserovirs by the end of the optimization period is a crucial input to SHOP that will be an important factor in determining the optimal operation of the hydropower system. It is possible to define the water value description for each reservoir in several ways. The two main categories of water value descriptions are independent and coupled water values, each with its own sub-categories explained below.

+++

## Independent water values
The value of the water left in a specific [](reservoir) only depends on the end volume in that [](reservoir) when an independent water value function is used.

### Constant water values
The simplest water value function available is to define a constant water value for each reservoir. The constant water value can be specified in either €/MWh or €/Mm$^3$ by using the following attributes:

|Object type|Attribute|Data type|Unit|I/O|Description|
|:-|:-|:-|:-|:-|:-|
|[](reservoir)|[](reservoir:energy_value_input)|double|€/MWh|Input|Constant local water value defined in €/MWh relative to the downstream plant|
|[](reservoir)|[](reservoir:water_value_input)|xy_array|x:Mm$^3$, y:€/Mm$^3$|Input|Constant global water value defined in €/Mm$^3$|

Note that the data type of the **[](reservoir:water_value_input)** attribute is a list of xy tables even though it can be used to define a constant water value. This is because the same attribute is also used for the other water value descriptions that require a more complex data structure. By giving in a single xy table with a single point with the water value in the y column, SHOP will interpret it as a constant independent water value. The x value is not used in the case of a constant water value. The reference value for the xy table is also not used in this case.

Unlike the water_value_input attribute, the **[](reservoir:energy_value_input)** is defined as a single double value since it is not possible to specify a more complex water value description in €/MWh. The value is considered to be the local energy value relative to the reservoir below the first plant found under the reservoir. If a reservoir is directly connected to several downstream plants, the plant with the best power-to-discharge ratio is used. The energy_value_input is converted to a global water value in €/Mm$^3$ by SHOP so that it can be used in the optimization problem. The conversion is based on the best point production of the plant below the reservoir given the initial head value, and the global water value of the reservoir below the plant is added to create the global calculated water value. The conversion factor and calculated global water value can be inspected by retreiving the following attributes:

|Object type|Attribute|Data type|Unit|I/O|Description|
|:-|:-|:-|:-|:-|:-|
|[](reservoir)|[](reservoir:energy_conversion_factor)|double|MWh/Mm$^3$|Output|Conversion factor relative to the downstream plant used to convert between €/MWh and €/Mm$^3$.|
|[](reservoir)|[](reservoir:calc_global_water_value)|double|€/Mm$^3$|Output|The result after convering energy_value_input (if specified) into a global water value.|

If both [](reservoir:water_value_input) and [](reservoir:energy_value_input) are specified for the same [](reservoir), the [](reservoir:water_value_input) is used.

### Water value tables
A more refined water value description can be defined by having a piece-wise constant water value based on the final volume of the reservoir. A water value table can be specified by using the same **[](reservoir:water_value_input)** attribute on the reservoir as in the constant water value case, but with more than one point on in the xy table. The x values represent the volume level break points and the y values describe the marginal water value for the volume segments. The first x value should be 0 (empty reservoir), and the last x value should not be greater than the maximal reservoir volume. The y values should always be decreasing so that the marginal water value is higher for low reservoir levels compared to high reservoir levels.

An example of a water value table for a reservoir of 100 Mm$^3$ is illustrated below. The water value table input in the figure on the left shows the pairs of (volume, marginal water value) points as orange dots that define the piece-wise constant water value function. Note that the final point is given for 90 Mm$^3$ and 5 €/Mm$^3$, and that the last dashed segment which extends to the maximal volume of the reservoir is automatically constructed by SHOP. The total end value of the reservoir as a function of the end volume is shown in the figure on the right side. The piece-wise linear function is the integral of the step curve on the left, and is used in the objective function of the optimization problem.

+++

| | |
|:-------------------------:|:-------------------------:|
|![](./img/water_value_table_marginal.png)|![](./img/water_value_table_total.png)|

+++

Note that the xy_array attribute [](reservoir:water_value_input) should only contain a single xy table in the case of a piece-wise constant water value description. The reference value of the xy table is not used in this case. There is no piece-wise description defined in €/MWh.

+++

## Coupled water values
As the reservoirs in a watercourse are physically and temporaly coupled, the future expected value of the water saved at the end of the optimization horizon depends on the state of all reservoir levels. In contrast to the independent water value descriptions, a single multi-dimensional water value function dependent on the end volume of all coupled reservoirs can be used instead for a more refined formulation. This function is represented as a set of inequality constraints, usually created by a Benders decomposition scheme in the long-term models, often referred to as "cuts".

### Standard cut description
To define a cut description for the system, a **[](cut_group)** object must be created and connected to all reservoirs part of the cut coupling. If several separate watercourses with different cut descriptions are used in the SHOP run, a [](cut_group) should be created for each coupled set of reservoirs. The constant terms (right-hand side) of the linear cut constraints are speceified on the [](cut_group) object through the **rhs** attribute:

|Object type|Attribute|Data type|Unit|I/O|Description|
|:-|:-|:-|:-|:-|:-|
|[](cut_group)|[](cut_group:rhs)|xy_array|x:No unit, y:€|Input|The future expected income of the system if all reservoir end volumes are locked to the cut reference volumes.|

The sign convention of the rhs attribute in SHOP is defined in terms of income, and so the y values should generally be positive values. Negative values in the rhs is a sign of penalties in the long-term model used to create the cuts. The rhs attribute is an xy_array like water_value_input, and only a single xy table should be used in for the standard cut description. The y values represent the constant for each cut constraint, while the x values are not used.

The water value cut coefficients for each reservoir in the cut description is again defined by the **water_value_input** attribute on the reservoir object, and a single xy table should be used in the standard cut description. The x values represent the reference volumes of the reservoir when the specific cut constraint was created, and the y values are the water value cut coefficients. The reference volumes are sometimes given as a percentage filling level of the reservoir in the long-term model coupling files, but should always be given in Mm$^3$ when interacting with the water_value_input attribute. The reference volumes are in some cases not reported from the long-term model, which means they are already baked into the rhs values of the cuts and the x values of the water_value_input attribute should be set to zero.

The optimized end value of the cut description of the system is saved to the **end_value** attribute on the [](cut_group) object. The binding cut constraint after optimization is also saved to the **[](cut_group:binding_cut_up)** attribute:

|Object type|Attribute|Data type|Unit|I/O|Description|
|:-|:-|:-|:-|:-|:-|
|[](cut_group)|[](cut_group:end_value)|txy|€|Output|The future expected income of the system, only the value for the last time step is used.|
|[](cut_group)|[](cut_group:binding_cut_up)|txy|No unit|Output|The zero-indexed number of the cut constraint that is binding after optimization.|

The end_value and [](cut_group:binding_cut_up) attributes are defined as TXYs instead of double values to allow stochastic SHOP runs to save the results from each scenario.

### Price dependent cut description
The standard cut description is independent of the market price. The extended cut functionality from ProdRisk incorporates a final price dimension into the cuts to better account for price uncertainty. The **[](cut_group)** object is still used to define the cut description, and the constant part of the cut constraints are specified by the **[](cut_group:rhs)** attribute. The **[](reservoir:water_value_input)** attribute on the reservoir object is also used to hold the water value cut coefficients in the price dependent case. The price dimension is incorporated by supplying a xy table for each price level in the rhs and water_value_input attributes, where the reference value for each xy table should be the price level. The reference values of the xy tables are not used in the other water value descriptions. The average weekly price in the last week of the SHOP horizon is used to find the cuts with the closest price above and below the average SHOP price. The average price is also used to calculate interpolation weights for these two sets of cuts when they are added to the objective function. The cuts with reference prices that are further away from the average SHOP price are not used in the SHOP run. Another attribute, **[](cut_group:binding_cut_down)**, is used to keep track of the binding cut in the set of cuts with the lower price after optimization:

|Object type|Attribute|Data type|Unit|I/O|Description|
|:-|:-|:-|:-|:-|:-|
|[](cut_group)|[](cut_group:binding_cut_down)|txy|No unit|Output|The zero-indexed number of the cut constraint that is binding after optimization in the set of cuts with the closest price lower than the average SHOP price.|

The **[](cut_group:binding_cut_up)** is similarly used for the set of cuts with the closest price above the average SHOP price in the extended cut functionality.

In addition to incorporating the market price into the cut description, cut coefficients for the inflow series objects in ProdRisk are also added in the extended cut functionality. Even though there are no variables related to the inflow in SHOP, **[](inflow_series)** objects corresponding to the inflow series in ProdRisk should be created to incorporate the inflow correction to the cuts. The **[](inflow_series:cut_coeffs)** attribute for the inflow_series are defined as:

|Object type|Attribute|Data type|Unit|I/O|Description|
|:-|:-|:-|:-|:-|:-|
|[](inflow_series)|[](inflow_series:cut_coeffs)|xy_array|x:Mm$^3$, y:€/Mm$^3$|Input|The cut coefficients of the inflow series in ProdRisk.|

The x values of the [](inflow_series:cut_coeffs) are reference values of the weekly average inflow in the ProdRisk scenario and week that created the specific cut. This reference values are omitted from the cut file format and already incorporated into the rhs of the cuts. The y values are the cut coefficients realted to the inflow, and are multiplied with the weekly inflow of the reservoirs connected to the inflow_series in SHOP. Note that it is the sum inflow to the connected reservoirs in the last week of the optimization horizon that is used. If the horizon is shorter than 168 hours, the sum inflow is scaled to accomodate this.

+++

## Value of water in transit
If a time delay is applied to a [](plant) or [](gate) in SHOP by setting the **time_delay** attribute, the water discharged close to the end of the optimization horizon will be in transit between reservoirs when the end value of the reservoirs is summed up by the objective function. The value of the delayed water must therefore be added separately to the objective function since it will not be captured by the end volume in any reservoir. The assigned value is based on the water value description in the reservoir that will eventually receive the delayed water. If a constant energy or water value is used, this value will be used to evaluate the delayed water. If the downstream reservoir has a water value table description, the water value of the delayed water is not know before the optimization is over. The marginal water value in the first step of the water value table is therefore used to estimate the value of the time delayed water. However, if the command *[](set_delay_valuation) /middle* is given (see also the **[](global_settings:delay_valuation_mode)** attribute on the **[](global_settings)** object), the marginal water value of the middle segment is used instead. 

Finding a reasonable water value to use for the water in transit is more complicated when cuts are used. In this case, the reservoir cut coefficient for the binding cut(s) given the initial reservoir level of all reservoirs in the cut group is used as a water value. 

Note that water in transit from [plants](plant) and [gates](gate) that have a specified **discharge_fee** is not evaluated in the ways mentioned above. In these cases, only the value given in the discharge_fee is used.

+++

## Related examples

Examples showing the use of different water value descriptions are available:

- [Individual water values example](individual-water-values) (Constant energy values, mix of constant energy values and constant water values, water value tables)
- [Coupled water values](cut-description) (Standard cut description and price dependent cut description)
