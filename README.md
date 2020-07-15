# Guidelines for using C++ in research projects.

This is a discussion document on using C++ for research projects. Goals of this document are to select C++ features that work well in a typical academic setting with changing PhD students that may require training, code bases that need to be easily adaptable for different research projects, and portability across a wide range of systems.

### Templates only for data types

The template machinery in C++ is extremely powerful. Yet, templates can easily lead to hard to understand code (somehow alleviated with contracts in C++20), overcomplex data types, difficult compiler error messages, and details of template mechanics or often hard to understand for non C++ experts.

For these reasons templates should only be used for data types.

An example is the following piece of code:

```
template<typename RealType>
void operateOnData(std::vector<RealType> myData){}
```

This function can be easily adapted to single and double precision, which is the most frequent use for a change of data type.

### Avoid object hierarchies. Use at most one level of inheritance

Complex object hierarchies can be a strong temptation to show off C++ coding skills. In practice, they lead to over-engineering and a diffult to adapt code structure. In particular, the only level of inheritance in most cases should be for the implementation of interfaces.

### Abstract classes only as interfaces, not as base for complex hierarchies

Abstract base classes should only be used in the sense of interface definition, not as base for complex object hierarchies.

### Never use new, delete, malloc or similar manual memory management features

Manual use of memory allocation is one of the most error-prone features of C/C++. Modern C++ offers a range of smart pointers that automate resource management. These should always be used. Apart from certain fringe cases (e.g. manual memory allocators), manual management of memory eventually always leads to errors.

### Do not use shared pointers. Always maintain unique ownership of resources

Shared pointers seem great. One can easily pass data around without worrying about object lifetime. However, we lose oversight of when objects are actually deleted, quickly leading to hard to debug cases in which memory we think that should have been released, is kept alive. By default we should always work with unique pointers and only pass out references to data when a function needs momentary access. This ensures that the lifetime of a resource remains bounded to the lifetime of the owning object and makes it easy to ensure that resources are released when objects go out of scope. The Rust programming language is completely designed around this ownernship model of resources.

### Warning free compilation on gcc and clang

Warning free compilation may sometimes be difficult. But we should always strife for it. Warnings in one compiler version can easily lead to non-compilation on a different compiler. Also, we should always make sure to not only develop for one compiler, but to at least test with the gcc and clang compiler families.

### Use modern C++ 17 syntax and avoid old style syntax

This includes in particular, range based loops, judicious use of lambda functions, and a preference for STL operations. The code will look cleaner, more readable, less error prone, and will be easier portable to STL like libraries such as TBB or SYCL for parallel execution.

### Design from the start with scripting languages in mind

A common mistake is to focus on C++ first, and then to discover that scripting language bindings are useful later on. Scripting languages should be part of the development process from the start. Most users want to use functions easily from Python or Jupyter without going through a CMake compilation loop first. For Python pybind11 is great for binding creation. But for generic scripting support, classical C interfaces to the C++ entry point functions are more generic. These allow easy binding to Python, Julia, or other modern languages such as Go and Rust.
