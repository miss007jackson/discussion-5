# Week 7 Discussion  


## C++ Const

Write your text answers in the shared slides for discussion.

We've seen that the `const` keyword can be used in several places, which can be confusing.

Below is an incomplete, but compilable, snippet of code that represents a chemical reaction. This consists of `Molecule`s
that participate in the reaction and their stoichiometric coefficients.

```C++

class Reaction
{
    private:
    
      std::vector<Molecule> molecules_;
      std::vector<double> coefficients_;

    public:

      void add_component(Molecule mol, double coefficient)
      {
        molecules_.push_back(mol);
        coefficients_.push_back(coefficient);
      }
      
      const Molecule & get_component(int index) const
      {
        return this->molecules_.at(index); //this is similar to self in python
      }

      Molecule & get_component(int index)
      {
        return molecules_.at(index);
      }

      ~Molecule()
      {
          coords_.clear();
          num_atoms_ = 0;
      }
};

Reaction r(..);
const Molecule & m = r.get_component(0);
m.atoms.clear()

```

We see `const` used in 3 places:

1. What are the three different places, and what do they each mean? That is, what is the `const` referring to (what is being kept constant)?
We first see const at the beginning of the Molecule argument in line 30, which means that the function's return value can not be modified. The second const is at the end of this same line (line 30) at the end of the function, and this means that the function does not modify any member variables of the object on which it is called. The third const is at the beginning of line 48; similar to the first const, this indicates that the argument will not e modified after it's initial assignment and ensures that it remains constant throughout the scope.
 
2. Could you rewrite the `add_component` function to use `const`? Why or why not? 
Yes, the function will be: 
void add_component(const Molecule& mol, double coefficient)
When it pass mol by const with &, the mol is not copied and it gurantees the function will not modify it (safer). 


3. You can rewrite the first `get_component` function and get rid of one `const`. Which one? Is there a benefit to that?
'const' from the get_component could be taken off even though it is a good idea to keep both const with the get_component function. 


4. Is the second `get_component` function a good idea?
It is a good idea to have the second get_component function as it runs the code when the Molecule and get_component arguments are changed

5. How does the compiler know which `get_component` function to  use when it is called? 
The compiler first checks for the best match. If it finds the get_component to be const, first get_component function will be executed or else the second one will be executed.



## C++ Class Design

Write your text answers on the shared class slides, and modify the code in `main.cpp` as necessary.

1. What are the main differences between classes in C++ and classes in Python?
One of the main differences is the syntax; C++ needs explicit data types for it's members, while there's no need for this within python. C++ classes also uses access modifiers such as private, public and protected, while python does not.

2. What are some possible mistakes or bad design decisions in the following code?
a) In line 93, the 1/3 of coords size is turned to num_atoms_, which might coour error if coords size is not a multiple of 3.
b) The protected values can be modified by derived class, which can be avoided by changing it to private. It will reduce mistkes in derived class.
c) Line 98 takes coords_ but it can be more efficient with & to reduce unneccessary copying step. 

3. How could chemical bonds be represented in this class? Remember that bonds have an 'order' to them (single, double, triple, etc)
Probably, a better way to represent chemical bonds could be by caalling it as a class within this class. This way, we could specify methods for bond orders, bond lengths or bond energies.

4. What are some advantages and disadvantages to storing coordinates as a single (flattened) list?
Shannon
Some advantages might include more memory efficiency; since its more compact it reduces the space required for each coordinate. A single line is also more efficient for smaller lists as far as readability, whereas this would be a disadvantage for larger lists. Additionally, larger lists within a single line are more error-prone in regards to indexing / structure. The longer the list gets, the less flexibility you will when trying to modify the properties.

5. If you were to need to use a molecule class for a calculation, what other features would you like to have?
Some other features that could be helpful could be calculation would be measuring the distance between the atoms (helpful for bond lengths), measuring the center of mass (helpful for weighted average position), or even measuring the inertia tensor (helpful for simulating molecular relations).

6. What else could be done to improve this class? Modify the code in `main.cpp` to improve this class!



```C++
#include <vector>

class Molecule
{
    protected:
      int num_atoms_;

      // Coordinates are stored "flattened" as a single array
      std::vector<double> coords_;

    public:
      Molecule()
        : num_atoms_(0)
      {}

      Molecule(std::vector<double> coords)
        : coords_(coords)
        if (coords.size() % 3 != 0)
          std::invalid_argument("coords size is not a multiptle of 3.")
      {
        num_atoms_ = coords.size() / 3;
      }

      const std::vector<double> & get_coords(void) const { return coords_; }

      void set_coords(std::vector<double> coords)
      {
        coords_ = coords;
      }
};
```


## C++ Templates

C++ templates are often used to write functions and classes that are generic with respect to types (for example,
types of functions arguments or types of objects stored in classes). However, templates can also be used
to define compile-time integer variables. For example, the following code will run the `print_integer` function
with `I` = 3.

```C++
template<int I>
void print_integer()
{
  std::cout << "Integer is " << I << std::endl;
}

int main(void)
{
  print_integer<3>();
}
```


### Questions

1. What is the difference between this and just passing the argument in? When can you/can't you use this?
2. Can you think of an example where you have seen this already with classes? (Hint: It was a class that is part of the Standard Library).
3. In `main.cpp`, add a function that calculates a factorial of a template argument. What would be the advantage/disadvantage of this function?
4. Can you write code that calculates a factorial using no `if` statements or `for` loops using templates? Hint: You need a template specialization of a function.
