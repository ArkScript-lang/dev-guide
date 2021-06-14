# Modules Architecture - DRAFT

*Last modification on the 14/06/2021 at <>*

## Definitions

_module_: C++ plugin for the ArkScript virtual machine, allowing use of C++ code (eg: the SFML)

## Precisions

The ArkScript modules need a consistent directory structure. Each module needs to be tested, and documented appriopriately.

## Directory structure

Each module should match the following structure:
```ignorelang
module
|___ documentation/
      |___ README.md
      |___ [other].md
|___ tests/
      |___ main.ark
      |___ [other].ark
|___ src/
|___ CMakeLists.txt
|___ README.md
```


# Authors

* [Fabien Zoccola](https://github.com/fabien-zoccola)