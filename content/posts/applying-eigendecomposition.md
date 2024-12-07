+++
date = '2024-12-07T09:25:04+01:00'
draft = false
title = 'Applying eigendecomposition'
tags = ['math', 'linear algebra', 'algorithms', 'software']
+++
In my previous post {{< backlink "eigendecomposition" >}} I shared one way to intuitively think about **eigendecomposition**. Here, I'll show its applications _and_ how to slay your programming interview.

## Spoilers ahead.

I also mentioned 3Blue1Brown's playlist on linear {{< sidenote algebra >}}<a href="https://www.youtube.com/watch?v=fNk_zzaMoSs&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab" target="_blank">Playlist.</a>{{</ sidenote >}} in my previous post. At the end of chapter 14, he leaves a {{< sidenote "puzzle for the audience" >}}<a href="https://youtu.be/PFDu9oVAE-g?si=z_j3Xg2PKJHTQ0PF&t=989" target="_blank"/>Puzzle.</a> {{</ sidenote >}}. I'll show the workings of it, so don't blame me for spoiling the fun. You've been warned.

Also, if you haven't seen _The Matrix_ (1999), this is 100% what the movie is about.

## Square the matrix, because why not.
Our matrix is \(\bm{A} = \begin{bmatrix} 0 & 1 \\ 1 & 1 \end{bmatrix}\). You might not know it yet, but it's a very interesting matrix.

Let's compute \(\bm{A}^2\) explictly

$$
\begin{align*}
 \bm{A}^2 &=
    \begin{bmatrix}
      0 & 1 \\
      1 & 1
    \end{bmatrix}
    \begin{bmatrix}
      0 & 1 \\
      1 & 1
    \end{bmatrix} \\ 
  &=
    \begin{bmatrix}
      (0 \times 0) + (1 \times 1) & (0 \times 1) + (1 \times 1) \\
      (0 \times 1) + (1 \times 1) & (1 \times 1) + (1 \times 1)
    \end{bmatrix} \\
  &=
    \begin{bmatrix}
      1 & 1 \\
      1 & 2
    \end{bmatrix}
\end{align*}
$$

Alright, yeah, the matrix has been squared, **so what?** Patience, the effects are not obvious yet. Let's follow it up with \(\bm{A}^3\)

$$
\begin{align*}
  \bm{A}^3 &= 
    \begin{bmatrix}
      0 & 1 \\
      1 & 1
    \end{bmatrix}
    \begin{bmatrix}
      1 & 1 \\
      1 & 2
      \end{bmatrix} \\
  &=
    \begin{bmatrix}
      (0 \times 1) + (1 \times 1) & (0 \times 1) + (1 \times 2) \\
      (1 \times 1) + (1 \times 1) & (1 \times 1) + (1 \times 2)
    \end{bmatrix} \\
  &= \begin{bmatrix}
      1 & 2 \\
      2 & 3
    \end{bmatrix}
\end{align*}
$$

You might see the pattern already but _just in case_, I'll show \(\bm{A}^4\) skipping the explicit step, as it's a pain to write down (just as it is to compute).

$$
\begin{align*}
    A^4 &=
    \begin{bmatrix}
        0 & 1 \\
        1 & 1
    \end{bmatrix}
    \begin{bmatrix}
        1 & 2 \\
        2 & 3
    \end{bmatrix} \\
    A^4 &=
    \begin{bmatrix}
        2 & 3 \\
        3 & 5
    \end{bmatrix}
\end{align*}
$$

Woah! Is that following the fibonacci sequence? You bet your ass it is. We can say that for \(n \in \mathbb{R}, n > 0\)

$$
\begin{align*}
  \bm{A}^{n}_{0,0} &= fib(n)\\
  \bm{A}^{n}_{1,0} &= \bm{A}^{n}_{0,1} =  fib(n+1)\\
  \bm{A}^{n}_{1,1} &= fib(n+2)
\end{align*}
$$ 

This is indeed a cool fact about this matrix. It might even be useful if you need to compute fibonacci sequences but as you've seen, it's **hell** to compute.

> I can't get enough matrix multiplication. - No one (like, ever.)

## Decompose Decompose Decompose
Let's forget for a second what we've just seen and _blindly_ get its **eigendecomposition** (because I'm telling you to).

### Quick reminder
>$$ \bm{A}\bm{v} = \lambda\bm{v}$$
> An **eigenvector** of a square matrix \(\bm{A}\) is a non-zero vector \(\bm{v}\) such that multiplication by \(\bm{A}\) alters only the scale of  \(\bm{v}\).
> The scalar \(\lambda\) is known as the **eigenvalue** corresponding to this **eigenvalue**.{{<sidenote  "">}}From the <a href="https://www.deeplearningbook.org/contents/linear_algebra.html">Deep Learning</a> book.{{</ sidenote >}}


So, the **eigendecomposition** of a matrix \(\bm{A}\) is given by

$$ \bm{A} = \bm{V}\bm{\Lambda}\bm{V}^{-1}.$$

where \(\bm{V}\) is the matrix of all the eigenvectors of \(\bm{A}\) concatenated as columns and \(\bm{\Lambda}\) is a diagonal matrix with each \(i\text{th}\) diagonal entry corresponding to the **eigenvalue** of the \(i\text{th}\) column of \(\bm{V}\) (the \(i\text{th}\) **eigenvector**) of \(\bm{A}\).

Let's get to work.

### Eigenvalues
We'll start by finding the **eigenvalues** of \(\bm{A}\), since we know that 

$$
\begin{align*}
\bm{A}\bm{v} &= \lambda\bm{v} \\
             &= \lambda\bm{I}\bm{v} \\
\end{align*}
$$

then by rearranging a bit we get

$$
\begin{align*}
  \bm{A}\bm{v} - \lambda\bm{Iv} &= \vec{0}\\
  (\bm{A} - \lambda\bm{I})\bm{v} &= \vec{0} \\
  \begin{bmatrix}
    0 - \lambda & 1 \\
    1 & 1 - \lambda
  \end{bmatrix}
  \begin{bmatrix}
    x \\ 
    y \\ 
  \end{bmatrix}
  &= \vec{0}
\end{align*}
$$

From this we derive that that \((\bm{A} - \lambda\bm{I})\) collapses \(\bm{v}\) by at least one dimension. This is to say, by definition, that the determinant of \((\bm{A} - \lambda\bm{I})\) is 0.

We can now susbstitue our values of \(\bm{A}\) and expand the determinant

$$
\begin{align*}
  det(
    \begin{bmatrix}
      0 - \lambda & 1 \\
      1 & 1 - \lambda
    \end{bmatrix}
  ) &= 0 \\
  (0 - \lambda)(1 - \lambda) - (1) (1) &= 0 \\
  \lambda^2 - \lambda - 1 &= 0
\end{align*}
$$

We're close, now we just have to solve for the roots of the polynomial we got

$$
\begin{align*}
  \lambda &= \frac{1 \pm \sqrt{(-1)^2 - 4 \times 1 \times -1}}{2} \\
          &= \frac{1 \pm \sqrt{5}}{2}
\end{align*}
$$

>Ladies and gentleman, we got em.

Okay, they're not pretty, but we got the **eigenvalues** of our matrix. Now, we just need to find the **eigenvectors** and we're done decomposing! (Lie, we also need to find the inverse of the matrix of **eigenvectors** but shhhh)

### Eigenvectors
Let's refer back to one of our previous equations.

$$
\begin{align*}
  \begin{bmatrix}
    0 - \lambda & 1 \\
    1 & 1 - \lambda
  \end{bmatrix}
  \begin{bmatrix}
    x \\ 
    y \\ 
  \end{bmatrix}
  &= \vec{0}
\end{align*}
$$

Now we just need to substitute our values of \(\lambda\) to get our eigenvectors. So, for \(\lambda = \frac{1 + \sqrt{5}}{2}\)

$$
\begin{align*}
  \begin{bmatrix}
    0 - \frac{1 + \sqrt{5}}{2} & 1 \\
    1 & 1 - \frac{1 + \sqrt{5}}{2}
  \end{bmatrix}
  \begin{bmatrix}
    x \\ 
    y \\ 
  \end{bmatrix}
  &= \vec{0} \\
  \begin{bmatrix}
    - \frac{1 + \sqrt{5}}{2} & 1 \\
    1 & \frac{1 - \sqrt{5}}{2}
  \end{bmatrix}
  \begin{bmatrix}
    x \\ 
    y \\ 
  \end{bmatrix}
  &= \vec{0} \\
  x
  \begin{bmatrix}
    - \frac{1 + \sqrt{5}}{2} \\
    1
  \end{bmatrix}
+
  y \begin{bmatrix}
    1 \\
    \frac{1 - \sqrt{5}}{2}
  \end{bmatrix}
  &= \vec{0}
\end{align*}
$$

This leaves us with a beautiful set of equations for us to solve.
{{< sidenote "">}}If your attention span is short and I haven't lost you yet, don't waste it here. Feel free to skip ahead.{{</ sidenote >}}

$$
\begin{align}
-\frac{1 + \sqrt{5}}{2}x + y &= 0 \\
x + \frac{1 - \sqrt{5}}{2}y &= 0
\end{align}
$$

Rearranging \((1)\) to get \(x\) in terms of \(y\)

$$
\begin{align}
\frac{1 + \sqrt{5}}{2}x &= y \\
(1 + \sqrt{5})x &= 2y \\
x &= \frac{2}{1 + \sqrt{5}}y \\
\end{align}
$$

Now we can replace \((5)\) in \((2)\)

$$
\begin{align}
\frac{2}{1 + \sqrt{5}}y + \frac{1 - \sqrt{5}}{2}y &= 0 \\[10pt]
\frac{4 + (1 - \sqrt{5})(1 + \sqrt{5})}{2(1 + \sqrt{5})}y &= 0 \\[10pt]
\frac{0}{2(1 + \sqrt{5})}y &= 0 \\[10pt]
0y &= 0 \\[10pt]
\end{align}
$$

\(y\) is a free variable. Then if \(y = 1\), by equation \((5)\) we get the **eigenvector** \(\begin{bmatrix} \frac{2}{1 + \sqrt{5}} \\ 1 \end{bmatrix}\) but since we can scale it however we want, let's remove the surd from the denominator. Therefore we get \(\begin{bmatrix} 2 \\ 1 + \sqrt{5} \end{bmatrix}\).

Now, that was **awfully** painful to write down. I'm gonna save us both the trouble, and tell you that for \(\lambda = \frac{1 - \sqrt{5}}{2}\) we get an **eigenvector** \(\begin{bmatrix} 2 \\ 1 - \sqrt{5} \end{bmatrix}\).


So we finally have our **eigenvectors**, and we can concatenate them in a matrix as columns to get \(\bm{V}\)

$$
  \bm{V} = 
    \begin{bmatrix}
      2 & 2 \\  
      1 + \sqrt{5} & 1 - \sqrt{5}
    \end{bmatrix}
$$

### Inverse of \(\bm{V}\)
I _really_ want to get to the reason I was writing this post. I just need to get this last part done, so I'm going to be quick.

To compute the inverse of \(\bm{V}\), we do a little magic trick.

$$
\begin{align*}
  \bm{VV}^{-1} = \bm{I} \\
  \begin{bmatrix}
    2 & 2 \\  
    1 + \sqrt{5} & 1 - \sqrt{5}
  \end{bmatrix}
  \begin{bmatrix}
    a & b \\  
    c & d 
  \end{bmatrix} &= \begin{bmatrix}
    1 & 0 \\  
    0 & 1 
  \end{bmatrix}\end{align*}
$$

Contrary to the last set of equations we worked out, this one does **not** have infinitely many solutions. So if you expand the matrix multiplication you eventually get

$$
\begin{align*}
\bm{V}^{-1} = \begin{bmatrix}
    \frac{\sqrt{5} - 1}{4\sqrt{5}} & \frac{1}{2\sqrt{5}} \\  
    \frac{1 + \sqrt{5}}{4\sqrt{5}} & -\frac{1}{2\sqrt{5}}
  \end{bmatrix}
\end{align*}
$$

### Finally, the complete eigendecomposition.


$$
  \bm{A} = \bm{V}\bm{\Lambda}\bm{V}^{-1}\\
  \begin{bmatrix}
    0 & 1 \\
    1 & 1
  \end{bmatrix} = 
    \begin{bmatrix}
      2 & 2 \\  
      1 + \sqrt{5} & 1 - \sqrt{5}
    \end{bmatrix}
    \begin{bmatrix}
      \frac{1 + \sqrt{5}}{2} & 0 \\  
      0 & \frac{1 - \sqrt{5}}{2}
    \end{bmatrix}
  \begin{bmatrix}
    \frac{\sqrt{5} - 1}{4\sqrt{5}} & \frac{1}{2\sqrt{5}} \\  
    \frac{1 + \sqrt{5}}{4\sqrt{5}} & -\frac{1}{2\sqrt{5}}
  \end{bmatrix}
$$

## Chad Fibonacci
In my previous post I described \(\bm{V}\) and \(\bm{V}^{-1}\) as translation layers, the "real" transformation happens in \(\bm{\Lambda}\).

So, for if we wanted to compute \(\bm{A}^n\) again, we could do it as
$$
  \begin{bmatrix}
    0 & 1 \\
    1 & 1
  \end{bmatrix}^n = 
    \begin{bmatrix}
      2 & 2 \\  
      1 + \sqrt{5} & 1 - \sqrt{5}
    \end{bmatrix}
    \begin{bmatrix}
      (\frac{1 + \sqrt{5}}{2})^n & 0 \\  
      0 & (\frac{1 - \sqrt{5}}{2})^n
    \end{bmatrix}
  \begin{bmatrix}
    \frac{\sqrt{5} - 1}{4\sqrt{5}} & \frac{1}{2\sqrt{5}} \\  
    \frac{1 + \sqrt{5}}{4\sqrt{5}} & -\frac{1}{2\sqrt{5}}
  \end{bmatrix}
$$

Computationally, this is _way_ more efficient than doing standard matrix multiplicaiton since stacking multiplications of diagonal matrixes is just multiplying the corresponding diagonal entries.

The "standard" way we did at the beginning requires 8 muls and 4 adds, _per step_. Even if you were to optimize by removing the operations that either add a zero or multiply by zero, you'd get a total of 8 ops _per step_. 

By using the _Chad Fibonacci_ way of squaring \(\bm{\Lambda}\) directly, the powers themselves only require 2 ops (2 muls) but we pay the fixed cost of having to "translate" the result every time.

Let's write some code :)

## Virgin Fibonacci
Let me showcase one of the worse fibonacci implementations
```python
import time
def virgin_fib(n):
  if n == 0:
    return 0
  elif n < 3:
    return 1
  else:
    return virgin_fib(n-1) + virgin_fib(n-2)

n = 20
ts = time.monotonic()
print(virgin_fib(n)) # 6765
dt = time.monotonic() - ts
print(f"took {dt*1000:.2f}ms with virgin fib") # took 1.94ms with virgin fib
```

One simple way to write our _Chad Fibonacci_ is with numpy, because there's no way I'm writing down matrix multiplicaiton again.

```python
# Create the "translation" layers
V = np.array([
    [2, 2],
    [1 + np.sqrt(5), 1 - np.sqrt(5)]
])
V_inv= np.array([
    [(np.sqrt(5) - 1) / (4 * np.sqrt(5)), 1 / (2 * np.sqrt(5))],
    [(np.sqrt(5) + 1) / (4 * np.sqrt(5)), -1 / (2 * np.sqrt(5))]
])
def chad_fib(n):
    # The transformation itself
    transform= np.array([
        [(1+np.sqrt(5))/2, 0],
        [0, (1 - np.sqrt(5))/2]
    ])
    total = transform

    # Easy optimization
    if n == 0:
        return 0
    elif n < 3:
        return 1

    # Since a step already contains n, n+1 and n+2
    n = n-2
    for _ in range(n):
        total = np.matmul(transform, total)

    translated = np.matmul(V, np.matmul(total, V_inv))
    return round(translated[1][1])
```

### Collect your winnings

A quick comparison of timings, although you can disregard the reccursive time and just look at our version's time.
```python
n = 40
ts = time.monotonic()
print(chad_fib(n)) # 102334155
dt = time.monotonic() - ts
print(f"took {dt*1000:.2f}ms with chad") # took 0.30ms with chad
ts = time.monotonic()
print(virgin_fib(n)) # 102334155
dt = time.monotonic() - ts
print(f"took {dt*1000:.2f}ms with rec") # took 12613.22ms with rec
```

Now you can **ace** your programming interview with this _not at all_ more complicated version of fibonacci. (Although, for real, don't use the recursive one)

I do have to mention, that this approach will end up with small inaccuracies in its output for larger values of \(n\), since we're chaining floating point operations and later rounding the result.

### Conclusion
As you could see, computational efficency is one of the use cases where **eigendecomposition** is useful. Hopefully this helped you understand **eigeneverythings** a little bit better. Writing this definitely helped _me_ understand it better, as you bet I did get some unexpected results a few times.

