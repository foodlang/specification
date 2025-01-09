# The Food Specification
This repository contains the specification of Food. The specifications are the documents that officially explain the Food language,
and how it should be implemented to be officially compatible. Furthermore, this is the version that is currently targetted by CK, the official Food compiler.
 
## [Food 1.0](draft10/README.md)
Food 1.0 is the first version of the Food specification. It lacks a lot of information (it is more or less left to the implementation.)
It is designed for writing basic implementations of Food.

## About Extensions
Extensions can be applied to an implementation to extend Food's features beyond the official specification. For the implementation to remain
officially compliant, they must not prevent the compilation or execution of valid official source code. Moreover, the source code should output code that works as intended.
If additionnal keywords are declared by the extension, they should have a `__keyword` notation.

## About Downloading
If you wish to download a version of the specification, please download the whole folder, as it is split across multiple files.
Drafts are usually still worked on (except for Food 1.0) and can change at any moment.

## License
*The Official Food Specification is licensed under the MIT license.*
