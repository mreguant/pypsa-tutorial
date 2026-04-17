---
layout: default
title: The 3-Bus Network
nav_order: 2
has_children: false
---

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

```python
import pypsa
n = pypsa.Network()

# Add three buses
for i in range(3):
    n.add("Bus", f"My Bus {i}", v_nom=380)

# Add a generator at Bus 0
n.add("Generator", "My Gen", bus="My Bus 0", p_set=100, control="Slack")

# Add a load at Bus 2
n.add("Load", "My Load", bus="My Bus 2", p_set=80)

# This finds the least-cost dispatch to satisfy the load
n.optimize(solver_name='glpk')

# What is the power flow on the lines?
print(n.lines_t.p0)

# What are the marginal prices (LMPs) at the buses?
print(n.buses_t.lambdas)
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
