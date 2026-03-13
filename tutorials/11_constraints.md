~~~
<!-- PlutoStaticHTML.Begin -->
<!--
    # This information is used for caching.
    [PlutoStaticHTML.State]
    input_sha = "ef8b3e4a319e384ca23e48dffb3f9d9dea6e3887cf2f71357af6e44516b664d2"
    julia_version = "1.12.4"
-->

<div class="markdown"><h1 id="Creating-Kinematic-Constraints">Creating Kinematic Constraints</h1><p><a href="https://www.youtube.com/watch?v=mZIOv7meGJo &quot;Creating Kinematic Constraints - click to watch&quot;"><img alt="Creating Kinematic Constraints" src="https://img.youtube.com/vi/mZIOv7meGJo/0.jpg"/></a></p></div>


<div class="markdown"><h2 id="1.-Introduction">1. Introduction</h2><p>This module transitions from analytical dynamics to computational dynamics. Previously, we derived equations of motion using variational calculus and solved initial value problems numerically. Now we examine how computers represent rigid-body systems through coordinates, constraints, and kinematics.</p><h2 id="2.-Multi‑Body-Kinematics-Overview">2. Multi‑Body Kinematics Overview</h2><p>Computational dynamics focuses on rigid-body motion described through generalized coordinates. For a body:</p><ul><li><p>Position: <span class="tex">\(R_1 = [x_1, y_1]^T\)</span></p></li><li><p>Orientation: <span class="tex">\(\theta_1\)</span></p></li><li><p>Body-fixed axes: <span class="tex">\(\mathbf{i}_1, \mathbf{j}_1\)</span></p></li></ul></div>


<div class="markdown"><p>Any point on the body can be written: <span class="tex">\(R = R_1 + x\,\mathbf{i}_1 + y\,\mathbf{j}_1\)</span></p><h2 id="3.-Door-Example-Setup">3. Door Example Setup</h2><p>A door is modeled as a rigid body with center of mass at <span class="tex">\(R_1\)</span> and rotation <span class="tex">\(\theta_1\)</span>. A point such as the handle is located using offsets <span class="tex">\((x_H, y_H)\)</span>: <span class="tex">\(R_H = R_1 + x_H\,\mathbf{i}_1 + y_H\,\mathbf{j}_1\)</span></p><p>The hinge point, offset by door width <span class="tex">\(W\)</span> and thickness <span class="tex">\(T\)</span>: <span class="tex">\(R_{hinge}^{(1)} = R_1 - \frac{W}{2}\mathbf{i}_1 + \frac{T}{2}\mathbf{j}_1\)</span></p><p>The same hinge point expressed in ground coordinates: <span class="tex">\(R_{hinge}^{(0)} = R_0 + M_x\mathbf{i}_0 + M_y\mathbf{j}_0\)</span></p><p>Setting these equal produces constraint equations.</p></div>


<div class="markdown"><h2 id="4.-Rotation-Matrix">4. Rotation Matrix</h2><p>To relate body-fixed and ground-fixed frames: <span class="tex">\(A(\theta_1) = \begin{bmatrix} \cos\theta_1 &amp; -\sin\theta_1 \\ \sin\theta_1 &amp; \cos\theta_1 \end{bmatrix}\)</span></p><p>Thus: <span class="tex">\(\mathbf{i}_1 = A(\theta_1)\mathbf{i}_0, \qquad \mathbf{j}_1 = A(\theta_1)\mathbf{j}_0\)</span></p><p>The transpose equals the inverse: <span class="tex">\(A^{-1}(\theta_1) = A^T(\theta_1)\)</span></p><h2 id="5.-Constraint-Equations">5. Constraint Equations</h2><p>The hinge must satisfy: <span class="tex">\(C(q) = R_1 + A(\theta_1) r_{hinge}^{(1)} - R_{hinge}^{(0)} = 0\)</span> This expands into two scalar equations (x and y constraints).</p><h2 id="6.-Velocity-Constraints">6. Velocity Constraints</h2><p>Taking the time derivative: <span class="tex">\(\frac{d}{dt}C(q,t) = 0\)</span> Applying the chain rule: <span class="tex">\(C_q \, \dot{q} = -C_t\)</span> where <span class="tex">\(q = [x_1, y_1,\theta_1]^T\)</span>.</p><p>Differentiating the rotation matrix term yields: <span class="tex">\(\frac{d}{dt}A(\theta_1) = A(\theta_1)\begin{bmatrix}0 &amp; -\dot{\theta}_1 \ \dot{\theta}_1 &amp; 0\end{bmatrix}\)</span></p></div>


<div class="markdown"><h2 id="7.-Adding-a-Time‑Varying-Constraint">7. Adding a Time‑Varying Constraint</h2><p>If the door is pushed upward at constant speed: <span class="tex">\(R_{1y} = 2t \quad \Rightarrow \quad \dot{R}_{1y} = 2\)</span> This creates a third constraint to be incorporated into the system.</p><h2 id="8.-Solving-the-System">8. Solving the System</h2><p>At each timestep:</p><ol><li><p><strong>Position solve:</strong> find <span class="tex">\(q\)</span> such that <span class="tex">\(C(q,t)=0\)</span></p></li><li><p><strong>Velocity solve:</strong> solve linear system <span class="tex">\(C_q \dot{q} = -C_t\)</span></p></li></ol><p>These yield consistent positions and velocities that satisfy all kinematic constraints.</p><h2 id="Wrapping-Up">Wrapping Up</h2><p>This framework provides:</p><ul><li><p>Rigorous description of rigid‑body motion</p></li><li><p>Consistent enforcement of physical constraints</p></li><li><p>Linear velocity equations even for nonlinear positions</p></li><li><p>A foundation for forming differential‑algebraic equations used in multi‑body simulation</p></li></ul><p>Computational dynamics transforms geometry, motion, and constraint information into forms solvable by numerical tools such as Julia, MATLAB, or Python.</p></div>
<div class='manifest-versions'>
<p>Built with Julia 1.12.4 and</p>

</div>

<!-- PlutoStaticHTML.End -->
~~~