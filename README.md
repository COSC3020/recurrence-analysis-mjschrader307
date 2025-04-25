# Recurrence Analysis -- Mystery Function

Analyze the running time of the following recursive procedure as a function of
$n$ and find a tight big $O$ bound on the runtime for the function. You may
assume that each operation takes unit time. You do not need to provide a formal
proof, but you should show your work: at a minimum, show the recurrence relation
you derive for the runtime of the code, and then how you solved the recurrence
relation.

```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}
```

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.

---

There are 3 recursive calls on a third of the input, resulting in $3T(\frac{n}{3})$ for part of the recursive portion of $T(n)$. There is also a nested for-loop, with the outer part running $n^2$ times, its child loop running $n$ times, and its child loop running $n^2$ times. This loop portion has a complexity of $n^5$.

The base case just has a return statement, which runs in constant time.

Therefore, I have:

$$
T(n) =
\begin{cases}
  1 & \text n \le 1 \\
  3T(\frac{n}{3}) + n^5 & n > 1
\end{cases}
$$

$T(n) = 3T(\frac{n}{3}) + n^5$

$ = 3[3T(\frac{n}{9}) + (\frac{n}{3})^5] + n^5$

$ = 9T(\frac{n}{9}) + \frac{n^5}{81} + n^5$

$ = 9T(\frac{n}{9}) + n^5(1 + \frac{1}{81})$

$...$

$T(n) = 3^i T\left(\frac{n}{3^i}\right) + n^5 \sum\limits_{k=0}^{i - 1}(\frac{1}{81})^k$

(Geometric series):

$ = 3^iT(3^{-i}n) + n^5[\frac{1-(\frac{1}{81})^k}{1-\frac{1}{81}}]$

$ = 3^iT(3^{-i}n) + n^5[\frac{81}{80}(1 - 81^{-i})] $

Let $i = \log_3(n)$:

$ = 3^{\log_3(n)}T(3^{-\log_3(n)}n) + n^5[\frac{81}{80}(1 - 81^{-\log_3(n)})] $

$ = nT(1) + n^5[\frac{81}{80}(1 - (3^{-4\log_3(n)})] $

$ = n + n^5[\frac{81}{80}(1 - n^{-4})] $

Asymptotically, the term multiplied by $n^5$ doesn't matter, and neither does the $n$ term, so the resulting runtime complexity is $O(n^5)$

---

**I certify that I have listed all sources used to complete this exercise, including the use
of any Large Language Models. All of the work is my own, except where stated
otherwise. I am aware that plagiarism carries severe penalties and that if plagiarism is
suspected, charges may be filed against me without prior notice.**