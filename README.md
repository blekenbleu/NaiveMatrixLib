# NaiveMatrixLib - [*Visual Studio fork*](https://blekenbleu.github.io/static/ImageProcessing/matrix.htm)
A simple C++ stdlib-based complex &amp; real matrix library,  
&emsp; with matrix inversion, [left division (A\b)](https://help.altair.com/compose/help/en_us/topics/language_guides/left_matrix_division_r.htm) and determinant calculation.<br />

## Features
* Designed for users who don't want to use large linear algebra libs.
* Only used C++ standard library, easy to learn and modify (Each file less than 600 lines).
* Header files only, separated complex and real matrix library.
* No recursive algorithm (using LU and Cholesky decomposition).  
  Reliable for 1000 x 1000 and larger matrices.


## Available Functions
* <b>rank</b>:    Matrix rank ([Cholesky decomposition](https://www.geeksforgeeks.org/maths/cholesky-factorization/))
* <b>det</b>:     Matrix [determinant](https://www.geeksforgeeks.org/maths/what-is-determinant-of-a-matrix/) calculation
* <b>inv</b>:     [LU decomposition-based](https://www.geeksforgeeks.org/engineering-mathematics/l-u-decomposition-system-linear-equations/) matrix inversion
* <b>pinv</b>:    pinv(G) = inv(G' * G) * G' (<b>WARNING</b>: [full-rank matrix](https://fiveable.me/linear-algebra-and-differential-equations/key-terms/full-rank-matrix) only!)
* <b>pinv2</b>:   [Moore-Penrose pseudoinversion](https://www.math.ucla.edu/~laub/33a.2.12s/mppseudoinverse.pdf) (same as pinv(G) in MATLAB)
* <b>[leftDiv</b>: x = A \ b](https://help.altair.com/compose/help/en_us/topics/language_guides/left_matrix_division_r.htm),
	using Moore-Penrose pinv, NOT same as MATLAB for a [singular matrix](https://en.wikipedia.org/wiki/Singular_matrix)


## Examples (Complex Matrices Only)
```cpp
#include "matBasic_complex.hpp"
int main(int argc, char **argv)
{
    i_complex_matrix matA = {
            {{1.0, 0.0}, {0.0, 1.0}, {4.0, 0.0}},
            {{0.0, 5.0}, {1.0, 0.0}, {4.0, 0.0}},
            {{8.0, 0.0}, {1.0, 1.0}, {0.0, 0.0}}};
    showMatrix(matA, "A");
    std::cout << "rank(A) = " << rank(matA) << "\n";
    std::cout << "det(A) = " << det(matA) << "\n";
    showMatrix(inv(matA), "inv(A)");
    showMatrix(pinv(matA), "pinv(A)");
    showMatrix(pinv2(matA), "pinv2(A)");

    i_complex_matrix matb = {
        {{1.0, 0.0}},
        {{1.0, 2.0}},
        {{0.0, 3.0}}};
    showMatrix(matb, "b");
	std::cout << "leftDiv(matA, matb) << "\n";
    showMatrix(leftDiv(matA, matb), "A \\ b");
    showMatrix(matMul(inv(matA), matb), "inv(A) * b");
    showMatrix(matMul(pinv(matA), matb), "pinv(A) * b");
    showMatrix(matMul(pinv2(matA), matb), "pinv2(A) * b (== A \\ b)");
    std::cin.get();
    return 0;
}
```
### Output
```
A : 3 x 3 Complex Matrix:
    row[1]: 1,  0+1i,  4;
    row[2]: 0+5i,  1,  4;
    row[3]: 8,  1+1i,  0;

rank(A) = 3
det(A) = (-56,48)
inv(A) : 3 x 3 Complex Matrix:
    row[1]: 0.00588235+0.0764706i,  -0.00588235-0.0764706i,  0.0764706-0.00588235i;
    row[2]: -0.329412-0.282353i,  0.329412+0.282353i,  0.217647-0.170588i;
    row[3]: 0.177941+0.0632353i,  0.0720588-0.0632353i,  -0.0617647-0.0529412i;

pinv(A) : 3 x 3 Complex Matrix:
    row[1]: 0.00588235+0.0764706i,  -0.00588235-0.0764706i,  0.0764706-0.00588235i;
    row[2]: -0.329412-0.282353i,  0.329412+0.282353i,  0.217647-0.170588i;
    row[3]: 0.177941+0.0632353i,  0.0720588-0.0632353i,  -0.0617647-0.0529412i;

pinv2(A) : 3 x 3 Complex Matrix:
    row[1]: 0.00588235+0.0764706i,  -0.00588235-0.0764706i,  0.0764706-0.00588235i;
    row[2]: -0.329412-0.282353i,  0.329412+0.282353i,  0.217647-0.170588i;
    row[3]: 0.177941+0.0632353i,  0.0720588-0.0632353i,  -0.0617647-0.0529412i;



b : 3 x 1 Complex Matrix:
    row[1]: 1;
    row[2]: 1+2i;
    row[3]: 0+3i;

leftDiv(matA, matb)
A \ b : 3 x 1 Complex Matrix:
    row[1]: 0.170588+0.217647i;
    row[2]: -0.0529412+1.31176i;
    row[3]: 0.535294-0.0411765i;

inv(A) * b : 3 x 1 Complex Matrix:
    row[1]: 0.170588+0.217647i;
    row[2]: -0.0529412+1.31176i;
    row[3]: 0.535294-0.0411765i;

pinv(A) * b : 3 x 1 Complex Matrix:
    row[1]: 0.170588+0.217647i;
    row[2]: -0.0529412+1.31176i;
    row[3]: 0.535294-0.0411765i;

pinv2(A) * b (== A \ b) : 3 x 1 Complex Matrix:
    row[1]: 0.170588+0.217647i;
    row[2]: -0.0529412+1.31176i;
    row[3]: 0.535294-0.0411765i;
```

## References
* Pierre Courrieu, [Fast Computation of Moore-Penrose Inverse Matrices](https://arxiv.org/abs/0804.4809)
* [Permute Sign Calculation, page5](https://www.math.rutgers.edu/docman-lister/math-main/academics/course-materials/250/assignments/1493-250c-lab3-sakai-pdf/file)
* [LU Decomposition C++ Implementation](https://blog.csdn.net/xx_123_1_rj/article/details/39553809)
* [LU Decomposition](https://www.math.ucdavis.edu/~linear/old/notes11.pdf)  

<p align="center">*** Project by Fanseline in Ericsson ***</p>
