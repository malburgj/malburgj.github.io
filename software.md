---
title: Software Development
layout: template
filename: software
--- 
### Static Code Analysis
Using static code checker tools can help improve one's software coding and, if enforced, ensures only compliant code is checked into repositories.  The standards used by companies, schools, etc varies.  MIRSA and AUTOSAR standards are common in industries like automotive.  Google has its on [style guide](https://github.com/google/styleguide).  The following are static code checker commonly used today.

#### [Cpplint](https://github.com/cpplint/cpplint)
Open source tool developed by Google Inc, now largely maintained by the user community.

*   Install using the python package manager (`python -m pip install --user cpplint`).
*   Passing the `--help` argument returns really useful information.
*   An example cpplint configuration file can be found [here](https://github.com/malburgj/malburgj.github.io).
*   To run cpplint on a single file: `cpplint main.c`
*   To run cpplint on a directory: `cpplint src`
*   To run cpplint on a directory recursively: `cpplint recursive src`
*   To run cpplint using the settings specified by a configuration file, simply put the configuration in the top folder: `cpplint src`
*  

#### [CppCheck](https://github.com/danmar/cppcheck)


### Test Frameworks
I'm currently using a custom, makeshift framework but want to start using a widely used, light-weight framework.  Since I'd like to work close to the / on embedded systems, the framework should be light-weight so its capable of running on small microcontrollers.  Using a widely used framework means I'm lowering the risk of having to trash prior exprience.   Finish reading [this](https://interrupt.memfault.com/blog/unit-testing-basics) and [this](https://testing.googleblog.com/2014/07/measuring-coverage-at-google.html).
*   [Google Test](https://github.com/google/googletest)
*   Boost Test Library
*   [Catch2](https://github.com/catchorg/Catch2)
*   CppUTest
*   Bandit

### Test Metrics
Need to do more research in this area.  Would like to find a tool that provides a graphical interfave and same capability as [Simulink Coverage](https://www.mathworks.com/products/simulink-coverage.html) toolbox.
*   [Gcov](https://gcc.gnu.org/onlinedocs/gcc/Gcov.html)