# Linear trends
## Method of least squares

For a set of ordered pairs: $(x,y)$

Where $n$ is the number of ordered pairs,

* Calculate mean of $x$ values:
$$
\bar{X} = \frac{\displaystyle\sum_{i=1}^{n} x_i} {n}
$$

* Calculate mean of $y$ values:

$$
\bar{Y} = \frac{\displaystyle\sum_{i=1}^{n} y_i} {n}
$$

* Calculate slope of the line of best fit:

$$
m=\frac{ \displaystyle\sum_{i=1}^{n} (x_i-\bar{X})(y_i-\bar{Y}) }
{ \displaystyle\sum_{i=1}^{n} (x_i-\bar{X})^2 }
$$

* Calculate the $y$-intercept of the line:
  $$
  b = \bar{Y}-m\bar{X}
  $$