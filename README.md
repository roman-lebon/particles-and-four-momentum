# Particles and four-momentum

## Highlights

- Implements a `Particle` class composed of a `FourMomentum` class (composition)
- Full Rule of Five for both classes (deep copy, move semantics)
- Operator overloading for four-momentum addition and dot product
- Encapsulated private data with validated getters and setters
- Separated interface (`.h`) and implementation (`.cpp`) files
- Demonstrates copy construction, copy assignment, move construction, and move assignment in main

## Overview

This program models Standard Model particles, each carrying a relativistic four-momentum vector. It demonstrates core OOP concepts - composition, encapsulation, operator overloading, and the Rule of Five - within a particle physics context.

Four-momenta are expressed in natural units (c = 1) with energy and momentum measured in MeV. The main program creates a collection of electrons, muons, and taus, then demonstrates four-momentum addition, the dot product, and all four special member functions (copy/move constructors and assignment operators).

## Class Structure

### FourMomentum

Represents the relativistic four-momentum P = (E, p_x, p_y, p_z).

Data is stored via a pointer to a dynamically allocated `std::vector<double>`. This dynamic allocation means the compiler-generated defaults would produce shallow copies, so the full Rule of Five is implemented to ensure correct deep copy and move behaviour.

Member functions:
- Default and parameterised constructors (parameterised validates E ≥ 0)
- Destructor (frees dynamically allocated memory)
- Copy constructor and copy assignment operator (deep copy with self-assignment check)
- Move constructor (transfers pointer ownership, nulls source) and move assignment operator (swaps pointers)
- Getters and setters (energy setter validates E ≥ 0)
- Overloaded `+` operator for component-wise four-momentum addition
- Dot product: E₁E₂ − p_x₁p_x₂ − p_y₁p_y₂ − p_z₁p_z₂

### Particle

Represents a Standard Model particle with a name and four-momentum.

Uses composition — a particle *has a* four-momentum rather than *being* one. The `FourMomentum` is stored as a direct member (not a pointer), so `Particle`'s Rule of Five delegates memory management to `FourMomentum`'s implementations.

Member functions:
- Default constructor (sets name to "unknown") and parameterised constructor
- Destructor, copy constructor, copy assignment, move constructor, move assignment
- Getters and setters (name setter validates against a list of recognised SM particles)
- Overloaded `+` operator returning a new particle with name "combined"
- Dot product delegated to `FourMomentum`

### Recognised Particles

The `set_name` function validates particle names against a list of Standard Model particles including leptons (electron, muon, tau and their antiparticles and neutrinos), gauge bosons (photon, Z, W, Higgs), and quarks. The program exits with an error message if an unrecognised name is provided.

## Physics Background

In special relativity, the four-momentum of a particle is a four-component vector P = (E/c, p_x, p_y, p_z) that combines the particle's energy and three-momentum into a single object. Using natural units where c = 1, this simplifies to P = (E, p_x, p_y, p_z), with both energy and momentum measured in MeV.

The dot product of two four-momenta is defined as E₁E₂ − p_x₁p_x₂ − p_y₁p_y₂ − p_z₁p_z₂. When a particle's four-momentum is dotted with itself, the result gives the invariant mass squared (m²), a quantity that is the same in all reference frames. Four-momentum addition is used when combining particle systems, for example when studying the products of a particle decay.

## Usage

To use these classes in your own `main()`, include the header files and link the implementation files at compilation:

```cpp
#include "particle.h"
#include "fourmomentum.h"
```

Creating a particle requires a valid Standard Model particle name and four-momentum values (E, p_x, p_y, p_z) in MeV:

```cpp
Particle electron("electron", 0.511, 0.1, 0.2, 0.0);
```

If an invalid particle name or negative energy is provided, the program will print an error message and exit.

Individual properties can be accessed and modified through getters and setters:

```cpp
double energy = electron.getE();
electron.setp_x(0.5);
```

Two particles can be summed using the `+` operator, which returns a new particle with the combined four-momentum:

```cpp
Particle combined = particle1 + particle2;
```

The dot product of two particle four-momenta is computed using:

```cpp
double result = particle1.dotProduct(particle2);
```

Particles can be stored in a `std::vector<Particle>` and iterated over using range-based for loops. Copy and move operations work correctly due to the Rule of Five implementations.

## Design Decisions

The `Particle` class contains a `FourMomentum` member (composition), and the four-momentum is implemented as a pointer to a dynamically allocated `std::vector<double>` as specified in the assignment brief. Components are pushed back in the order E, p_x, p_y, p_z, but the internal ordering is hidden from the user — access is provided only through named getters and setters (e.g. `getE()`, `setp_x()`). Because the class manages a raw pointer, the full Rule of Five is implemented to ensure correct deep copy and move behaviour.
 
Energy is validated as non-negative in both the parameterised constructor and the setter, since negative energy is unphysical. Particle names are validated against a fixed list of Standard Model particles. Invalid inputs cause the program to exit with an informative error message.
## Compilation

Compiled using g++ with the C++17 standard:

```bash
g++-11 -std=c++17 -o assignment4_test assignment4.cpp particle.cpp fourmomentum.cpp
```

Run:

```bash
./assignment4_test
```

## Development Process

1. Created the `FourMomentum` class with basic constructor and destructor
2. Added FourMomentum getters, setters, print function, and energy validation
3. Created the `Particle` class with basic constructor and destructor
4. Added Particle getters, setters, name validation, and print function
5. Implemented Rule of Five for `FourMomentum` (deep copy, move semantics, self-assignment protection)
6. Implemented Rule of Five for `Particle`, delegating to `FourMomentum`'s special member functions
7. Added overloaded `+` operator and dot product to both classes
8. Built full `main()` with all required tests: particle creation, four-momentum addition, dot product, copy assignment, copy construction, move construction, and move assignment

## Use of Generative AI

This assignment was completed without the use of generative AI tools.
All program code and documentation, including this README, were composed independently.

## Author

Roman le Bon  
University of Manchester
