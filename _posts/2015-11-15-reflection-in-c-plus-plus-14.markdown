---
layout: post
title: "Reflection in C++14"
date: 2015-11-15 20:10:31 +0100
description: Explanation of my implementation of a simple reflection engine in C++14
cover: /assets/header_image.jpg
categories: c++ english
author: Simon Ninon
---

# What Am I going to talk about?

A few months ago, I was developing an MVC web framework in C++, mostly inspired by the Ruby on Rails framework. Apart the fact that, like most of the side projects, it's been a while I haven't coded any line for this project, it leads me to some interesting problematics.

The problematic that is interesting here and that can introduce this article concerns the configuration of the routes. Indeed, one of the most important feature of an MVC web framework is to configure some routes that the client can request. Each route forward the request to a specific action of a controller.

For example, we can have a `/articles` route which is linked to the `index` action of the `articles` controller. When the client requests this path, `articles_controller::index` is called and returns a list of the articles.

For my MVC project, I wanted it to be easy to configure the routes and to hide this part from the user. I didn't want the developer to declare his routes by writing C++ code: it would have been less convenient, less readable and maybe less maintainable.

{% highlight c++ %}
router.add(Route{ HTTP::Method::GET, "/articles" }, &articles_controller::index);
router.add(Route{ HTTP::Method::POST, "/articles" }, &articles_controller::create);
router.add(Route{ HTTP::Method::GET, "/articles/:id" }, &articles_controller::show);
router.add(Route{ HTTP::Method::PUT, "/articles/:id" }, &articles_controller::update);
router.add(Route{ HTTP::Method::DELETE, "/articles/:id" }, &articles_controller::destroy);
{% endhighlight %}

Instead, I'd prefer to have a simple JSON configuration file where the developer can easily declare its routes.

{% highlight json %}
{
  "/articles": [
    { "method": "GET", "to": "articles#index" },
    { "method": "POST", "to": "articles#create" }
  ],
  "/articles/:id": [
    { "method": "GET", "to": "articles#show" },
    { "method": "PUT", "to": "articles#update" },
    { "method": "DELETE", "to": "articles#destroy" }
  ]
}
{% endhighlight %}

As you can see, by using this simple JSON configuration file, it would be really easy and readable to declare routes and to associate them to a specific action of a controller.

This idea directly leads us to the topic of this article: how to convert the string `articles#index` to the call `ArticlesController(...).index(...)`. The answer is Reflection.

Reflection is, in some languages, a part of the language that gives us the ability to retrieve information from a type or a method, dynamically, at runtime. And moreover, reflection lets us instantiate an object from a string containing the class name or call a method from a string containing its name. For example, in Ruby on Rails, we can do `"SomeClass".constantize.new.send "some_method"` and it will call `SomeClass::some_method` at runtime.

However, there is no reflection engine in C++, even in the latest version. So, I had to build my own reflection engine and I'm going to explain how I did in this paper.

<!-- more -->

# [UPDATE] Before reading!

Some people mentioned in the comments that the final implementation is clearly not optimal and that reflection should be avoided, especially in C++ since the language does not support such features.

It's important to understand that the point of this blog post is not to provide an implementation usable in real-world applications!

I just thought about what I explained in the introduction of this article and decided to challenge myself to see if I was able to provide my own implementation of a reflection engine in C++, mostly by tricking the language. I successfully achieved to do so but I clearly do not recommend to use that kind of thing in applications intended to be used in a production environment.

It's just for fun and I hope you'll have some fun while reading the next parts of this post!

# Possible implementations

In this part of the article, I will shortly describe some alternatives to my implementation. I will also explain why I didn't choose them: these explanations are, of course, arguable depending on your needs.

## dlopen

One simple way to make reflection in C++ is to use the `dlopen` and `dlsym` functions (or their equivalent functions on Windows).

It is indeed possible to load the current process symbols. Here is a citation of the dlopen man: `If a null pointer is passed in path, dlopen() returns a handle equivalent to RTLD_DEFAULT.`. And, here is a citation of the dlsym man: `If dlsym() is called with the special handle RTLD_DEFAULT, then all mach-o images in the process (except those loaded with dlopen(xxx, RTLD_LOCAL)) are searched in the order they were loaded.`.

{% highlight c++ %}
#include <dlfcn.h>
#include <iostream>

extern "C" {

  void display_nb(int nb) {
    std::cout << nb << std::endl;
  }

}

int main(void) {
  auto handler = dlopen(nullptr, RTLD_LAZY);
  auto fct = reinterpret_cast<void(*)(int)>(dlsym(handler, "display_nb"));

  fct(42);

  return 0;
}
{% endhighlight %}

In the above example, I simply call `dlopen` with a null pointer. Then, I'm able to retrieve the `display_nb` symbol of my short program.

But, you may have noticed the presence of `extern "C"` block around the definition of the `display_nb` function. This is due to the fact that symbols, in C++, are `mangled`. For a simple function like our `display_nb`, the symbol name is not really different from the original name (here, the symbol will be named `_display_nb`). But for member functions, the mangled symbols are really different from the original one (for example, the `SomeClass::do_something(const std::string&)` member function will be stored as the `__ZN9SomeClass12do_somethingERKNSt3__112basic_stringIcNS0_11char_traitsIcEENS0_9allocatorIcEEEE` symbol).

Well, this behavior is a real constraint. There are some workaround, but this is not really convenient and using `dlopen` can be an heavy operation at runtime.
However, the real advantage of this implementation choice is that every symbol can be used for reflection, even variables.
And nothing has to be manually registered because every symbol is stored in the executable and can be retrieved by dlopen.

But, I think the main reason I did not select this option to build my library is type-safety. By loading symbols, we have absolutely no type-safety. We may load an integer symbol but use it as a function in our program: it will compile without any warning. Even at runtime, we won't be able to detect if the symbol is cast in the right type and this will result in a crash. I think it's a real downside, especially when we are dealing with C++, a strong-typed language.

An example of reflection library using `dlopen`: [Reflect](https://github.com/jeremyletang/reflect)

## code generation

This is for sure the implementation I like the least. It concerns code generation, like Qt does.

So, the user register manually the classes and functions he wants to use for reflection (by using macros for example). Then, before the compilation, a tool just generates the code needed for the reflection at runtime.

One advantage of this method is that you can do nearly anything you want by injecting the code you need. However, I do not really like the idea to add a step before the compilation.

An example of reflection library using `code generation`: [CPP-Reflection](https://github.com/AustinBrunkhorst/CPP-Reflection)

# My implementation

## Preamble

Before diving inside my implementation, it is important to understand one thing: `registration`.

Because we do not load the symbols of the current process dynamically by calling `dlopen` or an equivalent, and because we do not use code generation, we have to explicitly declare which classes and functions should be used for reflections.
If we do not register these classes and methods to our reflection engine, we would have no way to operate reflection (except loading symbols, something that we do not want).

When I say "register classes and functions", I mean something like this `REGISTER_CLASS(SomeClass)`, `REGISTER_FUNCTION(SomeClass::some_function)`.

## My constraints

If I had not chosen one of the implementations presented before, it's because there are some things I want to do and others I don't.

* I want type-safety. That said, I want to be warned if I try to use a function `void (*)(const std::string&)` as if it was a function `int (*)(double)`.
* Reflection can be processed from anywhere: from the first line to the last line of the main function.
* Even if the user has to register his classes and functions, I want that he could do it with as little code as possible and with no way to make errors.
* I want reflection to work on member functions, static member functions and c-style functions.
* I want reflection to work for types declared within namespaces.
* I want to be able to operate member function reflection on new instance or on specific instance.

This is a short constraints list and it guided me during the development by keeping in mind the final goal.

## How I did?

### Static variables

As I said in the constraints part, I want that the registration process should be as easy as possible and that reflection can be operated from anywhere in the code (starting from the first line of the main).

{% highlight c++ %}
int main(void) {
  REGISTER_CLASS(SomeClass)
  REGISTER_MEMBER_FUNCTION(SomeClass::some_function)
  // ...
}
{% endhighlight %}

In the above code snippet, you can read a perfect example of what I want to avoid. The problem here is that the developer has to explicitly declare his classes and member functions from one of his own functions or from the main.
The problem with that behavior is that if the developer use reflection on a type before registering this type, it will fail. Moreover, it is not really convenient for the developer to think to register a specific class at program startup.

We will need to automate this step so that the developer does not have to think about it.

And for this, we have static variables! Static variables are initialized automatically at program startup. The following example demonstrates it very easily:

{% highlight c++ %}
#include <iostream>

class A {
public:
  A(void) { std::cout << "A::A()" << std::endl; }
};

static A a;

int main(void) {
  std::cout << "main()" << std::endl;
  return 0;
}
{% endhighlight %}

{% highlight bash %}
A::A()
main()
{% endhighlight %}

So, what if we just simply use static variables to initialize our reflection engine? We can declare a static variable for each class and function we want to register. This will move the registration code from the main (or any other function) to a static member function that will handle the registration during its construction.

{% highlight c++ %}
class reflectable {
public:
  //! constructor of a reflectable class where we can process the registration
  reflectable(/* ... */) {
    //! registration of a class or member function here
  }
};

static reflectable register_some_class;

int main(void) {
  //! process reflection
}
{% endhighlight %}

### Manager

Ok, now we know how to handle the registration: with static variables.

However, it leads to a problem: we have a static variable for each class or function. So, the information is not centralized at one place: not really convenient for the final usage.

Just imagine we have registered 10 classes for registration: we don't want to find by ourselves which static variable contains the data about the type we want to use for reflection. We just simply want to do something like `reflect("SomeClass::some_function")`.

So we need to centralize the information somewhere, in a single place: a single object that contains all the reflection information at any time of the program execution. I've called it the `reflection_manager`.
Of course, this reflection_manager will be implemented as a singleton in order to be sure that only one instance of the manager can be alive and that all the information is stored in this object.

The idea is simple: each reflectable registers itself to the reflection_manager during its construction. Then, the developer calls the reflection_manager to operate the reflection.

{% highlight c++ %}
class reflection_manager {
public:
  ~reflection_manager(void) = default;
  reflection_manager(const reflection_manager&) = delete;
  reflection_manager& operator=(const reflection_manager&) = delete;

  static reflection_manager& get_instance(void) {
    static reflection_manager instance;
    return instance;
  }

  void register_reflectable(reflectable& r) {
    // store the reflectable
    this->reflectables.push_back({ r });
  }

  void process_reflection(const std::string& class_name) {
    // process reflection here
  }

private:
  reflection_manager(void) = default;

  std::vector<std::reference_wrapper<reflectable>> reflectables;
};

class reflectable {
public:
  //! constructor of a reflectable class where we can process the registration
  reflectable(const std::string& class_name) {
    reflection_manager::get_instance().register_reflectable(*this);
  }
};

static reflectable register_some_class("SomeClass");

int main(void) {
  reflection_manager::get_instance().process_reflection("SomeClass");
}
{% endhighlight %}

The above code is just to make you understand the main idea behind the reflection manager: information centralization.

### Register Class

Now, we have static variables to automatically register classes and functions at program startup, and we have a manager which centralizes the information and that can be used by the user to operate reflection on the registered types.

This is a good base to go further in the registration process. So let's begin with class registration.

In order to be able to retrieve the type at runtime to operate reflection, we will need to store it in our `reflectable` class.
This can simply be done by using templates: we template our reflectable class on the type we want to use for reflection and that's all.
Moreover, we will also need the type's name. If we don't have it, we won't be able to retrieve the type by its name when the user will call our reflection engine with only a string containing the class name.

Let's update our reflectable class with these elements:

{% highlight c++ %}
template <typename T>
class reflectable {
public:
  //! constructor of a reflectable class where we can process the registration
  reflectable(const std::string& class_name) : name(class_name) {
    reflection_manager::get_instance().register_reflectable(*this);
  }

  //! function to get reflectable name
  const std::string& get_name(void) const {
    return this->name;
  }

private:
  std::string name;
};

static reflectable<SomeClass> register_some_class("SomeClass");
{% endhighlight %}

There is however a problem now: the manager can't store all the reflectable instances in the same container because of the addition of the template.
So, if we want to have a unique container containing all the reflectable objects, we will need to create an abstraction of the reflectable type.

{% highlight c++ %}
class reflectable_base {
public:
  virtual ~reflectable_base(void) = default;

  virtual const std::string& get_name(void) const = 0;
};

template <typename T>
class reflectable : reflectable_base {
public:
  //! constructor of a reflectable class where we can process the registration
  reflectable(const std::string& class_name) : name(class_name) {
    reflection_manager::get_instance().register_reflectable(*this);
  }

  //! function to get reflectable name
  const std::string& get_name(void) const {
    return this->name;
  }

private:
  std::string name;
};
{% endhighlight %}

Then, instead of storing reflectable objects, we will simply store reflectable_base objects in our manager.

{% highlight c++ %}
class reflection_manager {
public:
  ~reflection_manager(void) = default;
  reflection_manager(const reflection_manager&) = delete;
  refleection_manager& operator=(const reflection_manager&) = delete;

  static reflection_manager& get_instance(void) {
    static reflection_manager instance;
    return instance;
  }

  void register_reflectable(reflectable_base& r) {
    // store the reflectable
    this->reflectables.push_back({ r });
  }

  void process_reflection(const std::string& class_name) {
    for (const auto& reflectable : this->reflectables)
        if (reflectable.get().get_name() == class_name) {
          // process reflection here
        }
  }

private:
  reflection_manager(void) = default;

  std::vector<std::reference_wrapper<reflectable_base>> reflectables;
}

int main(void) {
  reflection_manager::get_instance().process_reflection("SomeClass");
}
{% endhighlight %}

Simple isn't it?

### Register Member functions

Registering classes is good, but it is not enough: we can't really do anything with this and we need to go further: registering member functions.

Before, I've said that we could use one static reflectable object per class or function. In fact, this is maybe not the best idea:

* It is painful to write
* It makes a looot of static variables

A more interesting approach would be store member functions of one class in the reflectable object of this class. The reflectable object will contain class information and its member functions information.

We will need to update our reflectable constructor in order to pass member functions information. For each function, we will need its name in a string and a pointer to it.
The most important point here is that we can have a variable number of member functions. Some classes will be associated with only one member function, but another one may be associated with 4, 5, 6 or more member functions.

We may store all of this information in a container (for example a vector of `pair<function_name, function_pointer>`), but each function will have its own signature, so it is complicated to store everything in the same container without losing any information, especially the signature of each function.

So, we need to operate differently: for example, taking a variable number of parameters in our reflectable constructor. This is absolutely possible and very easy to integrate thanks to C++11 variadic templates.

{% highlight c++ %}
template <typename T>
class reflectable : reflectable_base {
public:
  //! constructor of a reflectable class where we can process the registration
  //! note the presence of variadic template to accept any number of member functions
  template <typename... Fcts>
  reflectable(const std::string& class_name, Fcts... fcts) : name(class_name) {
    reflection_manager::get_instance().register_reflectable(*this);
  }

  //! function to get reflectable name
  const std::string& get_name(void) const {
    return this->name;
  }

private:
  std::string name;
};

static reflectable<SomeClass> register_some_class("SomeClass",
                                                  std::make_pair(std::string("some_fct_1"), &SomeClass::some_fct_1),
                                                  std::make_pair(std::string("some_fct_2"), &SomeClass::some_fct_2));
{% endhighlight %}

As you can see, we can now pass any member function as we want to our reflectable constructor. Functions can have any signature, this won't be a problem.
You may notice that the declaration of the static reflectable object begins to be a little bit longer and annoying to do, but we will improve that later.

Now, we have passed our member functions to our reflectable class. This is good, but not enough: we now need to store them somewhere to use them later for reflection.

This is maybe the tricky part because we will need to store all our information in the same container: a vector of functions for example.
That said, we need to abstract their types but, by doing this, we will loose the signature information.

But let's remember the goal. Our goal is to operate reflection with type safety. So, to have type safety, we need to be able to detect, at runtime, that we are trying to operate reflection on a function taking a string as parameter but that we are trying to use it with an integer parameter for example.
So, we don't really need to know the signature of the member function when we are storing it: we just need to know it at runtime when we want to make reflection.

Is that possible? Yes, by using inheritance and dynamic_cast.

First, we can make a simple class-container, let's say `function<ReturnType(Params...)>` where the function class is templated on the return type and on the parameters types (just like std::function).
Then, this class will inherit from a base class, `function_base`, which is not templated.
This way, we can have a container of function_base objects: of course, we have now lost the information about the member functions signatures. However, we don't really care about it.

What we want is to detect a function signature mismatch between what is stored and what we are trying to do at runtime during the reflection process.
And this can be achieved by using danymic_cast on the function_base object to cast it in its original type function<ReturnType(Params...)>.

For example, we will try to do `make_reflection<void(int, double)>("SomeClass::some_fct")`. By specifying the expected signature of the function SomeClass::some_fct, we can try to downcast from `function_base` to `function<void(int, double)>`.
In case of success, well, the cast will work and everything will be good.
However, if there is a mismatch, the dynamic_cast will fail, resulting in an exception (or returning null pointer depending on whether we are dealing with references or pointers). And by detecting this fail, we will be able to say that reflection was not used correctly without any crash.

This is a lot of text and it may be a little hard to follow what I'm saying, so let's see some code to understand what I mean.

{% highlight c++ %}
//! our function base class, non-templated.
//! it does really nothing, except helping us to store functions in the same container
class function_base {
public:
  virtual ~function_base(void) = default;
};

//! our function class, templated on return and parameters types
template <typename ReturnType, typename... Params>
class function;

template <typename ReturnType, typename... Params>
class function<ReturnType(Params...)> : public function_base {
public:
  //! our constructor
  //! we simply take an std::function as parameter and store it internally
  //! in fact, our function class is just a wrapper of std::function which inherits from function_base, nothing more
  function(const std::function<ReturnType(Params...)>& f) : f(f) {}

  //! a callable operator to be able to call our function
  ReturnType operator()(Params... params) {
      return f(params...);
  }

private:
  std::function<ReturnType(Params...)> f;
};
{% endhighlight %}

That's all for our function and function_base classes. In fact, as you can see, our function class is nothing more than a wrapper of std::function which inerits from function_base. This inheritance lets us store functions of different signatures in the same container.

{% highlight c++ %}
template <typename Type>
class reflectable : reflectable_base {
public:
  //! constructor of a reflectable class where we can process the registration
  //! note the presence of variadic template to accept any number of member functions
  template <typename... Fcts>
  reflectable(const std::string& class_name, Fcts... fcts) : name(class_name) {
    reflection_manager::get_instance().register_reflectable(*this);
    register_function(fcts...);
  }

  //! iterate over the parameters pack
  template <typename Head, typename... Tail>
  void register_function(const Head& head, Tail... tail) {
    register_function(head);
    register_function(tail...);
  }

  //! register specific member function
  template <typename ReturnType, typename... Params>
  void register_function(const std::pair<std::string, ReturnType (Type::*)(Params...)>& f) {
    //! wrap the instanciation of a new object of type Type and the call to the function f in a C++11 lambda
    auto f_wrapper = [f](Params... params) -> ReturnType {
      return (Type().*f.second)(params...);
    };

    //! store function information
    functions.push_back({
      f.first, //! function name
      std::make_shared<function<ReturnType(Params...)>>(f_wrapper) //! function object
    });
  }

  //! member functions getter
  const std::vector<std::pair<std::string, std::shared_ptr<function_base>>>& get_functions(void) const {
    return this->functions;
  }

  //! function to get reflectable name
  const std::string& get_name(void) const {
    return this->name;
  }

private:
  std::string name;
  std::vector<std::pair<std::string, std::shared_ptr<function_base>>> functions;
};
{% endhighlight %}

Wahoo, that was short but intense. Maybe I should give some explanations about the previous piece of code.

{% highlight c++ %}
//! iterate over the parameters pack
template <typename Head, typename... Tail>
void register_function(const Head& head, Tail... tail) {
  register_function(head);
  register_function(tail...);
}
{% endhighlight %}

First, we call the register_function in our constructor (`register_function(fcts...);`). This function is here to "unpack" the variadic parameters. It's not the topic of this article to explain how variadic templates work and how they should be used, so I won't give more explanations about it. If you are not really familiar with this modern feature of C++, I advise you to give a look at some articles treating this subject: it's a really important notion to know when developing softwares in C++.

{% highlight c++ %}
//! register specific member function
template <typename ReturnType, typename... Params>
void register_function(const std::pair<std::string, ReturnType (Type::*)(Params...)>& f) {
  //! wrap the instanciation of a new object of type Type and the call to the function f in a C++11 lambda
  auto f_wrapper = [f](Params... params) -> ReturnType {
    return (Type().*f.second)(params...);
  };

  //! store function information
  functions.push_back({
    f.first, //! function name
    std::make_shared<function<ReturnType(Params...)>>(f_wrapper) //! function object
  });
}
{% endhighlight %}

Ok, so what are we doing here?

First, we build a lambda with the same signature than the member function we are treating (`[f](Params... params) -> ReturnType {}`).
In this lambda, we create an object of type Type, the class we are currently registering for reflection (`Type()`), and we call our member function on this newly created instance (`(Type().*f.second)(params...)`).
Once again, if you are not familiar about recent C++ features like lambdas, I advise you to give a look to articles treating these subjects.

An interesting point about lambdas is that we can use them to initialize std::function instances. And, you know what? Our function class requires a std::function object in its constructor!

And it's exactly what we are going to do: initialize a function object with our lambda (`std::make_shared<function<ReturnType(Params...)>>(f_wrapper)`), associate it with the function name (`{ f.first, function_object }`) and store it in our functions list (`functions.push_back(...)`).

Very good, our member functions are now stored and ready to be used: we just have to add the missing reflection code, especially the dynamic_cast part that I've mentioned earlier.

### Make reflection

Let's add the reflection!

{% highlight c++ %}
class reflection_manager {
public:
  ~reflection_manager(void) = default;
  reflection_manager(const reflection_manager&) = delete;
  reflection_manager& operator=(const reflection_manager&) = delete;

  static reflection_manager& get_instance(void) {
    static reflection_manager instance;
    return instance;
  }

  void register_reflectable(reflectable_base& r) {
    // store the reflectable
    this->reflectables.push_back({ r });
  }

  template <typename ReturnType, typename... Params>
  ReturnType reflect(const std::string& class_name, const std::string& function_name, Params... params) {
    for (const auto& reflectable : this->reflectables)
        if (reflectable.get().get_name() == class_name) {
          //! get the member functions of the class
          const auto& functions = reflectable.get().get_functions();

          //! iterate over the functions to find the one we are interested in
          for (const auto& f : functions)
            if (f.first == function_name) {
              //! dynamic pointer cast to ensure type safety
              auto function_with_real_type = std::dynamic_pointer_cast<function<ReturnType(Params...)>>(f.second);

              //! type mismatch if dynamic_cast has failed
              if (not function_with_real_type)
                throw std::runtime_error(class_name + "::" + function_name + " called with the wrong signature");

              //! call our function: reflection is done!
              return (*function_with_real_type)(params...);
            }

          throw std::runtime_error(class_name + "::" + function_name + " is not registered for reflection");
        }

        throw std::runtime_error(class_name + " is not registered for reflection");
  }

private:
  reflection_manager(void) = default;

  std::vector<std::reference_wrapper<reflectable_base>> reflectables;
};

int main(void) {
  reflection_manager::get_instance().reflect<void, int, double>("SomeClass", "some_fct", 1, 4.2); // will call SomeClass().some_fct(1, 4.2);
}
{% endhighlight %}

I think the code speaks from himself here: we retrieve the member functions, we search the function the user asked. If we found it, we try to downcast from function_base to function object. And if the dynamic cast worked, then, we can call our function!

At this step, reflection has been operated! Cool, isn't it?

There is however a detail I don't really appreciate and you may have noticed it: `reflect<void, int, double>`. It is not really readable compared to the syntax `reflect<void(int, double)>`.
But this syntax can't be applied to member functions.

We can still however trick the language by creating a structure which wraps the call to reflection_manager::reflect<>. This structure will be templated on the function signature, just like the reflect member function. But here, we will be able to apply the std::function-template-design.

{% highlight c++ %}
template <typename ReturnType, typename... Params>
struct reflection_maker;

template <typename ReturnType, typename... Params>
struct reflection_maker<ReturnType(Params...)> {
  static ReturnType invoke(const std::string& class_name, const std::string& function_name, Params... params) {
      return reflection_manager::get_instance().reflect<ReturnType, Params...>(class_name, function_name, params...);
  }
};

int main(void) {
  reflection_maker<void(int, double)>::invoke("SomeClass", "some_fct", 1, 4.2); // will call SomeClass().some_fct(1, 4.2);
}
{% endhighlight %}

Better don't you think?

### Macros

There is a last step, really important, that I want to talk about.

As I said in the beginning, I want the registration process to be easy and as little error prone as possible.

However, if you remember our currently initialization process, it's quite heavy and painful:

{% highlight c++ %}
static reflectable<SomeClass> register_some_class("SomeClass",
                                                  std::make_pair(std::string("some_fct_1"), &SomeClass::some_fct_1),
                                                  std::make_pair(std::string("some_fct_2"), &SomeClass::some_fct_2));
{% endhighlight %}

Not really convenient.

One way to simplify this is to use macros with the help of boost preprocessor library.

Boost preprocessor will help us to build macros with variadic number of arguments (for member functions), to iterate over these arguments and to build the static variable.

{% highlight c++ %}
//! macro to convert any kind of value to string
#define __REFLEX_TO_STRING(val) #val

//! macro called for each member function to build a pair of <string, member_function_pointer>
#define __REFLEX_MAKE_REGISTERABLE_FUNCTION(r, type, i, function) \
BOOST_PP_COMMA_IF(i) std::make_pair(std::string(__REFLEX_TO_STRING(function)), &type::function)

//! main macro who build the static variable
#define REGISTER_CLASS(type, functions) \
static reflectable<type> \
reflectable_##type(#type, BOOST_PP_SEQ_FOR_EACH_I( __REFLEX_MAKE_REGISTERABLE_FUNCTION, \
                                                   type, \
                                                   functions ));
{% endhighlight %}

So, let's dive into the explanation.

{% highlight c++ %}
//! main macro who build the static variable
#define REGISTER_CLASS(type, functions) \
static reflectable<type> \
reflectable_##type(#type, BOOST_PP_SEQ_FOR_EACH_I( __REFLEX_MAKE_REGISTERABLE_FUNCTION, \
                                                   type, \
                                                   functions ));
{% endhighlight %}

We define a macro which takes as parameters the type of the class and the functions names.
These functions will be past as a boost preprocessor list: `(val1)(val2)(val3)`.

This macro simply declares a static variable of type `reflectable<type>`, gives a unique name to this variable by using the class type name (`reflectable_##type`) and call the constructor with the appropriate params: class name and member functions.

But, of course, member functions can't be passed with the actual boost preprossor list syntax (`(val1)(val2)(val3)`). We need to convert from `(val1)(val2)(val3)` to `{"val1", &SomeClass::val1}, {...}, ...`.

For this, we use the boost PP macro, `BOOST_PP_SEQ_FOR_EACH_I`, to call our own macro `__REFLEX_MAKE_REGISTERABLE_FUNCTION` for each function of the list.

{% highlight c++ %}
//! macro to convert any kind of value to string
#define __REFLEX_TO_STRING(val) #val

//! macro called for each member function to build a pair of <string, member_function_pointer>
#define __REFLEX_MAKE_REGISTERABLE_FUNCTION(r, type, i, function) \
BOOST_PP_COMMA_IF(i) std::make_pair(std::string(__REFLEX_TO_STRING(function)), &type::function)
{% endhighlight %}

This macro is simple: it just builds a pair composed of the function name and the function pointer. We use `BOOST_PP_COMMA_IF` to separate each pair with the previous one.

Just note that we use a `__REFLEX_TO_STRING` macro to get the function name as a string instead of directly using the `#` token. In fact, if we directly do `#function`, we will get the following string: `"(val1)"` instead of `"val1"`.

In this case, the macros are quite simple, but it can quickly become a mess... Just get a look of the macros I use in my final implementation [here](https://github.com/Cylix/Reflex/blob/master/includes/reflex/reflectable/macros.hpp).

And here is the final result:

{% highlight c++ %}
REGISTER_CLASS(SomeClass, (fct_1)(fct_2)(fct_3))
{% endhighlight %}

Better!

### What's next

This article is quite long and complex, so I've not covered everything: implementation for c-style functions, namespaced types, reflection on existing object, const member functions, ... are not explained here.
They will maybe be part of another article, but will not be integrated into this one.
Moreover, code is simplified and cut at certain points to make the article more accessible.

If you are interested to go deeper in my implementation, you may be interested in reading the source code on the github repository [here](https://github.com/Cylix/Reflex).
I'll try to update the library as soon as I can, but I'm a little bit busy for the moment.

There also may be some little errors in the examples: that's is due to the fact I've extracted and modified some part of my implementation to explain more easily my implementation. If you see some errors, I'll be thankful if you report them so that I can improve my article!

Thanks for reading and I hope this article was interesting and clear enough so that everyone could understand it! Please give me a feedback in the comments, I'd be happy to read it.
