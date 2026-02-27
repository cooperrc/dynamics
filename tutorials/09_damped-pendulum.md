~~~
<!-- PlutoStaticHTML.Begin -->
<!--
    # This information is used for caching.
    [PlutoStaticHTML.State]
    input_sha = "a68c0d9f135689c0dbac791513579f10422334c4c5fef0a0d39eed3f95fbd71f"
    julia_version = "1.12.4"
-->

<div class="markdown"><h1 id="Rigid-body-motion-with-non-conservative-forces">Rigid body motion with <em>non-conservative</em> forces</h1><p><a href="https://www.youtube.com/watch?v=nve8QSUjixs &quot;Rigid body motion with _non-conservative_ forces - click to watch&quot;"><img alt="Rigid body motion with _non-conservative_ forces" src="https://img.youtube.com/vi/nve8QSUjixs/0.jpg"/></a></p><p>In this video, we go through the definitions of </p><ol><li><p>Kinematics</p></li><li><p>Kinetics</p></li><li><p>Variational calculus</p></li></ol><p>to arrive at the equations of motion for a <em>compound pendulum</em> with a support that has both stiffness and damping (modeled here as a spring and damper). </p><p><img alt="Compound pendulum as a bar with spring and damper" src="https://raw.githubusercontent.com/cooperrc/dynamics/refs/heads/main/images/damped-pendulum.png"/></p><h2 id="Kinematics">Kinematics</h2><p>We determine the position of the pendulum is <span class="tex">\(q_2=[x_2,~y_2,~\theta_2] = [f_1(x,~\theta),~f_2((x,~\theta)),~\theta]\)</span></p><p>Where the generalized coordinates are <span class="tex">\(q = [x,~\theta]\)</span> so the position and orientation must be written as functions of these variables. The position of the pendulum is thus</p><p class="tex">$$\mathbf{r} = (x+\frac{L}{2}\sin\theta)\hat{i} + \frac{L}{2}\sin\theta\hat{j}$$</p><p>and velocity is</p><p class="tex">$$\mathbf{v} = (\dot{x}-\frac{L}{2}\dot{\theta}\cos\theta)\hat{i} + \frac{L}{2}\dot{\theta}\cos\theta\hat{j}$$</p><h2 id="Lagrangian-of-damped-pendulum">Lagrangian of damped pendulum</h2><p>The form of the Lagrangian (action, <span class="tex">\(L=T-V\)</span>) then results from the position and velocity definitions, </p><p class="tex">$$L = \frac{1}{2}m\left(\dot{x}^2+\frac{L^2}{3}\dot{\theta}^2 + 2L\dot{x}\dot{\theta}\cos\theta\right) - \frac{1}{2}kx^2 - mg\frac{L}{2}(1 - \cos\theta)$$</p></div>

<pre class='language-julia'><code class='language-julia'>using Symbolics, Plots, OrdinaryDiffEq, Latexify, ModelingToolkit</code></pre>


<pre class='language-julia'><code class='language-julia'>begin
@independent_variables t
@parameters m=0.4 L = 1 g = 9.81 k = 0.1 b = 0.2
@variables θ(t), x(t)    # θ and x are functions of time
D = Differential(t) # time derivative operator

# Define kinetic and potential energy
T = 1/2 * m *(D(x)^2 + L^2 *D(θ)^2/3 + 2*L*D(x)*D(θ)*cos(θ))
V = 1/2*k*x^2 + m * g * L/2 * (1 - cos(θ))

# Lagrangian
Lag = T - V
end</code></pre>
<p class="tex">$$\begin{equation}
 - 0.5 ~ \left( x\left( t \right) \right)^{2} ~ k - \frac{1}{2} ~ L ~ g ~ m ~ \left( 1 - \cos\left( \theta\left( t \right) \right) \right) + 0.5 ~ \left( \left( \frac{\mathrm{d} ~ x\left( t \right)}{\mathrm{d}t} \right)^{2} + \frac{1}{3} ~ \left( \frac{\mathrm{d} ~ \theta\left( t \right)}{\mathrm{d}t} \right)^{2} ~ L^{2} + 2 ~ L ~ \frac{\mathrm{d} ~ x\left( t \right)}{\mathrm{d}t} ~ \frac{\mathrm{d} ~ \theta\left( t \right)}{\mathrm{d}t} ~ \cos\left( \theta\left( t \right) \right) \right) ~ m
\end{equation}$$</p>

<pre class='language-julia'><code class='language-julia'>begin
dL_dθdot = Symbolics.derivative(Lag, D(θ))
dL_dθ    = Symbolics.derivative(Lag, θ)

EL_equation_θ = expand_derivatives(D(dL_dθdot) - dL_dθ)
simplify(EL_equation_θ)
end</code></pre>
<p class="tex">$$\begin{equation}
\frac{1}{2} ~ L ~ g ~ m ~ \sin\left( \theta\left( t \right) \right) + L ~ m ~ \frac{\mathrm{d} ~ x\left( t \right)}{\mathrm{d}t} ~ \frac{\mathrm{d} ~ \theta\left( t \right)}{\mathrm{d}t} ~ \sin\left( \theta\left( t \right) \right) + 0.5 ~ \left( \frac{2}{3} ~ L^{2} ~ \frac{\mathrm{d}^{2} ~ \theta\left( t \right)}{\mathrm{d}t^{2}} + 2 ~ L ~ \frac{\mathrm{d}^{2} ~ x\left( t \right)}{\mathrm{d}t^{2}} ~ \cos\left( \theta\left( t \right) \right) - 2 ~ L ~ \frac{\mathrm{d} ~ x\left( t \right)}{\mathrm{d}t} ~ \frac{\mathrm{d} ~ \theta\left( t \right)}{\mathrm{d}t} ~ \sin\left( \theta\left( t \right) \right) \right) ~ m
\end{equation}$$</p>

<pre class='language-julia'><code class='language-julia'>begin
dL_dxdot = Symbolics.derivative(Lag, D(x))
dL_dx    = Symbolics.derivative(Lag, x)

EL_equation_x = expand_derivatives(D(dL_dxdot) - dL_dx)
simplify(EL_equation_x)
end</code></pre>
<p class="tex">$$\begin{equation}
k ~ x\left( t \right) + 0.5 ~ \left( 2 ~ \frac{\mathrm{d}^{2} ~ x\left( t \right)}{\mathrm{d}t^{2}} + 2 ~ L ~ \frac{\mathrm{d}^{2} ~ \theta\left( t \right)}{\mathrm{d}t^{2}} ~ \cos\left( \theta\left( t \right) \right) - 2 ~ \left( \frac{\mathrm{d} ~ \theta\left( t \right)}{\mathrm{d}t} \right)^{2} ~ L ~ \sin\left( \theta\left( t \right) \right) \right) ~ m
\end{equation}$$</p>

<pre class='language-julia'><code class='language-julia'>eqs_lagrange = [EL_equation_x ~ -b*D(x); EL_equation_θ ~ 0]</code></pre>
<p class="tex">$$\begin{align}
k ~ x\left( t \right) + 0.5 ~ \left( 2 ~ \frac{\mathrm{d}^{2} ~ x\left( t \right)}{\mathrm{d}t^{2}} + 2 ~ L ~ \frac{\mathrm{d}^{2} ~ \theta\left( t \right)}{\mathrm{d}t^{2}} ~ \cos\left( \theta\left( t \right) \right) - 2 ~ \left( \frac{\mathrm{d} ~ \theta\left( t \right)}{\mathrm{d}t} \right)^{2} ~ L ~ \sin\left( \theta\left( t \right) \right) \right) ~ m &amp;=  - b ~ \frac{\mathrm{d} ~ x\left( t \right)}{\mathrm{d}t} \\
\frac{1}{2} ~ L ~ g ~ m ~ \sin\left( \theta\left( t \right) \right) + L ~ m ~ \frac{\mathrm{d} ~ x\left( t \right)}{\mathrm{d}t} ~ \frac{\mathrm{d} ~ \theta\left( t \right)}{\mathrm{d}t} ~ \sin\left( \theta\left( t \right) \right) + 0.5 ~ \left( \frac{2}{3} ~ L^{2} ~ \frac{\mathrm{d}^{2} ~ \theta\left( t \right)}{\mathrm{d}t^{2}} + 2 ~ L ~ \frac{\mathrm{d}^{2} ~ x\left( t \right)}{\mathrm{d}t^{2}} ~ \cos\left( \theta\left( t \right) \right) - 2 ~ L ~ \frac{\mathrm{d} ~ x\left( t \right)}{\mathrm{d}t} ~ \frac{\mathrm{d} ~ \theta\left( t \right)}{\mathrm{d}t} ~ \sin\left( \theta\left( t \right) \right) \right) ~ m &amp;= 0
\end{align}$$</p>
<div class='manifest-versions'>
<p>Built with Julia 1.12.4 and</p>
Latexify 0.16.10<br>
ModelingToolkit 11.10.0<br>
OrdinaryDiffEq 6.108.0<br>
Plots 1.41.5<br>
Symbolics 7.13.0
</div>

<!-- PlutoStaticHTML.End -->
~~~

_To run this tutorial locally, download [this file](/tutorials/09\_damped-pendulum.jl) and open it with
[Pluto.jl](https://plutojl.org)._
