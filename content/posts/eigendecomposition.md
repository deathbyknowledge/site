+++
date = '2024-12-01T09:58:59+01:00'
draft = false
title = 'Eigendecomposition'
tags = ['math', 'linear algebra']
+++

Mathematics textbooks will sometimes explain structures and concepts in terms of formulas and how to compute them, instead of an explanation that aims to yield intuitive understanding. Especially, since they **do** require a solid foundation.

I suggest any reader that finds themselves aware of this situation, always stop and ponder on each subject to really grasp what it means. Feel free to access other resources to see the matter from different perspectives.

The truth is that pinning down concepts that might seem too abstract into proper intuition, is what learning is all about.
Sure, you might not want to actively chase every subject you're given in class, but once you start learning on your own... you're not done until you **learn it**.

What's the point if you're just rushing stuff through?

## Eigendecomposition of a matrix.
{{< admonition >}}
This post assumes familiarty with eigenvectors and eigenvalues.
{{< /admonition >}}
I was reading _the_ [Deep Learning book](https://www.deeplearningbook.org/) as it was due on my list.

*Chapter 2* is a quick introduction to the necessary linear algebra in deep learning, it covers the basics and I took some intrigue in **eigendecomposition**.

### Decomposition.
The book starts with decomposition. Just like you can decompose integers into prime factors, e.g. **12** will always be \(2 \times 2 \times 3\) regardless of how you choose to represent integers.

Eigenvectors and eigenvalues are introduced with
> An eigenvector of a square matrix \(\bm{A}\) is a non-zero vector \(\bm{v}\) such that multiplication by \(\bm{A}\) alters only the scale of  \(\bm{v}\).

> $$ \bm{A}\bm{v} = \lambda\bm{v}$$
> The scalar \(\lambda\) is known as the **eigenvalue** corresponding to this eigenvector.

Pretty standard stuff. This is then followed by the introduction of the **eigendecomposition** of \(\bm{A}\).

> Suppose that a matrix \(\bm{A}\) has \(n\) linearly independent eigenvectors, \(\{\bm{v}^{(1)},...,\bm{v}^{(n)}\}\), with corresponding eigenvalues \(\{\lambda_{1},...,\lambda_{n}\}.\\\) We may concatenate all of the eigenvectors to form a matrix \(\bm{V}\) with one eigenvector per column: \(\bm{V} = [\bm{v}^{(1)},...,\bm{v}^{(n)}]\). Likewise, we can concatenate the eigenvalues to form a vector \(\bm{\lambda} = [\lambda_{1},...,\lambda_{n}].\\\) The **eigendecomposition** of \(\bm{A}\) is then given by

> $$ \bm{A} = \bm{V}diag(\bm{\lambda})\bm{V}^{-1}.$$

Aaaand they're done explaining.

Okay, I _knew_ what the symbols meant and how to compute them but it wasn't intuitively clear **why** that would be the **decomposition** of a matrix.

## A decomposition of eigendecomposition.
I'll try to offer a new way to _think_ about **eigendecomposition** rather than focusing on its _use cases_. I'll assume the reader has proper visual understadnging of linear transformations and change of {{< sidenote "basis" >}}3Blue1Brown's <a href="https://www.youtube.com/watch?v=P2LTAUO1TdA" target="_blank">video</a> is awesome. In fact, his entire playlist is golden.{{< /sidenote >}}.

Let's consider an abritrary diagonalizable matrix \(\bm{A} \in \mathbb{R}^{n \times n}\) acting on an arbitrary vector \(\bm{v} \in \mathbb{R}^n\). We know \(\bm{A}\) moves \(\bm{v}\) somehow (unless \(\bm{A}\) = \(\bm{I}\) then it stays the same).

One way to look at it is that each column of \(\bm{A}\) describes where each basis vector lands after its transformation. Let's try to look at through the lense of the *eigendecomposed* \(\bm{A}\):

$$ \bm{A}\bm{v} = \bm{V}diag(\bm{\lambda})\bm{V}^{-1}\bm{v} $$

Remember, \(\bm{V}\) is the matrix where each column is an **eigenvector** of \(\bm{A}\), and \(\bm{diag{\bm{\lambda}}}\) is a diagonal matrix with each **eigenvalue** of \(\bm{V}\).

Then, we can think of the transformation \(\bm{A}\) applies to \(\bm{v}\) in 3 steps: 

1. **Change of basis**: \(\bm{V}^{-1}\) translates the representation of \(\bm{v}\) to use the **eigenvectors** of \(\bm{A}\) as basis, more formally, its **eigenbasis**. Let's call this intermediary representation \(\bm{v'}\).
2. **Transformation**: Now \(diag(\bm{\lambda})\) just scales each component of \(\bm{v'}\). This **is** the transformation itself.  Let's call this \(\bm{v''}\). 
3. **Change to original basis**: \(\bm{V}\) translates \(\bm{v''}\) back to our original basis. This is now the complete \(\bm{Av}\).

I think of steps **1** and **3** as translation layers. The real transformation is happening in step **2**, if you were to skip it, the output would be the same as the input!

## Conclusion.
This was, perhaps, obvious for some. It sure wasn't for me and had to spend some time playing with the idea. The concept was a bit too abstract and couldn't figure out what it meant. 

Now that I feel confident, I'll compensate my lack of visuals with a short story.

---

\(\bm{v}\) was just as regular as everyone else.  Somehow, \(\bm{v}\) found himself in front of the portal. Curious as he was, he got closer. Close enough that he got sucked in.

"Welcome to the Eigenworld." -- said a voice close to that of a narrator.

"Weird." -- he thought -- he felt different, but the same. He couldn't quite describe it. In fact, not even words came out the same. When he tried to scream his name "\(\bm{v'}\)" came out instead.

He met the Eigenpeople, who understood him and helped him get back home. They warned him of the risks of going through the portal back, it could be fatal. Luckily, their local scientist told him he could inject him with the super-serum -- a flask labeled "\(diag(\bm{\lambda})\)" -- which would make him a super-vector and allow him to go back through the portal, risk free.

So he did. He took the serum and indeed became super. He didn't just feel stronger, he was different. He knew he'd never be the same. So he asked the Eigenpeople to call him \(\bm{v''}\).

After saying their goodbyes, \(\bm{v''}\) waved as he went back through the portal which somehow seemed inversed from this side.

He woke up. He was home. For a second he wondered if it t all had been a dream. He quickly realized, that was not the case. He was not \(\bm{v}\) anymore, he was \(\bm{Av}\).
