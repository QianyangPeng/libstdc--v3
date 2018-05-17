# libstdcxx-v3-r
This is a modified libstdc++ library for gcc-8.1.0. It provided a randomized iterator API for the built-in <unordered_set> class.
# motivation
The motivation is to generate a randomized sequence of traversal of an unordered_set, thus the iteration through this data structure becomes "unordered".
# installation
1. Download the source code of gcc-8.1.0:

`wget http://mirrors-usa.go-parts.com/gcc/releases/gcc-8.1.0/gcc-8.1.0.tar.gz`

2. Configure the installation. **Do not** install this gcc into the default path:

`mkdir /test/gcc`

`./configure --prefix=/test/gcc`

3. Compile the code and install the package:

`make`

`make install`

4. Download this git repo, replace the folder `libstdc++-v3` in `gcc-8.1.0` with this folder:

`mv ./gcc-8.1.0/libstdc++-v3 ./gcc-8.1.0/libstdc++-v3-backup`

`cp -r ./libstdc--v3 ./gcc-8.1.0/libstdc++-v3`

5. Rebuild `libstdc++` in `gcc-8.1.0`:

`make all-target-libstdc++-v3`

`make install`

6. Now you can use the new gcc to compile any c++ code, with the iterating order of unordered_set randomized.

`/test/gcc/bin/gcc xxx.cpp -o xxx -lstdc++`

# usage
Here is a example of using the randomized iterator:
```cpp
// unordered_set_r
#include <iostream>
#include <string>
#include <unordered_set>
int main ()
{
  std::unordered_set<std::string> myset = { "red","green","blue" };
  for (auto itr = myset.rbegin(); itr != myset.rend(); ++itr) {
  	std::cout<< *itr << std::endl;
  }
  return 0;
}
```
And here is the result of re-running the program for multiple times:
```
blue
green
red
```
```
green
red
blue
```
```
red
green
blue
```
...
## Q&A
Q: Why we have to use `rbegin()` to initialize a randomized iterator rather than just using `begin()`?

A: We have to keep two kinds of iterators in the <unordered_set> class, a ramdomized_iterator and an original_iterator. The reason is because some other built-in functions of <unordered_set>, for example, `insert()`, returns an iterator indicating the result of insertion and the returned iterator could not be randomized. The main reason why we define a new function `rbegin()` rather than redefining the `begin()` function is to avoid confusions.
