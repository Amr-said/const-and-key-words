####Const####
______________

* Const gives you the ability to document your program more clearly and actually enforce that documentation.
* By enforcing your documentation, the const keyword provides guarantees to your users that allow you to make performance optimizations without the threat of damaging their data.\
* For instance, const references allow you to specify that the data referred to won't be changed; this means that you can use const references as a simple and immediate way of improving performance for any function that currently takes objects by value without having to worry that your function might modify the data\
* Even if it does, the compiler will prevent the code from compiling and alert you to the problem. On the other hand, if you didn't use const references, you'd have no easy way to ensure that your data wasn't modified.\

***The primary purpose of const is to provide documentation and prevent programming mistakes\
***Const allows you to make it clear to yourself and others that something should not be changed. Moreover, it has the added benefit that anything that you declare const will in fact remain const short of the use of forceful methods.\
***It's particularly useful to declare reference parameters to functions as const references:\
**Ex:\
/*int find (const int& obj);  */

          **Here, the int obj is passed by reference into find. For safety's sake, const is used to ensure that find cannot change the obj--after all, it's just supposed to make sure that the obj is in a valid state. This can prevent silly programming mistakes that might otherwise result in damaging the obj (for instance, by setting a field of the class for testing purposes, which might result in the field's never being reset)\
          **Moreover, by declaring the argument const, users of the function can be sure that their object will not be changed and not need to worry about the possible effects of making the function call.\
          
**\
________________________________________________________________________________________________________________________________________________________________________________\
****Const Pointers****

***We've already seen const references demonstrated, and they're pretty natural: when you declare a const reference, you're only making the data referred to const.And references, by their very nature, cannot change what they refer to.\
***Pointers, on the other hand, have two ways that you can use them:\
1-you can change the data pointed to.\
2-or you can change the pointer itself.\
*Consequently, there are two ways of declaring a const pointer: one that prevents you from changing what is pointed to, and one that prevents you from changing the data pointed to.\
Ex:\
/* const int *p  */
*Here *p is a "const int". So the pointer may be changeable, but you definitely can't touch what p points to. The key here is that the const appears before the *.\

            if you just want the address stored in the pointer itself to be const, then you have to put const after the *:\
            /* int x;
               int * const p = &x; */
               
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------\

****Const Functions****

***The effects of declaring a variable to be const propagate throughout the program. Once you have a const object, it cannot be assigned to a non-const reference or use functions that are known to be capable of changing the state of the object.\
***This is necessary to enforce the const of the object, but it means you need a way to state that a function should not make changes to an object. In non-object-oriented code, this is as easy as using const references as demonstrated above.\


*In C++, however, there's the issue of classes with methods. If you have a const object, you don't want to call methods that can change the object, so you need a way of letting the compiler know which methods can be safely called\
**These methods are called "const functions", and are the only functions that can be called on a const object. Note, by the way, that only member methods make sense as const methods.\
***The way to declare that a function is safe for const objects is simply to mark it as const; the syntax for const functions is a little bit peculiar because there's only one place where you can really put the const: at the end of the function:\
Ex:\
/*const int circularArea(int r){
return pi*pi*r;
}*/
***Note that just because a function is declared const that doesn't prohibit non-const functions from using it; the rule is this:\
1-Const functions can always be called.\
2-Non-const functions can only be called by non-const objects.\

***const functions have a slightly stronger restriction than merely that they cannot modify the data.\
**They must make it so that they cannot be used in a way that would allow you to use them to modify const data.\
*This means that when const functions return references or pointers to members of the class, they must also be const.\
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------\
****Const Overloading****
*In large part because const functions cannot return non-const references to an objects' data, there are many times where it might seem appropriate to have both const and non-const versions of a function.\

**For instance, if you are returning a reference to some member data (usually not a good thing to do, but there are exceptions), then you may want to have a non-const version of the function that returns a non-const reference:\
Ex:\
               /*int& myClass::getData(){ \
                     return data;          \
                 }                          \
               */                            \
***On the other hand, you don't want to prevent someone using a const version of your object,\
Ex:\
/*myClass constDataHolder;*/ \
***from getting the data. You just want to prevent that person from changing it by returning a const reference.\
**But you probably don't want the name of the function to change just because you change whether the object is const or not--among other things, this would mean an awful lot of code might have to change just because you change how you declare a variable--going from a non-const to a const version of a variable would be a real headache.\


*Fortunately, C++ allows you to overload based on the const-ness of a method. So you can have both const and non-const methods, and the correct version will be chosen.\
**If you wish to return a non-const reference in some cases, you merely need to declare a second, const version of the method that returns a const method:\
EX:\
                      /*const int& myData::getData() const {\
                                    return data;\
                                  }\
                     */

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------\
****Const iterators****

*As we've already seen, in order to enforce const-ness, C++ requires that const functions return only const pointers and references.\
**Since iterators can also be used to modify the underlying collection, when an STL collection is declared const, then any iterators used over the collection must be const iterators.\
***They're just like normal iterators, except that they cannot be used to modify the underlying data. (Since iterators are a generalization of the idea of pointers, this makes sense.).\

*Const iterators in the STL are simple enough: just append "const_" to the type of iterator you desire. For instance, we could iterator over a vector as follows:\
Ex:\
/*std::vector<int> vec;\
vec.push_back( 3 );\
vec.push_back( 4 );\
vec.push_back( 8 );\
for ( std::vector<int>::const_iterator itr = vec.begin(), end = vec.end();\
itr != end;\
++itr )\
{\
// just print out the values...\
std::cout<< *itr <<std::endl;\
}\
*/
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------\
****Const cast****

***Sometimes, you have a const variable and you really want to pass it into a function that you are certain won't modify it.\
**But that function doesn't declare its argument as const. (This might happen, for instance, if a C library function like strlen were declared without using const.)\
*Fortunately, if you know that you are safe in passing a const variable into a function that doesn't explicitly indicate that it will not change the data, then you can use a const_cast in order to temporarily strip away the const-ness of the object.\
Ex:\

              // a bad version of strlen that doesn't declare its argument const
                 int bad_strlen (char *x)
                            {
                                  strlen( x );
                             }

              // note that the extra const is actually implicit in this declaration since
              // string literals are constant
                     const char *x = "abc";

              // cast away const-ness for our strlen function
                   bad_strlen( const_cast<char *>(x) );

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------\
****Dangers of Too-much Const****

*Beware of exploiting const too much; for instance, just because you can return a const reference doesn't mean that you should return a const reference.\
**The most important example is that if you have local data in a function, you really ought not return a reference to it at all (unless it is static) since it will be a reference to memory that is no longer valid.\

***Another time when returning a const reference may not a good idea is when you are returning a reference to member data of an object.\
**Although returning a const reference prevents anyone from changing the data by using it, it means that you have to have persistent data to back the reference--it has to actually be a field of the object and not temporary data created in the function.\
*Once you make the reference part of the interface to the class, then, you fix the implementation details. This can be frustrating if you later wish to change your class's private data so the result of the function is computed when the function is invoked rather than actually be stored in the class at all times.\
=================================================================================================================================================================================\

####'&' Symbol####
------------------

***The & symbol is used as an operator in C++. It is used in 2 different places, one as a bitwise and operator and one as a pointer address of operator.\
-----------------------------------------------------------------------------------------------------
****Bitwise AND****

***The bitwise AND operator (&) compares each bit of the first operand to that bit of the second operand.\
**If both bits are 1, the bit is set to 1. Otherwise, the bit is set to 0. Both operands to the bitwise AND operator must be of integral types.\

Ex:\
/*  int a=0;
int b=1000;
cout<<(a&b)<<endl;
*/
And the output is 0 \
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------\
****Address Of operator****

*** C++ provides two-pointer operators, which are Address of Operator (&) and Indirection Operator (*).\
**A pointer is a variable that contains the address of another variable or you can say that a variable that contains the address of another variable is said to "point to" the other variable. A variable can be any data type including an object, structure or again pointer itself.\
*The address of Operator (&), and it is the complement of *. It is a unary operator that returns the address of the variable(r-value) specified by its operand. For example,\

       /*
       int  var;
       int  *ptr;
       int  val;

       var = 3000;

       // take the address of var
       ptr = &var;

       // take the value available at ptr
       val = *ptr;
       cout << "Value of var :" << var << endl;
       cout << "Value of ptr :" << ptr << endl;
       cout << "Value of val :" << val << endl;
       */
And the output:\
Value of var :3000\
Value of ptr :0xbff64494\
Value of val :3000

