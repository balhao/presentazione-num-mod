---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Welcome to Slidev
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# duration of the presentation
duration: 35min
---




<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->


<RestartOnEnter><LorenzBackground /></RestartOnEnter>


<!--
Here is another comment.
-->


# Numerical methods to solve differential equations

[📥 Scarica le slide in PDF](/presentazione-num-mod/slides-export.pdf)

<a href="./slides-export.pdf" download class="px-3 py-1 rounded border(white opacity-20) hover:bg-white/10 transition">
  📥 Scarica PDF
</a>


---

# Differential Equations
##
<!-- ## per evidenziare il testo subito -->
Numerical methods to solve differential equations:

<div grid="~ cols-1 gap-4 mt-3">


<div v-click="1">

$$ 
\begin{equation*}
u_t=f(u,t) \qquad u_t = \frac{du}{dt}
\end{equation*}
$$

</div>

<div v-click="2">

Example:
$$
\begin{equation*} 
u_t=A(t) \cdot u + B(t) \qquad u_t=A(t) \cdot u 
\end{equation*}
$$

</div>

<div v-click="3">


Generally, we want to pass from $N$ order differential equations to a system of $N$ first order diff. equations:

$$
\begin{equation*}
\left \{
\begin{array}{rl}
u_{t} & = v\\
v_{t} & = g(u,v,t)\\
\end{array} \right.
\end{equation*}
$$

</div>

</div>

---

# Separation of variables
##
Given the Cauchy problem:


$$
\left \{
\begin{array}{rl}
&u_{t} = Au\\
& u(0) = u_0\\
\end{array} \right.
$$

We are going to solve this system with the separation of variables:

$$ 
{1|2|3|4|5}
\begin{aligned}
\frac{du}{dt} = Au \\
\Rightarrow \frac{du}{u} = A\,dt \\
\Rightarrow \int_{u_0}^{u} \frac{du'}{u'} = \int_0^t A\,dt \\
\Rightarrow \ln\left(\frac{u}{u_0}\right) = At \\
\Rightarrow u(t) = u_0 e^{At}
\end{aligned}
$$


---

<!-- Section: Discretization-->

# Discretization of the Solution

Given the Cauchy problem:

$$
\left \{
\begin{array}{rl}
&u_{t} = f(u,t)\\
& u(0) = u_0\\
\end{array} \right.
$$

<div grid="~ cols-1 gap-1 mt-1">


<div v-click="1">

Let's consider a temporal step $\Delta t = k$:

</div>

<div v-click="2">

$$
t_0=0; \quad t_1=k; \quad t_2=2k; \quad ... \quad t_k=nk; \qquad 0 \leq n \leq N
$$


</div>

<div v-click="3">


I want to obtain an approximation $v^n$ of $u(t_n)$:

</div>

<div v-click="4">

$$
u(t_n) = u_n \simeq v^n \quad \Rightarrow \quad f(u_n, t_n) \simeq f(v^n, t_n) = f^n
$$

</div>

<div v-click="5">

Then I should find a criterion to build the sequence $v^0, v^1, ..., v^n$ that furnishes the discrete representation of $u(t)$ in time. That will represent my **numerical solution**.

</div>

</div>

---

# Finite Difference Quotient
##

I can apply the definition of derivative as **difference quotient**:

<div grid="~ cols-2 gap-8 mt-4">

<div v-click="1">

$$\frac{u(t_{n+1})-u(t_n)}{t_{n+1}-t_n} \simeq u_t(t_n)$$

</div>

<div v-click="2">

$$\Rightarrow \qquad u_t(t_n) \simeq \frac{v^{n+1}-v^n}{k} = f^n$$

</div>

</div> 

<div class="mt-8">

<v-click at="3">
Obtaining then a recursive formula:
</v-click>

<div grid="~ cols-2 gap-8 mt-2">

<div v-click="4">

$$
\begin{cases}
    v^{n+1} = v^n + k f^n \\
    v^0 = u_0
\end{cases}
$$
</div>

<div class="text-sm">
<div v-click="5"> 

1. Starting from $v^0$, I evaluate $v^1 = v^0+kf^0$ with $v^0=u_0$ and $f^0=f(u_0, 0)$.

</div>
<div v-click="6" class="mt-2"> 

2. Once found $v^1$, I calculate $f^1=f(v^1, k)$.

</div>
<div v-click="7" class="mt-2"> 

3. ... and so on.

</div>
</div>

</div> </div>

---

# Explicit vs. Implicit Schemes

**ACHTUNG!**

I can have both:

<div grid="~ cols-1 gap-1 mt-1">

<div v-click="1">

$$
\frac{v^{n+1}-v^n}{k} = f^n \quad \text{and} \quad \frac{v^{n+1}-v^n}{k} = f^{n+1}
$$

</div>

<div v-click="2" class="mt-2"> 

Because I evaluate the derivative at endpoints of interest interval $[t_n; t_{n+1}]$.

</div>

</div>


<div v-click="3" class="mt-2">
  <p>I obtain then two recursive formulas:</p>


<div class="grid grid-cols-2 gap-4 mt-1">
  
  <div class="bg-indigo-800 border border-yellow-200 rounded-lg px-4 py-3 w-84 h-20 flex items-center justify-between">
    <span class="text-white">

  $$v^{n+1} = v^n + k \cdot f^n$$

  </span>
    <div class="text-yellow-200 text-xs font-bold ml-4 border-l border-yellow-200/30 pl-4">
      Explicit Euler
    </div>
  </div>

  <div class="bg-red-800 border border-yellow-200 rounded-lg px-4 py-3 w-84 h-20 flex items-center justify-between">
    <span class="text-white">

  $$v^{n+1} = v^n + k \cdot f^{n+1}$$

  </span>
    <div class="text-yellow-200 text-xs font-bold ml-4 border-l border-yellow-200/30 pl-4">
      Implicit Euler
    </div>
  </div>

</div>


</div>

<div v-click="4" class="mt-4"> 
 


<div v-click="4" class="absolute top-50 right-10 w-45 text-xs leading-tight text-gray-400 border-l-2 border-yellow-400 pl-4">

**Implicit** because the $f^{n+1}$ term contains $v^{n+1}$, a value that already appears at the first member.

<div class="text-2xl text-yellow-500 mt-2">↙</div>
</div>
</div>


---

# Slide: Central and Trapezium Schemes
##

I can also evaluate the derivative in a symmetrical point of the interval:

<div grid="~ cols-1 gap-1 mt-1">

<div v-click="1">

$$
u_t(t_n) = \frac{u(t_{n+1})-u(t_{n-1})}{t_{n+1}-t_{n-1}} = \frac{v^{n+1}-v^{n-1}}{2k}
$$


<div v-click="2" class="mt-8 flex items-center justify-center w-full">
  
  <div class="text-3xl text-yellow-400 mr-8">→</div>

  <div class="bg-indigo-800 border border-yellow-200 rounded-lg px-4 py-3 w-100 h-20 flex items-center justify-between">
    <span class="text-white">

  $$v^{n+1} = v^{n-1} +2k \cdot f^n $$

  </span>
    <div class="text-yellow-200 text-xs font-bold ml-4 border-l border-yellow-200/30 pl-4">
      Central Value
    </div>
  </div>

</div>


</div>

<div v-click="3" class="mt-2"> 



Or I can also evaluate the average of $f^n$ and $f^{n+1}$:

</div>

<div class="grid grid-cols-2 gap-4 mt-1">


<div v-click="4" class="mt-2"> 

$$
\frac{v^{n+1}-v^n}{k} = \frac{1}{2}\left(f^{n+1} +f^n \right)
$$

</div>

<div v-click="5" class="mt-2"> 



<div v-click="5" class="mt-8 flex items-center justify-center w-full">
  
  <div class="text-3xl text-yellow-400 mr-8">→</div>

  <div class="bg-red-800 border border-yellow-200 rounded-lg px-4 py-3 w-100 h-20 flex items-center justify-between">
    <span class="text-white">

  $$v^{n+1}  = v^n +\frac{k}{2}\left(f^n + f^{n+1} \right) $$

  </span>
    <div class="text-yellow-200 text-xs font-bold ml-4 border-l border-yellow-200/30 pl-4">
      Trapezium
    </div>
  </div>

</div>


<div v-click="6" class="absolute top-50 right-10 w-45 text-xs leading-tight text-gray-400 border-l-2 border-yellow-400 pl-4">

Taking the central value I cannot apply the formula to evaluate $v^1$: I have to use Euler for that.

<div class="text-2xl text-yellow-500 mt-2">←</div>
</div>



</div>
</div>
</div>

---

# Slide: Example - Explicit Euler

## Example


Now let's apply the previous formulas to the Cauchy problem:


$$
\left \{
\begin{array}{rl}
&u_{t} = f(u,t)=u\\
& u(0) = u_0\\
\end{array} \right.
$$


<div grid="~ cols-1 gap-1 mt-1">

<div v-click="1">

whose analytical solution is $u = u_0 \cdot e^t$.

</div>

<div v-click="2" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

 
  <div class="text-white text-sm flex-grow">

  <div v-click="3" class="mb-2">

  $$[1] \quad v^1 = u_0 + u_0 \cdot k = u_0(1+k)$$

  </div>

  <div v-click="4" class="mb-2">

  $$[2] \quad v^2 = v^1 + k \cdot f^1 = u_0(1+k)^2$$

  </div>

   <div v-click="5" class="mb-2">

  $$[3] \quad v^3 = u_0(1+k)^3$$

  </div>

  <div v-click="6" class="font-bold text-yellow-100">

  $$[n] \quad v^n = u_0(1+k)^n$$

  </div>

  </div>

  <div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-48 flex-shrink-0">

  RECURSIVE STEP

  <div class="mt-2 text-white font-mono bg-gray-900 p-2 rounded border border-gray-700">

  $$v^{n+1} = v^n + k f^n$$

  </div>

  <div class="mt-1 text-gray-400 italic font-normal">Explicit Euler</div>

  </div>



</div>
</div>

---

# Slide: Example - Implicit


<div grid="~ cols-1 gap-1 mt-1">

  
<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

  <div class="text-white text-sm flex-grow">

  <div v-click="2" class="mb-2">

  $$[1] \quad v^1 = v^0 + k v^1 \implies v^1 = \frac{1}{1-k} u_0$$

  </div>

  <div v-click="3" class="mb-2">

  $$[2] \quad v^2 = v^1 + k v^2 \implies v^2 = \frac{1}{(1-k)^2} u_0$$

  </div>

  <div v-click="4" class="mb-2">

  $$[3] \quad v^3 = u_0(1-k)^{-3}$$

  </div>

  <div v-click="5" class="font-bold text-yellow-100">

  $$[n] \quad v^n = u_0(1-k)^{-n}$$

  </div>

  </div>

  <div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-48 flex-shrink-0">
    RECURSIVE STEP
    <div class="mt-2 text-white font-mono bg-gray-900 p-2 rounded border border-gray-700">

  $$v^{n+1} = v^n + k f^{n+1}$$

  </div>

  <div class="mt-1 text-gray-400 italic font-normal">Implicit Euler</div>

  </div>

</div>
</div>


---

# Slide: Example - Trapezium

<div grid="~ cols-1 gap-1 mt-1">

<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

  <div class="text-white text-sm flex-grow">

  <div v-click="2" class="mb-2">

  $$[1] \quad v^1 = v^0 + \frac{k}{2}(v^0 + v^1) \implies v^1 = u_0 \frac{1+k/2}{1-k/2}$$

  </div>

  <div v-click="3" class="mb-2">

  $$[2] \quad v^2 = v^1 + \frac{k}{2}(v^1 + v^2) \implies v^2 = u_0 \frac{(1+k/2)^2}{(1-k/2)^2}$$

  </div>

  <div v-click="4" class="mb-2">
    
  $$[3] \quad v^3 = u_0 \frac{(1+k/2)^3}{(1-k/2)^3}$$
    
  </div>

  <div v-click="5" class="font-bold text-yellow-100">
      
  $$[n] \quad v^n = u_0 \left( \frac{1+k/2}{1-k/2} \right)^n$$
  
  </div>

  </div>

  <div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-52 flex-shrink-0">
    RECURSIVE STEP
    <div class="mt-2 text-white font-mono bg-gray-900 p-2 rounded border border-gray-700">

  $$v^{n+1} = v^n + \frac{k}{2}(f^n + f^{n+1})$$

  </div>
  <div class="mt-1 text-gray-400 italic font-normal">Trapezium Method</div>
  </div>

</div>
</div>


---

# Slide: Comparison of Approximations (n=1)
##

Now let's try to understand which of the outlined methods better approximate the analytical solution. The latter, evaluated at time $t_n$, is: $u(t_n) = u_0 e^{nk}$. Let's consider the 4 formulas at the first tme step ($n=1$).

<div grid="~ cols-1 gap-1 mt-1">

<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-3">
<span class="text-gray-400 mr-2">Explicit:</span>

$$ \frac{v^1}{u_0} = 1 + k $$

</div>

<div v-click="3" class="mb-3">
<span class="text-gray-400 mr-2">Implicit:</span>

$$ \frac{v^1}{u_0} = \frac{1}{1-k} \approx 1 + k + k^2 + k^3 + \dots $$

</div>

<div v-click="4" class="font-bold text-yellow-100">
<span class="text-yellow-200/50 mr-2 text-sm font-normal">Trapezium:</span>

$$ \frac{v^1}{u_0} = \frac{1+k/2}{1-k/2} \approx 1 + k + \frac{k^2}{2} + \frac{k^3}{4} + \dots $$

</div>

<div v-click="5" class="mt-4 pt-2 border-t border-gray-700 italic text-xs text-gray-300">
The trapezium method offers the best approximation, exact until the third term (second order in k). [The above formulas are obtained using Taylor expansions]
</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-56 flex-shrink-0">
ANALYTICAL SOLUTION
<div class="mt-2 text-white font-mono bg-gray-900 p-2 rounded border border-gray-700">

$$ \frac{u(t_1)}{u_0} = e^k $$

</div>
<div class="mt-1 text-gray-400 italic font-normal">

$$ 1 + k + \frac{k^2}{2} + \dots + \frac{k^m}{m!} $$

</div>
</div>

</div>
</div>

---

# Runge-Kutta methods
##
Up to now, we have limited ourselves to evaluating the function only at the limits of each time step.  
Now, given a time interval $[t_n, t_{n+1}]$, the idea is to also evaluate the function at intermediate points within this interval to improve accuracy.

<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">STEP 1: Intermediate Value (Euler)</span>

$$ \tilde{v}^{n+\frac{1}{2}} = v^n + \frac{k}{2} f^n $$

</div>

<div v-click="3" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">STEP 2: Evaluation</span>

$$ \tilde{f}^{n+\frac{1}{2}} = f(\tilde{v}^{n+\frac{1}{2}}, t_{n+\frac{1}{2}}) $$

</div>

<div v-click="4" class="font-bold text-yellow-100">
<span class="text-yellow-200/50 mr-2 text-sm font-normal">FINAL STEP:</span>

$$ v^{n+1} = v^n + k \tilde{f}^{n+\frac{1}{2}} $$

</div>

<div v-click="5" class="mt-4 pt-2 border-t border-gray-700 italic text-xs text-gray-300">
The <b>Midpoint</b> method uses an intermediate estimate to improve the slope accuracy.
</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-48 flex-shrink-0">
RECURSIVE STEP

<div class="mt-2 text-white font-mono bg-gray-900 p-2 rounded border border-gray-700">

$$ v^{n+1} = v^n + k f^{n+\frac{1}{2}} $$

</div>

<div class="mt-1 text-gray-400 italic font-normal">Runge-Kutta 2 (RK2)</div>
</div>

</div>


---

# Two-Stage Intermediate Time Integration
##

We consider two intermediate times within the interval $[t_n, t_{n+1}]$, namely $t_{n+\frac{1}{3}}$ and $t_{n+\frac{2}{3}}$.
Starting from the known values $(t_n, v^n, f^n)$, 
we can define intermediate estimates as follows:

<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-2 flex items-center justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">[1] First Stage:</span>

$$ \tilde{v}^{n+\frac{1}{3}} = v^n + \frac{k}{3} f^n, \quad \tilde{f}^{n+\frac{1}{3}} = f(\tilde{v}^{n+\frac{1}{3}}, t_{n+\frac{1}{3}}) $$

</div>

<div v-click="3" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">[2] Second Stage:</span>

$$ \tilde{v}^{n+\frac{2}{3}} = v^n + \frac{2k}{3} \tilde{f}^{n+\frac{1}{3}}, \quad \tilde{f}^{n+\frac{2}{3}} = f(\tilde{v}^{n+\frac{2}{3}}, t_{n+\frac{2}{3}}) $$

</div>

<div v-click="4" class="font-bold text-yellow-100">
<span class="text-yellow-200/50 mr-2 text-sm font-normal">[3] Final Step:</span>

$$ v^{n+1} = v^n + \frac{k}{2} \left( \tilde{f}^{n+\frac{1}{3}} + \tilde{f}^{n+\frac{2}{3}} \right) $$

</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-48 flex-shrink-0">
MULTI-STAGE STEP

<div class="mt-2 text-white font-mono bg-gray-900 p-2 rounded border border-gray-700">

$$ v^{n+1} = v^n + \sum c_i f_i $$

</div>

<div class="mt-1 text-gray-400 italic font-normal">Runge-Kutta Variant</div>
</div>

</div>

<div v-click="5" class="mb-3">
  
The idea is to divide the interval into intermediate times (in principle, we could use more stages), and recursively construct $\tilde{v}^{n+c_i}$ and $\tilde{f}^{n+c_i}$ for $0 < c_i < 1$. This approach improves the accuracy by incorporating multiple evaluations of the derivative function $f(v, t)$ within each time step.

</div>

---

# General Recursive Formula
##
Starting from $c_1 = 0$, let $\alpha_{ij}$ denote the intermediate coefficients and $b_i$ the final weights.


<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-2 flex items-center w-200 h-100 justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-2">
<span class="text-gray-400 mr-2 text-xs">[1]</span>

$$ \text{Known} \quad v^{n+c_1} \quad \text{and} \quad f^{n+c_1} = f(v^{n+c_1}, t_{n+c_1})$$

</div>

<div v-click="3" class="mb-2">
<span class="text-gray-400 mr-2 text-xs">[2]</span>

$$ \tilde{v}^{n+c_2} = v^n + k\,\alpha_{21} f^{n+c_1}; \quad \tilde{f}^{n+c_2} = f(\tilde{v}^{n+c_2}, t_{n+c_2}) $$

</div>

<div v-click="4" class="mb-2">
<span class="text-gray-400 mr-2 text-xs">[3]</span>

$$ \tilde{v}^{n+c_3} = v^n + k\left(\alpha_{31} f^{n+c_1} + \alpha_{32} f^{n+c_2}\right); \quad \tilde{f}^{n+c_3} = f(\tilde{v}^{n+c_3}, t_{n+c_3}) $$

</div>

<div v-click="5" class="mb-2 italic text-gray-400">
<span class="text-gray-400 mr-2 text-xs">[s]</span>

$$ \tilde{v}^{n+c_s} = v^n + k\left(\alpha_{s1}f^{n+c_1} + \dots + \alpha_{s,s-1}f^{n+c_{s-1}}\right) $$

</div>

<div v-click="6" class="font-bold text-yellow-100 border-t border-gray-700 pt-2">
<span class="text-yellow-200/50 mr-2 text-sm font-normal">[END]</span>

$$ v^{n+1} = v^n + k\sum_{i=1}^{s} b_i f^{n+c_i} $$

</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-48 flex-shrink-0">
BUTCHER TABLEAU

<div class="mt-2 text-white font-mono bg-gray-900 p-2 rounded border border-gray-700">

$$ \begin{array}{c|c} c & A \\ \hline & b^T \end{array} $$

</div>

<div class="mt-1 text-gray-400 italic font-normal">General RK Structure</div>
</div>

</div>

---

# Runge–Kutta Coefficient Matrix (Butcher Tableau)


<div class="flex flex-col items-center my-4">

$$
\begin{array}{c|cccc}
c_1 \\
c_2 & \alpha_{21} \\
c_3 & \alpha_{31} & \alpha_{32} \\
\cdots & \cdots & \cdots & \ddots \\
c_s & \alpha_{s1} & \alpha_{s2} & \cdots & \alpha_{s,s-1} \\
\hline
& b_1 & b_2 & \cdots & b_{s-1} & b_s
\end{array}
$$

<div class="text-sm mt-4 text-left w-full max-w-md">

$c_i$: stage time coefficients <br>
$\alpha_{ij}$: intermediate weights, $i=1,\dots,s$, $j=1,\dots,i-1$ <br>
$b_i$: final weights

</div>

</div>

---

# Runge–Kutta of Order 1 (Midpoint Method)

<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">[1]</span>

$$ \tilde{v}^{n+\frac{1}{2}} = v^n + \frac{k}{2}f^n \quad \Rightarrow \quad \tilde{f}^{n+\frac{1}{2}} $$

</div>

<div v-click="3" class="font-bold text-yellow-100">
<span class="text-yellow-200/50 mr-2 text-sm font-normal">[2]</span>

$$ v^{n+1} = v^n + k\,\tilde{f}^{n+\frac{1}{2}} $$

</div>

</div>

<div v-click="4" class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-40 flex-shrink-0 text-center">
MIDPOINT (RK2)

<div class="mt-2 text-white bg-gray-900 p-2 rounded border border-gray-700">

$$
\begin{array}{c|cc}
0 \\
\frac{1}{2} & \frac{1}{2} \\
\hline
& 0 & 1
\end{array}
$$

</div>
</div>

</div>

<div v-click="5" class="mt-8 px-4">
<p class="text-sm text-gray-300 italic mb-4">
The simple Euler method can be viewed as a Runge–Kutta method of order zero, with matrix:
</p>

<div class="flex items-center space-x-8">

<div class="bg-gray-800 border border-gray-600 rounded p-4">

$$
\begin{array}{c|c}
0 \\
\hline
& 1
\end{array}
$$

</div>


<div class="text-2xl text-yellow-500 mt-2">→</div>

<div class="bg-gray-900 border border-gray-700 rounded p-4 font-mono text-white">

$$ v^{n+1} = v^n + k f^n $$

</div>

</div>
</div>

---

# From Matrix to Implementation

<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">[1]</span>

$$ \tilde{v}^{n+\frac{1}{3}} = v^n + \frac{k}{3} f^n \Rightarrow \tilde{f}^{n+\frac{1}{3}} $$

</div>

<div v-click="3" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">[2]</span>

$$ \tilde{v}^{n+\frac{2}{3}} = v^n + k\left(0\cdot f^n + \frac{2}{3}\tilde{f}^{n+\frac{1}{3}}\right) \Rightarrow \tilde{f}^{n+\frac{2}{3}} $$

</div>

<div v-click="4" class="font-bold text-yellow-100 border-t border-gray-700 pt-2">
<span class="text-yellow-200/50 mr-2 text-sm font-normal">[END]</span>

$$ v^{n+1} = v^n + \frac{k}{4} \left(f^n + 3 f^{n+\frac{2}{3}} \right) $$

<div class="text-right text-xs italic text-gray-400 mt-1">
[Heun formula, 2nd order]
</div>

</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-50 h-50 flex-shrink-0 text-center">
HEUN (RK3 VARIANT)

<div class="mt-2 text-white bg-gray-900 p-2 rounded border border-gray-700">

$$
\begin{array}{c|ccc}
0 \\
\frac{1}{3} & \frac{1}{3} \\
\frac{2}{3} & 0 & \frac{2}{3} \\
\hline
& \frac{1}{4} & 0 & \frac{3}{4}
\end{array}
$$

</div>
</div>

</div>

---

# Application Example

<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-2 text-gray-400">
<span class="mr-2 text-xs">(1)</span>

$$\text{Initial state:} \quad t_n, v^n, f^n$$

</div>

<div v-click="3" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">(2)</span>

$$ \tilde{v}^{n+c_2} = v^n + k \alpha_{21} f^n \quad \Rightarrow \quad \tilde{f}^{n+c_2} $$

</div>

<div v-click="4" class="font-bold text-yellow-100">
<span class="text-yellow-200/50 mr-2 text-sm font-normal">(3)</span>

$$ v^{n+1} = v^n + k(b_1 f^n + b_2 f^{n+c_2}) $$

</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-40 flex-shrink-0 text-center">
GENERAL 2-STAGE RK

<div class="mt-2 text-white bg-gray-900 p-2 rounded border border-gray-700">

$$
\begin{array}{c|cc}
0 \\
c_2 & \alpha_{21} \\
\hline
& b_1 & b_2
\end{array}
$$

</div>
</div>

</div>

<div v-click="5" class="mt-8 px-4">
<p class="text-sm text-gray-300 mb-4">
Apply this to the Cauchy problem:
</p>



<div class="flex items-center space-x-12">

<div class="bg-gray-800 border border-gray-600 w-35 h-25 rounded p-1">

$$
\begin{cases}
u_t = u \\
u(0) = u_0
\end{cases}
$$

</div>

<div class="text-2xl text-yellow-500 mt-2">→</div>

<div class="bg-gray-900 border border-yellow-600 w-35 h-25 rounded p-1 text-white">

<span class="text-xs text-gray-400 block mb-1">Analytical Solution:</span>
$$ u(t) = u_0 e^{t} $$

</div>

</div>
</div>

---

# Order Conditions

<div v-click="1" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-2 text-gray-400">
<span class="mr-2 text-xs">(1)</span>

$$t_0 = 0, \; v^0 = u_0, \; f^0 = u_0$$

</div>

<div v-click="3" class="mb-3">
<span class="text-gray-400 mr-2 text-xs">(2)</span>

$$ \tilde{v}^{c_2} = u_0 + k\alpha_{21}u_0 = u_0(1 + k\alpha_{21}) \quad \Rightarrow \quad \tilde{f}^{c_2} = \tilde{v}^{c_2} $$

</div>

<div v-click="4" class="font-bold text-yellow-100">
<span class="text-yellow-200/50 mr-2 text-sm font-normal">(3)</span>

$$ v^1 = u_0 \left[ 1 + k (b_1 + b_2) + k^2 b_2 \alpha_{21} \right] $$

</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-40 flex-shrink-0 text-center">
STABILITY ANALYSIS

<div class="mt-2 text-white bg-gray-900 p-2 rounded border border-gray-700">

$$ u(t_1) = u_0 e^k $$

</div>

<div class="mt-1 text-gray-400 italic font-normal text-[10px]">

$$ u_0(1 + k + \tfrac{1}{2}k^2 + \dots) $$

</div>
</div>

</div>



<div v-click="5" class="mt-8 px-4">
<p class="text-sm text-gray-300 mb-4">
Comparing with the exact solution, for <b>second-order accuracy</b> we require:
</p>

<div class="flex items-center space-x-12">

<div class="bg-gray-800 border border-gray-600 w-35 h-25 rounded p-2">

$$
\begin{cases}
b_1 + b_2 = 1 \\
\alpha_{21}b_2 = \frac{1}{2}
\end{cases}
$$

</div>

<div class="flex-grow text-sm text-gray-400">

Two equations for three unknowns $(b_1, b_2, \alpha_{21})$.  

<br>
We add the common constraint relating coefficients of the same row.
</div>

</div>
</div>

---

# Consistency Contraints


<p class="text-sm text-gray-400 mb-4 italic">
General row-sum constraint (consistency):
</p>



<div grid="~ cols-3 gap-8 mt-4">

<div v-click="1">

$$
\begin{aligned}
\alpha_{21} &= c_2 \\
\alpha_{31} + \alpha_{32} &= c_3 \\
\vdots \\
\alpha_{s1} + \dots + \alpha_{s,s-1} &= c_s
\end{aligned}
$$

</div>


<div v-click="2" class="text-2xl text-yellow-500 mt-2">

  $$ \Rightarrow $$

</div>


<div v-click="2" class="bg-gray-900 border border-yellow-600 w-35 h-25 rounded p-1 text-white">

$$\sum_{j=1}^{i-1}\alpha_{ij} = c_i$$

</div>

</div>


<div v-click="3" class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4 max-w-3xl">

<div class="text-white text-sm flex-grow">

<div class="text-yellow-200/70 text-xs mb-4 font-bold uppercase tracking-wider">
Imposing the constraint for our case:
</div>

<div v-click="4" class="flex items-center space-x-12">

<div class="bg-gray-900 border border-gray-700 p-6 rounded-lg">

$$
\begin{cases}
b_1 + b_2 = 1 \\
c_2 b_2 = \frac{1}{2}
\end{cases}
$$

</div>

<div class="text-gray-400 text-xs italic">
These conditions ensure second-order accuracy by reducing the number of independent unknowns.
</div>

</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-32 flex-shrink-0 text-center">
ORDER 2 CONDITIONS

<div class="mt-4 text-gray-500 font-normal">

$s = 2$

</div>
</div>

</div>

---

<div class="bg-gray-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between mt-4">

<div class="text-white text-sm flex-grow">

<div class="text-yellow-200/70 text-xs mb-2 font-bold uppercase tracking-wider">
The resulting R–K matrix is:
</div>

<div v-click="1" class="mb-2">

$$
\begin{array}{c|cc}
0 \\
c_2 & c_2 \\
\hline
& 1 - \frac{1}{2c_2} & \frac{1}{2c_2}
\end{array}
$$

</div>

</div>

<div class="text-yellow-200 text-xs font-bold ml-6 border-l border-yellow-200/30 pl-6 w-44 flex-shrink-0 text-center">
PARAMETRIC FORM
<div class="mt-2 text-gray-400 italic font-normal text-[10px]">

General solution for 2nd order accuracy with $s=2$
</div>
</div>

</div>


<div v-click="3" class="mt-8 px-4 flex items-center space-x-6">

<div class="bg-gray-900 border-l-4 border-yellow-500 p-4">
<p class="text-sm text-gray-300">
In general, it also holds that:
</p>

$$ \sum_{i=1}^{s} b_i = 1 $$

</div>

<div class="text-sm text-gray-400 italic">
For specific choices of coefficients, canonical reference matrices exist.
</div>

</div>




---

# Linear Multistep
##

A general form of a linear multistep scheme is given by:

<div v-click="1" class="my-4">

$$
\begin{aligned}
\alpha_{n+1} v^{n+1} + \alpha_n v^n + \dots + \alpha_{n-s+1} v^{n-s+1} = \\
= k \left( \beta_{n+1} f^{n+1} + \beta_n f^n + \dots + \beta_{n-s+1} f^{n-s+1} \right)
\end{aligned}
$$

</div>

<div v-click="2" class="mt-6">

This class of schemes is called a **Linear Multistep Method (LMM)**.

Unlike Runge–Kutta methods, which use multiple function evaluations within one step,
LMMs use several past time levels to advance the solution.

</div>



<div v-click="3" class="mt-10">

We start again from the Cauchy problem:

$$
\left\{
\begin{array}{rl}
u_t &= f(u, t) \\
u(0) &= u_0
\end{array}
\right.
$$

</div>


---
layout: default
---

# Integral Formulation

We discretize time as $t_n = n k$ and define:

<div v-click="1" class="my-4">

$$ v^n = u(t_n), \qquad f^n = f(v^n, t_n) $$

</div>

<div v-click="2" class="mt-6">

Integrating between two consecutive time levels:

$$ \int_{t_n}^{t_{n+1}} u_t \, dt = \int_{t_n}^{t_{n+1}} f(u, t) \, dt $$

</div>



<div v-click="3" class="mt-8">

Let $q(t)$ be a polynomial that interpolates $f$ within $[t_n, t_{n+1}]$.  
Then:

$$ u(t_{n+1}) - u(t_n) = \int_{t_n}^{t_{n+1}} q(t)\, dt $$

</div>

<div v-click="4" class="mt-4 font-bold text-yellow-500">

$$ \Rightarrow \quad v^{n+1} = v^n + \int_{t_n}^{t_{n+1}} q(t)\, dt $$

</div>

---

# Polynomial Interpolation and Implicitness
##
If $f(t)$ is known at several points, we can build higher-order interpolating polynomials $q(t)$.

<div v-click="1" class="mt-6">

Depending on whether $f^{n+1}$ is included in $q(t)$, the resulting scheme is:

* **Explicit**, if $f^{n+1}$ is not included.
* **Implicit**, if $f^{n+1}$ appears in $q(t)$.

</div>

<div v-click="2" class="mt-10 flex flex-col items-center">

<img src="./Figures/poly.png" class="h-50 rounded shadow-md" alt="Polynomial Interpolation" />

<div class="mt-2 text-sm text-gray-500 italic">

Interpolation of $f(t)$ at multiple time levels.

</div>

</div>


---

# Adams-Bashforth Methods
## 
Consider $q_{AB}(t)$ as the interpolating polynomial. We seek the solution in the time interval $[t_n, t_{n+1}]$.

<div class="mt-8">

**1st ORDER**

<div v-click="1" class="mt-4">

Supposing we know the function at $t_n$: $q_{AB}(t) = f^n$.  
Applying $t_{n+1} - t_n = k$:

</div>

<div v-click="2" class="mt-4">

$$v^{n+1} = v^n + \int_{t_n}^{t_{n+1}} f^n dt$$

</div>

<div v-click="3" class="flex items-center gap-4 mt-4">
  
  <div class="text-2xl text-yellow-500">

  $$ \Rightarrow $$
</div>

  <div class="bg-indigo-800 border border-yellow-200 rounded-lg px-6 py-3 flex items-center justify-between min-w-100 shadow-lg">
    <span class="text-white text-xl">

  $$ v^{n+1} = v^n + k f^n$$

  </span>

  <div class="text-yellow-200 text-xs font-bold ml-8 border-l border-yellow-200/30 pl-4 uppercase tracking-wider">
      Adams-Bashforth, 1°
    </div>
  </div>

</div>

</div>

---

# Adams–Bashforth Methods
##  
**2nd ORDER**

Suppose we know the function at $f^{n-1}$ and $f^n$. Then $q_{AB}(t)$ is the line passing through the points $(t_{n-1}, f^{n-1})$ and $(t_n, f^n)$:

<div v-click="1" class="mt-4">

$$q_{AB}(t) = f^{n-1} + \frac{f^n - f^{n-1}}{t_n - t_{n-1}}(t - t_{n-1})$$

</div>

<div v-click="2" class="mt-6">

Integrating between $t_n$ and $t_{n+1}$ yields:

</div>

<div v-click="3" class="flex items-center gap-4 mt-4">
  
  

  <div class="bg-indigo-800 border border-yellow-200 rounded-lg px-6 py-4 flex items-center justify-between min-w-120 shadow-lg">
    <span class="text-white text-xl">

  $$v^{n+1} = v^n + \frac{k}{2} (3 f^n - f^{n-1})$$

  </span>

  <div class="text-yellow-200 text-xs font-bold ml-8 border-l border-yellow-200/30 pl-4 uppercase tracking-wider text-right">
      Adams-Bashforth,<br>2° Order
    </div>
  </div>

</div>



---

# Adams–Moulton Methods
##
Including also $f^{n+1}$ in the interpolation makes the scheme **implicit**. We are interested in the solution in $[t_n, t_{n+1}]$.

<div class="mt-8">

**1st ORDER**

<div v-click="1" class="mt-4">

Supposing we know the function at $t_{n+1}$: $q_{AM}(t) = f^{n+1}$.  
Integrating between $t_n$ and $t_{n+1}$ gives:

</div>

<div v-click="2" class="flex items-center gap-4 mt-8">
  


  <div class="bg-red-800 border border-yellow-200 rounded-lg px-4 py-3 w-120 h-20 flex items-center justify-between">
    <span class="text-white text-xl">

  $$v^{n+1} = v^n + k f^{n+1}$$

  </span>

  <div class="text-yellow-200 text-xs font-bold ml-8 border-l border-yellow-200/30 pl-4 uppercase tracking-wider text-right">
      Adams–Moulton,<br>1° Order
    </div>
  </div>

</div>

</div>



---
---

# Adams–Moulton Methods

**2nd ORDER**

Suppose $q_{AM}(t)$ is the linear interpolant passing through $(t_n, f^n)$ and $(t_{n+1}, f^{n+1})$:

<div v-click="1" class="mt-4">

$$q_{AM}(t) = f^n + \frac{f^{n+1} - f^n}{t_{n+1} - t_n}(t - t_n)$$

</div>

<div v-click="2" class="mt-6">

Integrating between $t_n$ and $t_{n+1}$ yields:

</div>

<div v-click="3" class="flex items-center gap-4 mt-4">
  
  
  <div class="bg-red-800 border border-yellow-200 rounded-lg px-4 py-3 w-120 h-20 flex items-center justify-between">
    <span class="text-white text-xl">

  $$v^{n+1} = v^n + \frac{k}{2} (f^{n+1} + f^n)$$

  </span>
    <div class="text-yellow-200 text-xs font-bold ml-8 border-l border-yellow-200/30 pl-4 uppercase tracking-wider text-right">
      Adams-Moulton,<br>2° Order
    </div>
  </div>

</div>

<div v-click="4" class="mt-8 p-3 bg-gray-800/50 rounded border-l-2 border-blue-400">
  <p class="text-sm">
    <span class="font-bold text-blue-400">Trapezoidal Rule:</span> 
    This 2nd order implicit scheme is famously known as the Trapezoidal method. At the 3° ORDER I will have a parabola instead.
  </p>
</div>





---

# Backward Differentiation
##

We now approximate the solution $u(t)$ itself with an interpolating polynomial $q(t)$ passing through known points $(t_{n+1}, v^{n+1}), (t_n, v^n), \dots$ 

<div v-click="1" class="mt-4">

Depending on the points selected, $q$ will be a polynomial of 1°, 2°, ... order.

<div class="mt-2 text-sm text-gray-400 italic">

Note: Approximating $q$ as a constant at $v^{n+1}$ results in a zero derivative, which is a too low order approximation.

</div>

</div>

<div v-click="2" class="mt-10">

If $q$ is of **degree 1**, it passes through $(t_n, v^n)$ and $(t_{n+1}, v^{n+1})$:

$$q_{BD}(t) = v^n + \frac{v^{n+1} - v^n}{k} (t - t_n)$$

</div>

<div v-click="3" class="flex items-center gap-6 mt-8">

  <div class="text-2xl text-yellow-500">

  $$ \Rightarrow $$ 
 </div>

  <div class="text-2xl font-semibold">

  $$\dot{q}_{BD}(t) = \frac{v^{n+1} - v^n}{k}$$

  </div>

  
</div>

---

# Backward Differentiation — Order 1
##
We can now evaluate $\dot{q}_{BD}(t)$ at different time levels:

<ul class="mt-8 space-y-8">

<li v-click="1">

In $t_n$: &nbsp;&nbsp; $\frac{v^{n+1} - v^n}{k} = f^n$

<div class="mt-2 flex items-center gap-4">
  <div class="border-2 border-blue-400 px-3 py-1 rounded text-blue-100 font-mono">

  $v^{n+1} = v^n + k f^n$

  </div>
  <span class="text-gray-400 italic">(Explicit Euler)</span>
</div>

</li>

<li v-click="2">

In $t_{n+1}$: &nbsp;&nbsp; $\frac{v^{n+1} - v^n}{k} = f^{n+1}$

<div class="mt-2 flex items-center gap-4">
  <div class="border-2 border-red-400 px-3 py-1 rounded text-red-100 font-mono">

  $v^{n+1} = v^n + k f^{n+1}$

  </div>
  <span class="text-gray-400 italic">(Implicit Euler)</span>
</div>

</li>

</ul>

<div v-click="3" class="mt-12 pt-4 border-t border-gray-700 text-gray-300">

These represent the simplest members of the Backward Differentiation family.

</div>



---

# Backward Differentiation — Order 2
##

Now consider three points: $(t_{n-1}, v^{n-1})$, $(t_n, v^n)$, and $(t_{n+1}, v^{n+1})$.

<div class="mt-4">

The quadratic interpolant (a parabola) is:

</div>

<div v-click="1" class="my-6">

$$
\begin{aligned}
q_{BD}(t) = &\, v^{n-1} + (t - t_{n-1}) \frac{v^n - v^{n-1}}{k} \\
&+ \frac{(t - t_n)(t - t_{n-1})}{2k^2} (v^{n+1} - 2v^n + v^{n-1})
\end{aligned}
$$

</div>



<div v-click="2" class="mt-8">

Differentiating with respect to time:

</div>

<div v-click="3" class="flex items-center gap-6 mt-4">


<div class="text-lg">

$$
\dot{q}_{BD}(t) = \frac{v^n - v^{n-1}}{k} + \frac{v^{n+1} - 2v^n + v^{n-1}}{2k^2} (2t - t_n - t_{n-1})
$$

</div>

</div>

---


# Backward Differentiation — Evaluation
##
Evaluating the derivative $\dot{q}_{BD}(t)$ at different time levels:

<ul class="mt-10 space-y-12">

<li v-click="1">

**In $t_n$:** &nbsp;&nbsp; $\dot{q}_{BD}(t_n) = f^n$

<div class="mt-4 flex items-center gap-6">
  <div class="text-2xl text-yellow-500">
  
  $$\Rightarrow$$

</div>
  <div class="border-2 border-blue-400 px-3 py-1 rounded text-blue-100 font-mono">

  $$v^{n+1} = v^{n-1} + 2k f^n$$

  </div>
  <span class="text-gray-400 italic ml-4">(Explicit 2-step scheme)</span>
</div>

</li>

<li v-click="2">

**In $t_{n+1}$:** &nbsp;&nbsp; $\dot{q}_{BD}(t_{n+1}) = f^{n+1}$

<div class="mt-4 flex items-center gap-6">
  <div class="text-2xl text-yellow-500">
  
  $$\Rightarrow$$ 

</div>

  <div class="border-2 border-red-400 px-3 py-1 rounded text-red-100 font-mono">

  $v^{n+1} = -\frac{1}{3}v^{n-1} + \frac{4}{3}v^n + \frac{2}{3}k f^{n+1}$

  </div>
  <span class="text-gray-400 italic ml-4">(Implicit 2-step scheme / BDF2)</span>
</div>

</li>

</ul>


---

# Spatial-temporal discretization
##
Suppose we know the solution at discrete points $(x_j, t_n)$. 
We define $v_j^n$ as the numerical approximation: $v_j^n \simeq u(x_j, t_n)$.

<div class="grid grid-cols-2 gap-4 mt-8">
  <div v-click="1" class="bg-gray-800/50 p-4 rounded-lg border border-blue-500/30">
    <p class="text-blue-400 font-bold mb-2">Spatial Grid</p>

  $$x_{j+1} = x_j + h$$

  <p class="text-sm text-gray-400"> 

  $$\Delta x = h \quad \text{(spatial step)}$$ 
</p>
  </div>
  <div v-click="2" class="bg-gray-800/50 p-4 rounded-lg border border-yellow-500/30">
    <p class="text-yellow-400 font-bold mb-2">Temporal Grid</p>

  $$t_{n+1} = t_n + k$$

  <p class="text-sm text-gray-400">
  
  $$\Delta t = k \quad \text{(temporal step)}$$ 
</p>

  </div>
</div>



<div v-click="3" class="mt-10 p-4 ">
Let's introduce a series of <strong>discrete operators</strong>. 
Those acting on the <b>time</b> coordinate use a <b>superscript</b> ($$n$$), 
while those for the <b>spatial</b> coordinate use a <b>subscript</b> ($j$).

</div>

---

# Discrete Operators

<div class="grid grid-cols-2 gap-8">

<div v-click="1" class="border-2 border-green-500/50 p-2 rounded bg-green-500/5 h-55">
<h3 class="text-green-400 !text-lg mb-4 text-center">Temporal (Time)</h3>

$$
\begin{aligned}
\delta^+ v_j^n &= \frac{1}{k}(v_j^{n+1}-v_j^n) \\
\delta^- v_j^n &= \frac{1}{k}(v_j^{n}-v_j^{n-1}) \\
\delta^0 v_j^n &= \frac{1}{2k}(v_j^{n+1}-v_j^{n-1})
\end{aligned}
$$
</div>

<div v-click="2" class="border-2 border-purple-500/50 p-2 rounded bg-purple-500/5 h-55">
<h3 class="text-purple-400  !text-lg mb-2 text-center">Spatial (Space)</h3>

$$
\begin{aligned}
\delta_+ v_j^n &= \frac{1}{h}(v_{j+1}^{n}-v_j^n) \\
\delta_- v_j^n &= \frac{1}{h}(v_j^{n}-v_{j-1}^n) \\
\delta_0 v_j^n &= \frac{1}{2h}(v_{j+1}^{n}-v_{j-1}^n)
\end{aligned}
$$
</div>

</div>

<div v-click="3" class="mt-8 p-2 border-2 border-gray-600 rounded bg-gray-800/30 px-6 py-0 h-47">

<p class="text-yellow-500 mb-2">Properties:</p>

<div class="grid grid-cols-2 gap-4 text-center h-20 ">
  <div class="border-r border-gray-700">
    
  $$\delta^- v_j^{n+1} = \delta^+ v_j^n$$

  $$\delta^0 = \frac{1}{2}(\delta^- + \delta^+)$$
  </div>
  <div>
    
  $$\delta_- v_{j+1}^{n} = \delta_+ v_j^n$$

  $$\delta_0 = \frac{1}{2}(\delta_- + \delta_+)$$

  </div>
</div>
</div>




---

# Composition of Operators
##
Consider the composition of the two operators $\delta^-$ and $\delta^+$. 
By defining $\delta^X = \delta^- \cdot \delta^+$, we obtain the second-order central difference:

<div v-click="1" class="mt-8 flex items-center justify-center gap-8">
  
  <div class="border-2 border-yellow-500 px-6 py-4 rounded bg-yellow-500/5 font-mono text-xl">

  $$\delta^X v_j^n = \frac{1}{k^2} \left(v_j^{n+1}-2v_j^n + v_j^{n-1} \right)$$
  </div>

  <div class="border-l-4 border-gray-500 pl-4 py-1">
    <span class="text-gray-400 italic !text-xs block mb-1">Commutative Property:</span>
    <div class="text-blue-300 text-lg">

  $$\delta^X = \delta^- \cdot \delta^+ = \delta^+ \cdot \delta^-$$

  </div>
  </div>

</div>

<div v-click="2" class="mt-12">
  <p class="mb-4 ">The spatial analog is defined similarly:</p>
  <div class="border-2 border-purple-500 px-6 py-4 rounded bg-purple-500/5 font-mono text-xl w-fit mx-auto">

  $$\delta_X v_j^n = \frac{1}{h^2} \left(v_{j+1}^{n}-2v_j^n + v_{j-1}^n \right)$$

  </div>
</div>

---

# Geometric Interpretation

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-8 py-6 w-full mt-4">

<div class="text-white text-sm">

<div v-click="2" class="mb-6">
<span class="text-gray-400 mr-2 text-xs"> 
</span>

If I know the solution near $v_j^n$, I could know it everywhere interpolating with a straight line:

<div class="my-2">

$$u(x, t_n) = v_j^n + \frac{v_{j+1}^n - v_j^n}{h} (x-x_j) \quad [\text{line by } v_{j}^n ; v_{j+1}^n ]$$

</div>
</div>

<div v-click="3" class="mb-6">
<span class="text-gray-400 mr-2 text-xs">
</span>
The slope of the straight line is given by:

<div class="my-2">

$$\delta_+ v_j^n = \frac{1}{h} (v_{j+1}^n - v_j^n) \quad \Rightarrow \quad \fbox{$ \delta_+ v_j^n = \frac{\partial}{\partial x} u (x, t_n) $}$$

</div>
</div>

<div v-click="4" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>

The same holds for $\delta_-$ interpolating between $x_{j-1}$ and $x_j$:

<div class="my-2">

$$\delta_- v_j^n = \frac{1}{h} (v_j^n - v_{j-1}^n) \quad \Rightarrow \quad \fbox{$ \delta_- v_j^n = \frac{\partial}{\partial x} u (x, t_n) $}$$

</div>
</div>

</div>

</div>


---

# Higher Order Interpolation

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-4 py-0 w-full h-70 mt-2">

<div class="text-white text-sm">

<div v-click="2" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>

Now suppose we interpolate with a parabola passing through $v_{j-1}^n, v_j^n, v_{j+1}^n$.
After some passages, we arrive at the second order derivative:

<div class="my-4">

$$\frac{\partial^2 }{\partial x^2} u(x, t_n) = \frac{1}{h^2} \left(v_{j+1}^{n}-2v_j^n + v_{j-1}^n\right) = \delta_X v_j^n$$

</div>
</div>

<div v-click="3" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>
This yields the central difference for the second derivative:

<div class="my-4 flex items-center gap-4">

$$\Rightarrow \fbox{$ \frac{\partial^2 u}{\partial x^2} = \delta_X v_j^n $}$$

<span class="text-gray-400 italic text-xs">[curvature of the parabola]</span>
</div>
</div>

</div>

</div>



<div v-click="4" class="mt-4 space-y-2">
<p class="text-gray-300">Summary of approximations:</p>
<ul class="list-disc list-inside text-sm text-gray-400 ml-4">
  <li> 

Interpolating polynomial $1^\circ$ degree: $D^{(1)} = \delta_{\pm}$
</li>
  <li>

Interpolating polynomial $2^\circ$ degree: $D^{(1)} = \delta_0$; $D^{(2)} = \delta_X$

</li>
</ul>
</div>

<div v-click="5" class="mt-3 p-0 text-sm">

It is possible to achieve the same result by expanding in a <strong>Taylor series</strong> around $(x_j, t_n)$.

</div>


---

# UP-WIND (UW1)
##
Let's start from the transport equation: $u_t + c u_x = 0$.

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-8 py-6 w-full mt-4 flex justify-between items-center">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-6">
<span class="text-gray-400 mr-2 text-xs"></span>
Starting from: &nbsp; <span class="text-lg">

$$\fbox{$ \delta^+ v_j^n = -c \delta^+ v_j^n $} $$

</span>

<div class="my-4">

$$\frac{1}{k} \left(v_j^{n+1} - v_j^n \right) = - \frac{c}{h} \left( v_{j+1}^n - v_j^n\right) \qquad \text{setting } \lambda = \frac{k}{h}$$

</div>
</div>

<div v-click="3" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>
We obtain the update formula:

<div class="my-4">

$$\fbox{$ v_j^{n+1} =  v_j^n - c \lambda \left( v_{j+1}^n - v_j^n\right) $}$$

</div>
</div>

</div>

<div v-click="4" class="ml-6 border-l border-gray-700 pl-6 flex-shrink-0">

  

  <img src="./Figures/UW_1 2.png" class="h-20 rounded shadow-md" alt="Polynomial Interpolation" />

  <p class="text-[10px] text-gray-500 mt-2 text-center max-w-40 italic">
    Two points at the base allow to find the upper one.
  </p>
</div>

</div>

---

# UP-WIND (UW2)
##
Starting from the transport equation: $u_t + c u_x = 0$.

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-8 py-6 w-full mt-4 flex justify-between items-center">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-6">
<span class="text-gray-400 mr-2 text-xs"></span>
Starting from: &nbsp; <span class="text-lg">

$$\fbox{$ \delta^+ v_j^n = - c \delta^- v_j^n $} $$

</span>

<div class="my-4">

$$\frac{1}{k} \left(v_j^{n+1} - v_j^n \right) = - \frac{c}{h} \left( v_{j}^n - v_{j-1}^n\right) \qquad \text{setting } \lambda = \frac{k}{h}$$

</div>
</div>

<div v-click="3" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>
We obtain the update formula:

<div class="my-4">

$$\fbox{$ v_j^{n+1} = v_j^n - c \lambda \left( v_{j}^n - v_{j-1}^n\right) $}$$

</div>
</div>

</div>

<div v-click="4" class="ml-6 border-l border-gray-700 pl-6 flex-shrink-0">

  <img src="./Figures/UW_2 2.png" class="h-20 rounded shadow-md" alt="UW2 Stencil" />

  <p class="text-[10px] text-gray-500 mt-2 text-center max-w-40 italic">

  The backward spatial stencil uses points $j-1$ and $j$.
  </p>
</div>

</div>

---

# UP-WIND Choice
##
But how can I choose between the previous formulas?

<div v-click="1" >

<div class="text-white text-sm">

<div v-click="2" >
<div class="flex items-center gap-4 mb-2">
  <div class="bg-blue-500 text-black px-2 py-0.5 rounded font-bold text-xs">CASE 1</div>
  <span class="text-blue-200 font-bold"> 
    
  If $c > 0$:

  </span>
</div>
The wave propagates backward, and it is useful to use the first formula:
<div class="mt-4 text-center">

  $$\delta^+ v_j^n = c \delta_+ v_j^n$$

</div>
</div>

<div v-click="3">
<div class="flex items-center gap-4 mb-2">
  <div class="bg-green-500 text-black px-2 py-0.5 rounded font-bold text-xs">CASE 2</div>
  <span class="text-green-200 font-bold">

  If $c < 0$:

  </span>
</div>
The wave propagates forward, and it is useful to use the second one:
<div class="mt-4 text-center">

  $$\delta^+ v_j^n = c \delta_- v_j^n$$

</div>
</div>

</div>

</div>



<div v-click="4" class="mt-6 p-4 bg-yellow-900/20 border-l-4 border-yellow-500 text-xs italic text-gray-300">
Note: The choice of the spatial operator depends on the <strong>direction of information flow</strong> (characteristic direction).
</div>



---

# EULER (EU)
##
Starting from the transport equation: $u_t + c u_x = 0$.

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-8 py-6 w-full mt-4 flex justify-between items-center">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-6">
<span class="text-gray-400 mr-2 text-xs"></span>
Starting from: &nbsp; <span class="text-lg">

$$\fbox{$ \delta^+ v_j^n = - c \delta_0 v_j^n $} $$

</span>

<div class="my-4">

$$\frac{1}{k} \left(v_j^{n+1} - v_j^n \right) = - \frac{c}{2h} \left( v_{j+1}^n - v_{j-1}^n\right) \qquad \text{setting } \lambda = \frac{k}{h}$$

</div>
</div>

<div v-click="3" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>
We obtain the update formula:

<div class="my-4">

$$\fbox{$ v_j^{n+1} = v_j^n - \frac{1}{2} c \lambda \left( v_{j+1}^n - v_{j-1}^n\right) $}$$

</div>
</div>

</div>

<div v-click="4" class="ml-6 border-l border-gray-700 pl-6 flex-shrink-0">

  <img src="./Figures/EU_1 2.png" class="h-20 rounded shadow-md" alt="EU Stencil" />

  <p class="text-[10px] text-gray-500 mt-2 text-center max-w-40 italic">
    The two points at the base allow to find the upper one.
  </p>
</div>

</div>


---

# LEAP-FROG (LF)
##
Starting from the transport equation: $u_t + c u_x = 0$.

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-8 py-6 w-full mt-4 flex justify-between items-center">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-6">
<span class="text-gray-400 mr-2 text-xs"></span>
Starting from: &nbsp; <span class="text-lg">

$$\fbox{$ \delta^0 v_j^n = - c \delta_0 v_j^n $} $$

</span>

<div class="my-4">

$$\frac{1}{2k} \left(v_j^{n+1} - v_j^{n-1} \right) = - \frac{c}{2h} \left( v_{j+1}^n - v_{j-1}^n\right) \qquad \text{setting } \lambda = \frac{k}{h}$$

</div>
</div>

<div v-click="3" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>
We obtain the update formula:

<div class="my-4">

$$\fbox{$ v_j^{n+1} =  v_j^{n-1} - c \lambda  \left( v_{j+1}^n - v_{j-1}^n\right) $}$$

</div>
</div>

</div>

<div v-click="4" class="ml-6 border-l border-gray-700 pl-6 flex-shrink-0">

  <img src="./Figures/LF 2.png" class="h-20 rounded shadow-md" alt="LF Stencil" />

  <p class="text-[10px] text-gray-500 mt-2 text-center max-w-40 italic">

  The leap-frog scheme uses the time level $n-1$ to find $n+1$.
  </p>
</div>

</div>

---

# CRANK-NICHOLSON (CN)
##
Starting from the transport equation: $u_t + c u_x = 0$.

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-8 py-6 w-full mt-4 flex justify-between items-center">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-6">
<span class="text-gray-400 mr-2 text-xs"></span>
Starting from: &nbsp; <span class="text-lg">

$$\fbox{$ \delta^+ v_j^n = - \frac{c}{2} \left( \delta_0 v_{j}^n + \delta_0 v_j^{n+1}\right) $} $$

</span>

<div class="my-4">

$$\frac{1}{k} \left(v_j^{n+1} - v_j^{n} \right) = - \frac{c}{4h} \left( v_{j+1}^n - v_{j-1}^n + v_{j+1}^{n+1} - v_{j-1}^{n+1}\right) \qquad \text{setting } \lambda = \frac{k}{h}$$

</div>
</div>

<div v-click="3" class="mb-2 text-white">
<span class="text-gray-400 mr-2 text-xs"></span>
We obtain the update formula:

<div class="my-4">

$$\fbox{$ v_j^{n+1} =  v_j^{n} - \frac{1}{4} c \lambda  \left( v_{j+1}^n - v_{j-1}^n + v_{j+1}^{n+1} - v_{j-1}^{n+1} \right) $}$$

<p class="text-blue-300 italic text-xs mt-2 text-right"> [implicit] </p>
</div>
</div>

</div>

<div v-click="4" class="ml-6 border-l border-gray-700 pl-6 flex-shrink-0">

  <img src="./Figures/CN 2.png" class="h-20 rounded shadow-md" alt="Crank-Nicholson Stencil" />

  <p class="text-[10px] text-gray-500 mt-2 text-center max-w-40 italic">

  The Crank-Nicholson stencil involves points at both time levels $n$ and $n+1$.
  </p>
</div>

</div>


---

# BACKWARD EULER (B-EU)
##
Starting from the transport equation: $u_t + c u_x = 0$.

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-8 py-6 w-full mt-4 flex justify-between items-center">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-6">
<span class="text-gray-400 mr-2 text-xs"></span>
Starting from: &nbsp; <span class="text-lg">

$$\fbox{$ \delta^+ v_j^n = - c \delta_0 v_j^{n+1} $} $$

</span>

<div class="my-4">

$$\frac{1}{k} \left(v_j^{n+1} - v_j^{n} \right) = - \frac{c}{2h} \left( v_{j+1}^{n+1} -  v_{j-1}^{n+1}\right) \qquad \text{setting } \lambda = \frac{k}{h}$$

</div>
</div>

<div v-click="3" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>
We obtain the update formula:

<div class="my-4">

$$\fbox{$ v_j^{n+1} =  v_j^{n} - \frac{1}{2} c \lambda  \left(v_{j+1}^{n+1} - v_{j-1}^{n+1} \right) $}$$

<p class="text-blue-300 italic text-xs mt-2 text-right"> [implicit] </p>
</div>
</div>

</div>

<div v-click="4" class="ml-6 border-l border-gray-700 pl-6 flex-shrink-0">

  <img src="./Figures/B-EU 2.png" class="h-20 rounded shadow-md" alt="Backward Euler Stencil" />

  <p class="text-[10px] text-gray-500 mt-2 text-center max-w-40 italic">

  The Backward Euler scheme evaluates the spatial derivative at the future time level $n+1$.
  </p>
</div>

</div>


---

# LAX-WENDROFF (LF)
##

It was born as an attempt to stabilize Euler: it adds a $2^\circ$ order term to Euler's formula. \[ some like $u_t = - cu_x + \frac{c^2}{2}u_{xx}$]

<div v-click="1" class="bg-gray-800 border border-yellow-200/50 rounded-lg px-8 py-6 w-full mt-4 flex justify-between items-center">

<div class="text-white text-sm flex-grow">

<div v-click="2" class="mb-6">
<span class="text-gray-400 mr-2 text-xs"></span>
Starting from: &nbsp; <span class="text-lg">

$$\fbox{$ \delta^+ v_j^n = c  \left( - \delta_0 v_{j}^{n} + c \frac{k}{2} \delta_X v_{j}^{n}\right)  $} $$

</span>

<div class="my-4 scale-95 origin-left">

$$\frac{1}{k} \left(v_j^{n+1} - v_j^{n} \right) = - \frac{c}{2h} \left( v_{j+1}^{n} -  v_{j-1}^{n}\right) + \frac{c^2 k}{2h^2} \left( v_{j+1}^{n} - 2v_j^n + v_{j-1}^{n}\right)$$

</div>
</div>

<div v-click="3" class="mb-2">
<span class="text-gray-400 mr-2 text-xs"></span>
We obtain the update formula:

<div class="my-4 scale-95 origin-left">

$$\fbox{$ v_j^{n+1} =  v_j^{n} - \frac{1}{2} c \lambda  \left(v_{j+1}^{n} - v_{j-1}^{n} \right) + \frac{1}{2} c^2 \lambda^2 \left(v_{j+1}^n - 2v_j^n + v_{j-1}^n\right) $}$$

</div>
</div>

</div>

<div v-click="4" class="ml-6 border-l border-gray-700 pl-6 flex-shrink-0">

  <img src="./Figures/EU_1 2.png" class="h-20 rounded shadow-md" alt="Lax-Wendroff Stencil" />

  <p class="text-[10px] text-gray-500 mt-2 text-center max-w-40 italic">
    The Lax-Wendroff scheme adds numerical diffusion for stability.
  </p>
</div>

</div>


---

# Stability of the Solution
## 
We study the stability of discrete solutions for the transport equation:

<div v-click="1" class="mt-8">

Starting from the IVP (Initial Value Problem) with constant velocity $c > 0$:

<div class="my-6 text-center">

$$
\begin{cases}
u_t + c u_x = 0, \\
u(x,0) = u_0(x).
\end{cases}
$$

</div>
</div>

<div v-click="2" class="mt-10">

We consider the Upwind (UW) scheme:

<div class="my-6 text-center">

$$\fbox{$ v_j^{n+1} = v_j^n - c\lambda \left( v_j^n - v_{j-1}^n \right) $}$$

</div>

<p class="text-gray-500 italic text-xs mt-2 text-center">

where $\lambda = \frac{\Delta t}{\Delta x}$ is the mesh ratio.
</p>
</div>


---

# Von Neumann Hypothesis
## 

Assume a harmonic solution of the form:

<div v-click="1" class="mt-10 text-center">

$$v_j^n = A_n e^{i\xi x_j}$$

</div>

<div v-click="2" class="mt-12">

Then:

<div class="mt-4 text-center">

$$v_{j-1}^n = A_n e^{i\xi (x_j-h)} = v_j^n e^{-i\xi h}$$

</div>

</div>

<div v-click="3" class="mt-12">

Define the amplification factor:

<div class="mt-4 text-center">

$$\fbox{$ z = \frac{A_{n+1}}{A_n} $}$$

</div>

</div>

---

# Amplification Factor
## 

Substituting the von Neumann ansatz into the upwind scheme:

<div v-click="1" class="mt-8 text-center">

$$
\begin{aligned}
z v_j^n &= v_j^n - c\lambda \left( v_j^n - v_{j-1}^n \right) \\
&= v_j^n - c\lambda \left( 1 - e^{-i\xi h} \right)v_j^n
\end{aligned}
$$

</div>

<div v-click="2" class="mt-8">

Therefore:

<div class="mt-4 text-center text-lg">

$$\fbox{$ z = 1 - c\lambda\left(1 - e^{-i\xi h}\right) $}$$

</div>

<p class="text-gray-500 text-xs mt-2 text-center">

Let $\theta = \xi h$</p>

</div>

<div v-click="3" class="mt-8">


Since $z$ does not depend on $n$:


<div class="mt-4 text-center">

$$v_j^n = z^n v_j^0 = z^n u_0(x_j)$$

</div>

</div>

---

# Real and Imaginary Parts of $z$
## 

Using Euler's formula: $e^{-i\theta} = \cos\theta - i\sin\theta$, we obtain:

<div v-click="1" class="mt-12 text-center">

$$
\begin{aligned}
z &= 1 - c\lambda + c\lambda(\cos\theta - i\sin\theta) \\
&= \underbrace{\left(1 - c\lambda + c\lambda\cos\theta\right)}_{\Re(z)} - i\,\underbrace{(c\lambda\sin\theta)}_{\Im(z)}
\end{aligned}
$$

</div>



<div v-click="2" class="mt-12 p-4 bg-gray-100/5 border-l-4 border-gray-500">
<p class="text-sm text-gray-400">

This decomposition allows us to analyze the <strong>magnitude</strong> $|z|$, which must be $\leq 1$ for stability.
</p>
</div>


---

# Modulus of the Amplification Factor
## 

For a complex number $z = a + ib$, the modulus is defined as:

<div v-click="1" class="mt-10 text-center">

$$|z| = \sqrt{a^2 + b^2}, \qquad |z|^2 = a^2 + b^2$$

</div>

<div v-click="2" class="mt-12">

Thus, for our specific amplification factor:

<div class="mt-6 text-center text-lg">

$$|z|^2 = \left(1 - c\lambda + c\lambda\cos\theta\right)^2 + \left(c\lambda\sin\theta\right)^2$$

</div>

</div>

---

# Simplification of $|z|^2$
## 

Expanding the terms and using the identity $\sin^2\theta + \cos^2\theta = 1$:

<div v-click="1" class="mt-10 text-center">

$$|z|^2 = (1 - c\lambda)^2 + 2c\lambda(1 - c\lambda)\cos\theta + (c\lambda)^2$$

</div>

<div v-click="2" class="mt-12">

Rewriting the expression:

<div class="mt-6 text-center text-lg">

$$\fbox{$ |z|^2 = 1 - 2c\lambda(1 - c\lambda)(1 - \cos\theta) $}$$


</div>

</div>

---

# Stability Condition
## 

Since we know that $1 - \cos\theta \ge 0$ for all $\theta$, the stability condition $|z| \le 1$ (or $|z|^2 \le 1$) becomes:

<div v-click="1" class="mt-10 text-center text-lg">

$$2c\lambda(1 - c\lambda) \ge 0$$

</div>

<div v-click="2" class="mt-12">

Hence, we find the required range for $c\lambda$:

<div class="mt-6 text-center text-2xl">

$$\fbox{$ 0 \le c\lambda \le 1 $}$$

</div>

<p class="mt-6 text-gray-400 italic text-center">

This is the <span v-mark.red="3"> <strong>CFL (Courant-Friedrichs-Lewy)</strong> </span>  condition for the upwind scheme.

</p>

</div>


---

# Exact Solution Comparison
## 

For a harmonic initial condition:

<div v-click="1" class="mt-8 text-center">

$$
\begin{aligned}
u_0(x) &= A e^{i\xi x} \\
u(x,t) &= A e^{i\xi (x - ct)}
\end{aligned}
$$

</div>

<div v-click="2" class="mt-10">

The exact amplification factor over one time step $\Delta t$ is:

<div class="mt-4 text-center text-lg">

$$\fbox{$ z_{\text{exact}} = e^{-i\xi c\Delta t}, \qquad |z_{\text{exact}}| = 1 $}$$

</div>

</div>

<div v-click="3" class="mt-10 p-4 bg-gray-100/5 border-l-4 border-blue-500">

The ideal numerical scheme should satisfy $|z|=1$ to preserve the amplitude of the solution.

</div>



---

# Effect of the Amplification Factor
## 

The discrete solution can be expressed as:

<div v-click="1" class="mt-12 text-center text-xl">

$$\fbox{$ v_j^n = |z|^n e^{i n \phi} e^{i\xi x_j} $}$$

</div>

<div v-click="2" class="mt-12">

The behavior of the scheme depends on the magnitude of $z$:

<div class="mt-6 space-y-4">

* <span class="text-yellow-400 font-bold">$|z| < 1$:</span> stable but **numerically diffusive** (amplitude decreases)
* <span class="text-green-400 font-bold">$|z| = 1$:</span> stable and **amplitude-preserving**
* <span class="text-red-400 font-bold">$|z| > 1$:</span> **unstable** (amplitude grows exponentially)

</div>

</div>


---

# Stability of Common Schemes (Von Neumann)
## 

Summary of stability conditions for the 1D transport equation:

<div v-click="1" class="mt-10 ml-6 text-sm">

* <span class="text-blue-300">**Upwind:**</span> stable $\iff 0 \le c\lambda \le 1$
* <span class="text-red-400">**Forward Euler (FTCS):**</span> always unstable
* <span class="text-green-300">**Backward Euler:**</span> unconditionally stable (diffusive)
* <span class="text-purple-300">**Crank–Nicolson:**</span> $|z|=1$ (neutrally stable)
* <span class="text-blue-300">**Lax–Friedrichs:**</span> stable $\iff c\lambda \le 1$
* <span class="text-blue-300">**Lax–Wendroff:**</span> stable $\iff c\lambda \le 1$

</div>

<div v-click="2" class="mt-12 p-4 bg-yellow-900/10 border border-yellow-500/30 rounded text-xs text-gray-400 italic">
Note: Unconditionally stable schemes allow for larger time steps, but they may introduce significant numerical dissipation.
</div>

---

# CFL Condition: Characteristics
## 

We consider the linear transport equation:

<div v-click="1" class="mt-8 text-center">

$$u_t + c u_x = 0, \qquad c \in \mathbb{R}$$

</div>

<div v-click="2" class="mt-10">

The exact solution is constant along characteristic curves $x - ct = \alpha$:

<div class="mt-4 text-center text-lg">

$$u(x,t) = u_0(x - ct)$$

</div>

</div>



<div v-click="3" class="mt-10 p-4 border-l-4 border-blue-500 italic">

The value of the solution at a point $(x,t)$ depends only on values taken along the same characteristic line at previous times.

</div>

---

# Mathematical Domain of Dependence
## 

Let $x_j$ be a grid point and $t_{n+1} = t_n + \Delta t$. Tracing the characteristic backward in time:

<div v-click="1" class="mt-8 text-center">

$$x' - c t_n = x_j - c t_{n+1}$$

</div>

<div v-click="2" class="mt-10">

Which gives:

<div class="mt-4 text-center text-lg">

$$\fbox{$ x' = x_j - c\Delta t $}$$

</div>

</div>

<div v-click="3" class="mt-10">

Thus, the value $u(x_j,t_{n+1})$ depends on the interval:

<div class="mt-4 text-center text-lg">

$$\fbox{$ I_{\text{MAT}} = [x_j - c\Delta t,\, x_j] $}$$

</div>

<p class="mt-4 text-center text-gray-500 italic">This is the mathematical domain of dependence.</p>

</div>

---

# Numerical Domain of Dependence
## 

The numerical domain of dependence is the set of grid points used to compute $v_j^{n+1}$. For the upwind scheme ($c>0$):

<div v-click="1" class="mt-6 text-center">

$$v_j^{n+1} = v_j^n - c\lambda (v_j^n - v_{j-1}^n)$$

</div>

<div v-click="2" class="mt-8">

The numerical domain of dependence is:

<div class="mt-4 text-center text-lg">

$$\fbox{$ I_{\text{NUM}} = [x_{j-1},\, x_j] $}$$

</div>

</div>



<div v-click="3" class="mt-10 text-center">

**CFL principle:**

<div class="mt-4 text-2xl">

$$\fbox{$ I_{\text{NUM}} \supseteq I_{\text{MAT}} $}$$

</div>

</div>

---

# CFL Condition
## 

From the domains of dependence:

<div v-click="1" class="mt-10 text-center text-lg">

$$x_j - c\Delta t \ge x_{j-1} = x_j - \Delta x$$

</div>

<div v-click="2" class="mt-12">

This implies $c\Delta t \le \Delta x$, or equivalently:

<div class="mt-6 text-center text-2xl">

$$\fbox{$ c\lambda \le 1, \qquad \lambda = \frac{\Delta t}{\Delta x} $}$$

</div>

<p class="mt-6 text-gray-400 italic text-center">
This is the CFL condition for the upwind scheme.
</p>

</div>

---

# Remark on the CFL Condition
## 

The CFL condition is a **necessary** condition for stability, but in general it is **not sufficient**.

<div v-click="1" class="mt-10 ml-10">

* Some unstable schemes satisfy CFL (e.g. FTCS for transport)
* Stability must be verified by von Neumann analysis

</div>

<div v-click="2" class="mt-12 p-4 border-l-4 border-gray-500">

The CFL condition ensures that numerical information propagates at least as fast as physical information.

</div>

---

# Physical Interpretation
## 

The CFL number $c\lambda = c\frac{\Delta t}{\Delta x}$ represents the distance traveled by a wave during one time step, measured in units of the spatial grid size.

<div v-click="1" class="mt-12 ml-10">

* <span class="text-blue-400">**$c\lambda < 1$:**</span> wave travels less than one cell per time step
* <span class="text-green-400">**$c\lambda = 1$:**</span> wave travels exactly one cell
* <span class="text-red-400">**$c\lambda > 1$:**</span> numerical scheme cannot capture the propagation

</div>

---

# Boundary Conditions
## 

In addition to initial conditions, boundary conditions are required. Let $x_j$, $j=1,\dots,N$, be grid points with spacing $\Delta x$.

<div v-click="1" class="mt-12 p-6 border border-yellow-500/30 rounded-lg">

Boundary conditions must be imposed only where characteristics **enter** the computational domain.

</div>



---

# Direction of Propagation
## 

Consider again the transport equation: $u_t + c u_x = 0$.

<div v-click="1" class="mt-12 ml-10 space-y-6">

* **If $c > 0$:** information travels from left to right
* **If $c < 0$:** information travels from right to left

</div>

<div v-click="2" class="mt-12 text-center text-lg">

Boundary conditions are needed only at the **inflow boundary**.

</div>

---

# Upwind Boundary Conditions
## 

<div v-click="1" class="mt-6">

**Case $c > 0$ (right-moving wave):**

<div class="mt-4 text-center">

$$
\begin{cases}
v_1^n = g(t_n) \quad \text{(inflow)} \\
v_j^{n+1} = v_j^n - c\lambda (v_j^n - v_{j-1}^n), \quad j\ge 2
\end{cases}
$$

</div>

</div>

<div v-click="2" class="mt-10">

**Case $c < 0$ (left-moving wave):**

<div class="mt-4 text-center">

$$
\begin{cases}
v_N^n = g(t_n) \quad \text{(inflow)} \\
v_j^{n+1} = v_j^n - c\lambda (v_{j+1}^n - v_j^n), \quad j\le N-1
\end{cases}
$$

</div>

</div>

---

# Heat Equation
## 

Consider the heat equation: $u_t = b^2 u_{xx}$.

<div v-click="1" class="mt-6 p-4 border-l-4 border-red-500 italic">

This equation has **no characteristics**, since information propagates instantaneously.

</div>

<div v-click="2" class="mt-10 ml-10">

* The CFL condition cannot be derived via characteristics
* Stability must be studied purely by von Neumann analysis

</div>

<div v-click="3" class="mt-10">

Explicit schemes will impose a restrictive stability condition of the form:


<span v-mark.circle.orange="4">
<div class="mt-4 text-center text-xl">

$$\fbox{$ \Delta t \le C \Delta x^2 $}$$

</div>

</span>

</div>


