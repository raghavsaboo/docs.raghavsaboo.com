
!!! note ""
    TODO: Add notes here - leaving blank for now.

## Vector Equations

Equation of linear combination of vectors with unknown coefficients. Essentially defines a linear system. For example, $\cos(x) + \sin(y) = 0$ is NOT linear.

There three ways to think of them:

- **System of linear equations**

$$
\begin{matrix}
2x_1 + 3x_2-3x_3=7\\
x_1-x_2-3x_3=5
\end{matrix}
$$

- **Augmented Matrix:** Representing the equation as a matrix and allowing elimination operations easily when done by hand.
    - **Algorithms:** Gaussian Elimination and Row Reduction.  
    - To solve: All of them are methods to get augmented matrix in reduced row echelon form.  
    - **Reduced:** Where all pivots are equal to 1, and are the only non-zero entry in the column.

$$
\left[
\begin{array}{ccc|c}
2 & 3 & -2 & 7 \\
1 & -1 & -3 & 5
\end{array}
\right]
$$


- **Vector Equation**

$$
x_1 \begin{pmatrix}
2\\1
\end{pmatrix} + x_2 \begin{pmatrix}
3\\-1
\end{pmatrix} + x_3 \begin{pmatrix}
-2\\-3
\end{pmatrix} = \begin{pmatrix}
7\\5
\end{pmatrix}
$$

A system of equation is **consistent** if it has a solution.  
If there are no solutions, it is **inconsistent**.

# Great References
[^1]: He Wang, Math 2331 Linear Algebra, Department of Mathematics Northeastern University. https://hewang.sites.northeastern.edu/math2331/
[^2]: He Wang, Math 4570 Matrix Methods in Data Analysis and Machine Learning https://hewang.sites.northeastern.edu/math4570/