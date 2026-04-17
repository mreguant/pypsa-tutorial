<!-- ---
layout: default
title: The 3-Bus Network
nav_order: 2
has_children: false
--- -->

# Lesson 1: Building a 3-Bus Network
{: .no_toc }

In this lesson, we will create a simple triangular network to see how PyPSA handles power flow between multiple nodes.
{: .fs-6 .fw-300 }

---

## The Goal
We want to simulate a small system with:
1. One Slack Bus (Generator)
2. One Load Bus (Demand)
3. One intermediate Bus

### Static Code Snippet
To define this in PyPSA, you would use the following logic:


[Open In Colab](https://colab.research.google.com/github/mreguant/pypsa-tutorial/blob/main/notebooks/first_example.ipynb){: .btn .btn-blue }

```python
# 1. Create the Network
n = pypsa.Network()
n.add("Bus", "Market", v_nom=380)

# 2. Add Generators with different costs and capacities
# Costs are in EUR/MWh, capacity in MW
n.add("Generator", "Wind", bus="Market", p_nom=100, marginal_cost=0)
n.add("Generator", "Coal", bus="Market", p_nom=100, marginal_cost=30)
n.add("Generator", "Gas",  bus="Market", p_nom=100, marginal_cost=60)

# 3. Add a Load (Demand)
n.add("Load", "Demand", bus="Market", p_set=150)

# 4. Solve using 'highs' (a modern, fast open-source solver)
n.sanitize()
n.optimize(solver_name='highs')

# 5. Display Results
print("\n--- Dispatch Results (MW) ---")
print(n.generators_t.p)
```

---


## Reviewing the Results
Once the power flow is solved, PyPSA stores the results in the `n.lines_t` and `n.buses_t` dataframes. For a static look, we focus on the **Active Power (p0)**.

In our 3-bus example, you would see the power flow split between the two available paths from the generator to the load, determined by the impedance of the lines.

{: .highlight }
In a real scenario, you would run `n.pf()` to calculate these values before plotting.

---

## What's Next?
Now that you've built a basic network, we'll look at how to add **Time Series** data (Snapshots) to simulate a full day of operation.

[Next Lesson: Adding Time Series →]({{ site.baseurl }}/lessons/02-time-series){: .btn .btn-purple }
