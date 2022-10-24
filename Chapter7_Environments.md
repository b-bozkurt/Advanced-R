---
Title: "Chapter 7"
Author: "Batuhan T. Bozkurt"
Date: "2022-10-23"
---

# Quiz
1.  List at least three ways that an environment differs from a list.
    
2.  What is the parent of the global environment? What is the only environment that doesn’t have a parent?
    
3.  What is the enclosing environment of a function? Why is it important?
    Functions has their own environment "bubbles". 
4.  How do you determine the environment from which a function was called?
    
5.  How are `<-` and `<<-` different?
# Exercises
## 7.2.7
1. List three ways in which an environment differs from a list.
	1.  Every name must be unique.
	2. The names in an environment are not ordered.
	3. An environment has a parent.
	4. Environments are not copied when modified.
2. I defined an environment called loop, and set it to itsef:
   ```r
> loop<-env()
> loop$loop <- loop
> env_print(loop)
<environment: 0x000001d16b1fc458>
Parent: <environment: 0x000001d16bb663e0>
Bindings:
* loop: <env>
```
3. Similar to previous:
```r
> dedoop <- env()
> loop$dedoop <- dedoop
> dedoop$loop <- loop
> env_print(loop)
<environment: 0x000001d16b1fc458>
Parent: <environment: 0x000001d16bb663e0>
Bindings:
* loop: <env>
* dedoop: <env>
> env_print(dedoop)
<environment: 0x000001d16ea53078>
Parent: <environment: global>
Bindings:
* loop: <env>
```
4. Elements within the environment are not ordered, so it would make no sense to call an indexed number. Not sure about the second one.
5. I've simply added a check that will break out if the name is present in the environment.
   ```r
    env_poke_b <- function (env = caller_env(), nm, value, inherit = FALSE, create = !inherit) 
{
  if (env_has(env,nm))
  {
    break
    print("Name duplicate")
  }
  check_environment(env)
  invisible(.Call(ffi_env_poke, env = env, nm = nm, values = value, 
                  inherit = inherit, create = create))
}
```
6. Looks like rebindstops if it can't find the name assigned to it. <<- will create a new variable in the global environment in that case.

## 7.3.1
1. 1. 1.  Modify `where()` to return _all_ environments that contain a binding for `name`. Carefully think through what type of object the function will need to return.
```r
where_b <- function(name, env = caller_env(), out <- list()) {
  if (identical(env, empty_env())) {
    out
  } else {
    # Recursive case
    out<- append(out, env)
    where_b(name, env_parent(env))
  }
}
2. 
   ```r
   fget <- function (name, env = caller_env(),inherits=FALSE)
{
  if (inherits==FALSE)
  {
    if (env_has(env, name))
      get <- env_get(env, name)
      if(is.function(get))
      { 
        return(get)
      }
      else
      { 
        print("Can't find function")
      }
    
  }
  else if(inherits== TRUE)
  {
    fget(name, env_parent(env))
  }
}
```

# Notes
## 7.2 Environment basics
1. Properties of environments
	1. Every name must be unique.
	2. The names in an environment are not ordered.
	3. An environment has a parent.
	4. Environments are not copied when modified.
2. All environments has parents.
	1. There is the `empty` environment, which has no parents.
3. Super assignment : instead of creating a new variable in the existing environment, modifies an existing variable found in the parent environment.
	1. If <<- can't find a variable, will create a new one in the global environment.
