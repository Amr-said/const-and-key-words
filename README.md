# const-and-key-words
####Const####
______________

* Const gives you the ability to document your program more clearly and actually enforce that documentation.\
* By enforcing your documentation, the const keyword provides guarantees to your users that allow you to make performance optimizations without the threat of damaging their data.\
* For instance, const references allow you to specify that the data referred to won't be changed; this means that you can use const references as a simple and immediate way of improving performance for any function that currently takes objects by value without having to worry that your function might modify the data\
* Even if it does, the compiler will prevent the code from compiling and alert you to the problem. On the other hand, if you didn't use const references, you'd have no easy way to ensure that your data wasn't modified.\

***The primary purpose of const is to provide documentation and prevent programming mistakes\
***Const allows you to make it clear to yourself and others that something should not be changed. Moreover, it has the added benefit that anything that you declare const will in fact remain const short of the use of forceful methods.\
***It's particularly useful to declare reference parameters to functions as const references:\
     **Ex:\
          /*int find (const int& obj);  */

          **Here, the int obj is passed by reference into find. For safety's sake, const is used to ensure that find cannot change the obj--after all, it's just supposed to make               sure that the obj is in a valid state. This can prevent silly programming mistakes that might otherwise result in damaging the obj (for instance, by setting a field of             the class for testing purposes, which might result in the field's never being reset)\
          **Moreover, by declaring the argument const, users of the function can be sure that their object will not be changed and not need to worry about the possible side                   effects of making the function call.\
___________________________________________________________________________________________________________________________________________________________________________________
