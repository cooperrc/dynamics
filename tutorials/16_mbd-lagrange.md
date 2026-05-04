# Lagrange equations for reaction forces

[![Lagrange equations for reaction forces](https://img.youtube.com/vi/tUlfvIGkGn0/0.jpg)](https://www.youtube.com/watch?v=tUlfvIGkGn0 "Lagrange equations for reaction forces - click to watch")

In this video, we build the full set of differential algebraic equations
needed to model multibody dynamics (MBD) systems. The general approach
has 
- $n_b$ number of moving or fixed parts
- $3\times n_b$ generalized coordinates (in planar motion)
- $n_c$  number of constraints
- a mass matrix, $\mathbf{M}$ that is $3 n_b\times 3 n_b$
- constraint Jacobian, $\mathbf{C}_{\mathbf{q}}$ that is $n_c \times
  n_b$
- externally applied forces from springs, gravity, dampers, motors, etc.
  $\mathbf{Q}_e$ that is $3 n_b$ long
- reaction constraints, $\mathbf{Q}_d$ that is $n_c$ long (every
  constraint needs a reaction force to maintain the constraint)

The set of equations leaves two unknown vectors in the left-hand side
equation, 

- the acceleration of each generalized coordinate $\ddot{\mathbd{q}}$
- the reaction forces that maintain constraints, $\lambda$

By solving these equations at each time step, we can maintain
constraints and calculate the following velocity and position of the
multibody system. 


_To run this tutorial locally, download [this file](/tutorials/16\_mbd-lagrange.jl) and open it with
[Pluto.jl](https://plutojl.org)._
