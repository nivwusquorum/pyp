### Preprocessor

Suited for simple preprocessing of Python files. The original use case of the package was annoying Cython work to do with data types. Notice that Preprocessor can run on any type of file (e.g. TSV, C++ sources etc.). To avoid clashes with other programming language preprocessor prefix and suffix is fully tunable.

#### Example
Here's a small file where we replicate the same Cython code 3 times - once for every of int, float and double datatypes. You can find the code for `typed_expression` function in `sample_utils.py` in this repo.

```Python
pyp
from sample_utils import typed_expression
ypy

cdef class Mat:
    cdef CMat[dtype] matinternal
    int dtype

    def sum(Mat self):
        # Inline preprcoessor expression. Executes a function.
        # Generally inline expressions should be one line,
        # But can be extended to multiple where all the data
        # on second line and following will be captured into
        # a string argument passed as last positional argument
        # to a function
        pypinline typed_expression(pyp, "self.matinternal", "CMat",
            print('siema')
            return WrapMat(TYPED_EXPRESSION.sum())
        ypy
```

The output of the preprocessed file looks like this:

```Python
cdef class Mat:
    cdef CMat[dtype] matinternal
    int dtype

    def sum(Mat self):
        # Inline preprcoessor expression. Executes a function.
        # Generally inline expressions should be one line,
        # But can be extended to multiple where all the data
        # on second line and following will be captured into
        # a string argument passed as last positional argument
        # to a function
        if self.dtype == np.int32:
            print('siema')
            return WrapMat((<CMat[int]>(self.matinternal)).sum())
        elif self.dtype == np.float32:
            print('siema')
            return WrapMat((<CMat[float]>(self.matinternal)).sum())
        elif self.dtype == np.float64:
            print('siema')
            return WrapMat((<CMat[double]>(self.matinternal)).sum())
        else:
            raise ValueError("Invalid dtype:" + self.dtype + " (should be one of int32, float32, float64)")
```

#### Command Line Interface

Preprocessor comes with a command line script, to easily run the preprocessor form environments outside of Python

```bash
preprocessor --input sample.py.pre --output sample.py
```
