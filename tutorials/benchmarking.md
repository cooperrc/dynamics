~~~
<!-- PlutoStaticHTML.Begin -->
<!--
    # This information is used for caching.
    [PlutoStaticHTML.State]
    input_sha = "1c8856f178c5e33699171fe193e9c1ed0e156adbaa748a06cc69c23a3e449fdf"
    julia_version = "1.11.7"
-->




~~~
+++
title = "Benchmarking"
+++

~~~


<div class="markdown"><h1 id="Benchmarking">Benchmarking</h1><p>This notebook shows some <code>@benchmark</code> and <code>@code_warntype</code> output. Both outputs are shown via the <code>with_terminal</code> from <a href="https://github.com/JuliaPluto/PlutoUI.jl">PlutoUI.jl</a>, see below.</p><p>We define some function <code>double</code> and a dictionary of numbers in order to show type inference problems via <code>@code_warntype</code>:</p></div>

<pre class='language-julia'><code class='language-julia'>numbers = Dict(:one =&gt; 1f0, :two =&gt; 2.0)</code></pre>
<pre class="code-output documenter-example-output" id="var-numbers">Dict{Symbol, AbstractFloat} with 2 entries:
  :two =&gt; 2.0
  :one =&gt; 1.0</pre>

<pre class='language-julia'><code class='language-julia'>function double(mapping, key::Symbol)
    return 2 * mapping[key]
end;</code></pre>



<div class="markdown"><p>Now, the code works.</p></div>

<pre class='language-julia'><code class='language-julia'>double(numbers, :one)</code></pre>
<pre class="code-output documenter-example-output" id="var-hash458071">2.0f0</pre>

<pre class='language-julia'><code class='language-julia'>double(numbers, :two)</code></pre>
<pre class="code-output documenter-example-output" id="var-hash234834">4.0</pre>


<div class="markdown"><p>But the <code>@code_warntype</code> shows some big red warnings:</p></div>

<pre class='language-julia'><code class='language-julia'>using PlutoUI: with_terminal</code></pre>


<pre class='language-julia'><code class='language-julia'>with_terminal() do
    @code_warntype double(numbers, :one)
end</code></pre>
<pre id="plutouiterminal">MethodInstance for Main.var\"workspace#4\".double(::Dict{Symbol, AbstractFloat}, ::Symbol)
  from double(�[90mmapping�[39m, �[90mkey�[39m::�[1mSymbol�[22m)�[90m @�[39m �[90mMain.var\"workspace#4\"�[39m �[90m~/Documents/UConn/ME5180/dynamics/tutorials/�[39m�[90m�[4mbenchmarking.jl#==#3f0e2049-8597-4dac-b499-4d7a8a35978e:1�[24m�[39m
Arguments
  #self#�[36m::Core.Const(Main.var\"workspace#4\".double)�[39m
  mapping�[36m::Dict{Symbol, AbstractFloat}�[39m
  key�[36m::Symbol�[39m
Body�[91m�[1m::Any�[22m�[39m
�[90m1 ─�[39m %1 = Main.var\"workspace#4\".:*�[36m::Core.Const(*)�[39m
�[90m│  �[39m %2 = Base.getindex(mapping, key)�[91m�[1m::AbstractFloat�[22m�[39m
�[90m│  �[39m %3 = (%1)(2, %2)�[91m�[1m::Any�[22m�[39m
�[90m└──�[39m      return %3
</pre>


<div class="markdown"><p>We can fix this by forcing all elements in the dictionary to have the same type. Specifically, to we force all elements to be of type <code>Float64</code>:</p></div>

<pre class='language-julia'><code class='language-julia'>typednumbers = Dict{Symbol, Float64}(:one =&gt; 1f0, :two =&gt; 2.0)</code></pre>
<pre class="code-output documenter-example-output" id="var-typednumbers">Dict{Symbol, Float64} with 2 entries:
  :two =&gt; 2.0
  :one =&gt; 1.0</pre>


<div class="markdown"><p>This gets rid of all the type warnings:</p></div>

<pre class='language-julia'><code class='language-julia'>with_terminal() do
    @code_warntype double(typednumbers, :one)
end</code></pre>
<pre id="plutouiterminal">MethodInstance for Main.var\"workspace#4\".double(::Dict{Symbol, Float64}, ::Symbol)
  from double(�[90mmapping�[39m, �[90mkey�[39m::�[1mSymbol�[22m)�[90m @�[39m �[90mMain.var\"workspace#4\"�[39m �[90m~/Documents/UConn/ME5180/dynamics/tutorials/�[39m�[90m�[4mbenchmarking.jl#==#3f0e2049-8597-4dac-b499-4d7a8a35978e:1�[24m�[39m
Arguments
  #self#�[36m::Core.Const(Main.var\"workspace#4\".double)�[39m
  mapping�[36m::Dict{Symbol, Float64}�[39m
  key�[36m::Symbol�[39m
Body�[36m::Float64�[39m
�[90m1 ─�[39m %1 = Main.var\"workspace#4\".:*�[36m::Core.Const(*)�[39m
�[90m│  �[39m %2 = Base.getindex(mapping, key)�[36m::Float64�[39m
�[90m│  �[39m %3 = (%1)(2, %2)�[36m::Float64�[39m
�[90m└──�[39m      return %3
</pre>


<div class="markdown"><p>And makes the method more quick:</p></div>

<pre class='language-julia'><code class='language-julia'>using BenchmarkTools</code></pre>


<pre class='language-julia'><code class='language-julia'>with_benchmark_terminal() do
    @benchmark double(numbers, :one)
end</code></pre>
<pre id="plutouiterminal">BenchmarkTools.Trial: 10000 samples with 966 evaluations.
 Range (min … max):  56.905 ns …  7.125 μs  ┊ GC (min … max): 0.00% … 98.34%
 Time  (median):     63.362 ns              ┊ GC (median):    0.00%
 Time  (mean ± σ):   73.214 ns ± 77.865 ns  ┊ GC (mean ± σ):  1.42% ±  1.65%

  ▆▆▄█▇▄▅▅▄▅▅▄▂▂▂▂▁▁▁▁▁▁     ▁      ▁▁▁  ▁▁▁▂▃▃▂▂▁            ▂
  ████████████████████████████████████████████████████▇██▆▆▇▆ █
  56.9 ns      Histogram: log(frequency) by time       125 ns &lt;

 Memory estimate: 16 bytes, allocs estimate: 1.
</pre>

<pre class='language-julia'><code class='language-julia'>with_benchmark_terminal() do
    @benchmark double(typednumbers, :one)
end</code></pre>
<pre id="plutouiterminal">BenchmarkTools.Trial: 10000 samples with 994 evaluations.
 Range (min … max):  30.721 ns …  8.184 μs  ┊ GC (min … max): 0.00% … 98.32%
 Time  (median):     32.460 ns              ┊ GC (median):    0.00%
 Time  (mean ± σ):   35.884 ns ± 85.784 ns  ┊ GC (mean ± σ):  3.19% ±  1.67%

  ▅█▆▆▅▄▃▂▁▂▂▄▅▅▄▃▂▂▂▂▁                                       ▂
  ███████████████████████▇▆▆▆▆▅▆▆▅▅▅▆▄▅▄▃▅▅▄▄▄▃▄▃▅▂▂▅▄▅▆▆▆▆▇▆ █
  30.7 ns      Histogram: log(frequency) by time        60 ns &lt;

 Memory estimate: 16 bytes, allocs estimate: 1.
</pre>


<div class="markdown"><h2 id="Appendix">Appendix</h2></div>

<pre class='language-julia'><code class='language-julia'>function with_benchmark_terminal(f)
    out = sprint(show, "text/plain", f())
    with_terminal() do
        print(out)
    end
end;</code></pre>

<div class='manifest-versions'>
<p>Built with Julia 1.11.7 and</p>
BenchmarkTools 1.3.2<br>
PlutoUI 0.7.51
</div>

<!-- PlutoStaticHTML.End -->
~~~

_To run this tutorial locally, download [this file](/tutorials/benchmarking.jl) and open it with
[Pluto.jl](https://plutojl.org)._
