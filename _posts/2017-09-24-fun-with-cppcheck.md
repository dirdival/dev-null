---
title: "Fun with cppcheck"
---

Do you use static analysis tools? Most of developers don't care about
them ,but it's a huge mistake. Especially young developers after studies think
that they are _omniscient_ and they don't make mistakes. 

When I started my journey with programming long long time ago I and my
buddies used only small part of programming tools, of course compiler
was there and debugger. After some time when speed of CPU and size
of memory started growing appears new better tools. Modern IDE shows
you mistakes before you call compilation, gives you some clues according
to the code. Competition between `gcc` and `clang` gives us better compilers.
Right now almost each day I use `valgrind`, `scan-build`, `cppcheck`, `jenkins`.
Since five years I and my team are writing automatic tools which extract cores,
compare bactracks and report info about issue into bug tracking system. Self-regulating
environment which build new software, install it on machines, execute tests and
generate reports about new issues and regressions.

Small part of of that process is [cppcheck](http://cppcheck.sourceforge.net/)
called during compilation. It's
a static analysis tool but what doesn't mean? Most of developers think that
compiler prevents them against errors. It's only half-true. Of course, each
compiler has many flags which might help you to improve your code, but
at least two problems are here. First you have to know about this flags and
second - you should enable them - I think that it might be a _good story_ for
a some future post. But, even you enable all of them it's not enough. There
are some kind of issues -- logical mistakes -- which are not check by compilers.
And here we have static analysis tools like `cppcheck`. It contains `base` of
most popular mistakes made by developers.

OK, let's play game - _code review_. I will give you a code snippet and try
find in it a mistake. Under each example you will find output from `cppcheck` and my
comment. Most of samples are small and you might think that they are dummy but
they only might give you a clue, what kind of check you might have.

_Are you ready for more action?_

---

```cpp
char *foo()
{
    char str[100] = {0};
    return str;
}
```

```
[test.cpp:4]: (error) Pointer to local array variable returned.
```

> We start from easy mistake. It's common made by developers which where moved from _Java_ or _C#_ into _C/C++_
> It might be catch easily by compiler but did you enable that check? And second, more important question: did you enabled
> `-Werror`?

```
dirdival@pld:/tmp$ gcc -Wall -o a a.c 
a.c: In function ‘foo’:
a.c:11:5: warning: function returns address of local variable [-Wreturn-local-addr]
     return str;
     ^
```
```
dirdival@pld:/tmp$ clang -Wall -o a a.c 
a.c:11:5: warning: address of stack memory associated with local variable 'str' returned [-Wreturn-stack-address]
    return str;
```

BTW. How you can see `gcc` and `clang` have a different flag to prevent it.

---

```cpp
void f(char *p) {
    if (x)
        free(p);
    else
        p = 0;
    free(p);
}
```
```
[test.c:6]: (error) Memory pointed to by 'p' is freed twice.
```
> Double free. I have seen so many times... Is hard to find it when code has many _if_ statements, loops and
> function takes whole screen.

---

```cpp
void foo()
{
    list<int> l1;
    list<int> l2;
    list<int>::iterator it = l1.begin();
    while (it != l2.end())
    {
        ++it;
    }
}
```
```
[test.cpp:6]: (error) Same iterator is used with different containers 'l1' and 'l2'.
```
> This one is hard to find. Believe me. In most cases it's _copy/paste_ issue. If line is long and
> you have few similar code snippets one by one you can miss it.

---

```cpp
void foo(std::vector<int> &r, int c)
{
    std::vector<int>::iterator aI = r.begin();
    while(aI != r.end())
    {
        if (*aI == 0)
        {
            r.insert(aI, 42);
            if (c)
            {
                return;
            }
        }
        ++aI;
    }
}
```
```
[test.cpp:14] (error) After insert(), the iterator 'aI' may be invalid.
```
> It shows that author doesn't know much about using containers.

---

```cpp
void f()
{
    std::vector<int> foo;
    std::vector<int>::iterator it;
    for (it = foo.begin(); it != foo.end(); ++it)
    {
        if (a) {
            if (b)
                foo.erase(it);
            else
                *it = 0;
        }
    }
}
```

```
[test.cpp:5] -> [test.cpp:9]: (error) Iterator 'it' used after element has been erased.
```

> What to say more? Modifying any collection during iteration on it *always* is not the best idea.

---

```cpp
class A 
{ 
    A& operator=(const A& other) { return *this; }
    int x;
};
```

```
[test.cpp:1]: (warning) The class 'A' has 'operator=' but lack of 'copy constructor'.
```

> Small _lesson_ about _C++_ by `cppcheck`.

---

```cpp
class F
{
   public:
   char *c,*p,*d;
   F(const F &f) : p(f.p), c(f.c)
   {
      p=(char *)malloc(strlen(f.p)+1);
      strcpy(p,f.p);
   }
   F(char *str)
   {
      p=(char *)malloc(strlen(str)+1);
      strcpy(p,str);
   }
}
```

```
[test.cpp:5]: (style) Value of pointer 'p', which points to allocated memory, is copied in copy constructor instead of allocating new memory.\n"
```

> _Copy/paste_ issue. So obvious when you look on it but during coding you can lose a focus and we have something like that.

---

```cpp
template <typename T> class A
{
    public:
        virtual ~A(){}
};
template <typename T> class AA
{
    public:
        ~AA(){}
};
class B : public A<int>, public AA<double>
{
    public:
        ~B(){int a;}
};

AA<double> *p = new B; delete p;
```

```
[test.cpp:9]: (error) Class 'AA < double >' which is inherited by class 'B' does not have a virtual destructor.
```

> Yet another common mistake. To be honest, in my opinion it's _C++_ language issue. It has few stupid things, notations and
> wrong decisions made by authors. One of them you can see above.

---

```cpp
void f(int x) {
    if (1 != x || 3 != x)
        a++;
}
```

```
[test.cpp:2]: (warning) Logical disjunction always evaluates to true: x != 1 || x != 3.
```

> _Shit happens_. Tired man may make stupid things. Here I can give you a small clue -- always watch out
> on negation.

---

```cpp
int a;
int foo() {
    a = 1+2;
    return a;
}
assert(foo() == 3); 
```

```
[test.cpp:6]: (warning) Assert statement calls a function which may have desired side effects: 'foo'.
```

> In my current work we set `NODEBUG` macro to disable asserts on production. And always when
> I see similar code I wish rip someone's head off.

---

```cpp
int f()
{
    time_t t = 0;
    localtime(&t);
}
```

```
[test.cpp:3]: (portability) Non reentrant function 'localtime' called.
For threadsafe applications it is recommended to use the reentrant replacement function 'localtime_r'.
```
> Hard one. Most of young developers not even heard about reentrant functions.

---

```cpp
void f()
{
    int val[50];
    int i, sum=0;
    for (i = 0; i < 100; i++)
        sum += val[i];
}
```

```
[test.cpp:6]: (error) Array 'val[50]' accessed at index 99, which is out of bounds.
```
> Here is obvious and looks funny, but it most cases _out of bounds_ issue is related
> to first index after array.

---
```cpp
void f(bool x) {
    if (x < 10) {
        printf("foo");
    }
}
```
```
[test.cpp:2]: (warning) Comparison of a boolean expression with an integer other than 0 or 1.
```
> You may think: how something like this was created? The answer is simple: refactorization. It was
> some type, in most cases _int_ and somebody decided to reduce it to _bool_.

---

```cpp
class Base;
void foo()
{
    Base * b = (Base *) derived;
}
```
```
[test.cpp:4]: (style) C-style pointer casting
```
> Marked as _style_ but it's good idea in _C++_ use static_cast. If you don't know _why_
> just find (write regular expression) to catch all of them. 

---

Above issues comes from `cppcheck` tests. The time has come for real life. Examples under come from my work.
To be clear -- it's not my code -- but I had to fix them all. I changed them a little, you know -- licence.

```cpp
177 out:
178     free(foo);
179     free(boo);
180     free(data);
181     free(more);
182     free(things);
183 
184     if (error) {
185         goto out;
186         logger(error);
187         free(error);
188     }
189 }
```

```
[a.c:186]: (style) Statements following return, break, continue, goto or throw will never be executed.
```

> Brain fart. And we have evidence that author didn't make tests.

---

```cpp
168     if (res) {
169         size_t len = SerializeData(data, &s);
170         if (s) {
171             if (len <= 0 || len > MAX_DATA_SIZE) {
172                 logger("%s: data exceeds maximum size (%d)", __func__, MAX_DATA_SIZE);
173                 free(s);
...
```
```
[a.c:171]: (style) Checking if unsigned variable 'len' is less than zero.
```

> Always check type which is returned by function.

---

```cpp
 658         free(foo->boo);
 659         foo->boo = NULL;
 660         foo->boo = BooCreate(123, NULL);
```
```
[a.c:659]: (style) Variable 'foo->boo' is reassigned a value before the old one has been used.
```
> You might think that is not mistake and you are right. It's a style issue, but to be honest it's dangerous.
> If you change value of variable, and there is no _if_ statement, no call of function -- then it's _suspected_.
> Sometimes it's a _copy/paste_ issue, but sometimes it's _merge_ -- and then it might be a problem.
> If automatic merge switches lines `659` and `660` we've got trouble.


```cpp
Foo
FooCreate(Boo boo,
          Error * errorOut)
{
    Foo self = NULL;
    Error error = NULL;

    if (!self || !boo) {
        error = ErrorCreate("NULL argument passed (self=%p, boo=%p)", self, boo);
        goto fini;
    } else if (!IsGoodBoo(boo) && !IsBetterBoo(boo)) {
        error = ErrorCreate("Invalid Boo passed");
        goto fini;
    }
```

```
[a.c:67]: (style) Condition '!self' is always true
```
> Yet another _Copy/Paste_ example. Error checking was applied a little to fast.

---

```cpp
272     if (kind == SomeKind_Foo) {
273         if (self->policy == SomeFunPolicyHere_Boo) {
274             MakeSomethingsHere(self, 0); 
275             return;
276         } else if (self->policy == SomeFunPolicyHere_Boo) {
277             MakeSomethingHere(self, self->len - 1);
278             return;
279         } else {
```

```
[a.c:276]: (style) Expression is always false because 'else if' condition matches previous condition at line 273.
```

> This check is my favorite. I made hundreds of code review and believe me -- this kind of issue is easy to omit.
> Just imagine longer block of _if/else_ and the same _rf statement_ separated by many others. You may think that
> is rare problem but it happens more often than you think. Many developers work on the same code, they make
> _merges_ and on the end we have something like above. I made some _tests_ with this check: I switched variables,
I wrote the same expression in other way and it still works. Nice to have that check.

---

```cpp
106     if (!self | !someData) {
107         error = ErrorCreate("NULL argument passed (self=%p, someData=%p)", self, someData);
108         goto fini;
```

```
[a.c:106]: (style) Boolean result is used in bitwise operation. Clarify expression with parentheses.
```

> This one is _fun_, example of _not Copy/Paste_.  If you are not careful you might miss it. 

---

```
139     } else if (outputID <= sizeof(someOutput) / sizeof(someOutput[0])) {
140         if (someOutput[outputID].mode != Mode_none)
```

```
[a.c:140]: (warning) Either the condition 'outputID<=sizeof(someOutput)/sizeof(someOutput[0])' is redundant or the array 'someOutput[16]' is accessed at index 16, which is out of bounds.
```

> _someOutput[16]_ is an array defined many lines above. Part of code: _sizeof(array)/sizeof(array[0])_ is a common way to get length of array. It
> works when you change size of it in definition, even when you change type. But here problem is with operator _<=_ which _allows_ in this example touch memory
> just after the table.

---

I've learnt a lot during examining issues found by static analysis tools.
If you are not using any of them you should start. I recommend you `cppcheck`. Just call it and check report.
You might be surprised how many issues are present in your code. First full clean up might be a nightmare.
Source code which had around `500.000` lines I cleaned up a month.
But when everything is done the time has come to punish developers. Just enable new check and see how they cry.
The funniest part is during upgrade software to the newest version.
You have to fix all new issues found by tool but then you have a free new lesson about common programming mistakes.
