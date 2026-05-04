# Full Kinematic Equations for piston-crank

[![Full Kinematic Equations for piston-crank](https://img.youtube.com/vi/UsqBALdUCPQ/0.jpg)](https://www.youtube.com/watch?v=UsqBALdUCPQ "Full Kinematic Equations for piston-crank - click to watch")


In this video, we revisit the piston-crank kinematic equations to define
the kinematic constraints on acceleration,  

$\mathbf{C}_\mathbf{q} \ddot{\mathbf{q}} = -\mathbf{C}_{tt}-(\mathbf{C}_\mathbf{q} \dot{\mathbf{q}})_\mathbf{q}\dot{\mathbf{q}}- 2 \mathbf{C}_{\mathbf{q}t}\dot{\mathbf{q}}$

where subscripts denote partial derivatives i.e. $\mathbf{C}_\mathbf{q}
= \frac{\partial \mathbf{C}}{\partial \mathbf{q}}$. 

These equations are especially important as we introduce degrees of
freedom into multibody dynamic systems. Each degree of freedom
represents a second order differential equation where, 

$\ddot{\mathbf{q}} = f(\mathbf{q},~\dot{\mathbf{q}},~t)$

because forces are usually a function of time, dislacement, or velocity
and $F = ma$. 


_To run this tutorial locally, download [this file](/tutorials/15\_piston-full.jl) and open it with
[Pluto.jl](https://plutojl.org)._
