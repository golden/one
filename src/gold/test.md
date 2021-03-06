# Unit Tests

```awk
@include "gold/math"
```

To test a file `x.md`, create a file in
the same directory called `xok.md` that

- Loads the `x.md` file;
- The runs the `tests` function to call a set of test functions...
- ... where each function has a reserved argument `f` ...
- ... and the `ok` tests in each function take two arguments:
  - that `f` variable
  - and a second argument that evaluates to true if test is passed.

        ```awk
        @include "xok"
        
        BEGIN { tests("x", "_test1, test2") }

        function _test1(f,    x) {
           x = 1 + 1
           ok(f,  x == 2)
        }

        function _test2(f,    x) {
           x = 1 - 1 
           ok(f,  ! x )
        }
        ```

## Top-level Test Driver
### tests(s1, s2). For file "s1", run the tests listed in "s2".
Top level unit-test driver.  Resets the random number generator
before each test.  Prints the group and name of the test.
Warns about stray globals at the end.
```awk
function tests(what, all,   f,a,i,n) {
  n = split(all,a,",")
  print "\n#--- " what " -----------------------"
  for(i=1;i<=n;i++) { 
    srand(1)
    f = a[i]; 
    @f(f) 
  }
  rogues()
}
```

### ok(s1, b).  Checks test result "b" in file "s1",
Report the `yes` or `no` message if a test passes or fails.
Increments the global `test.yes` and `test.no` counters.
```awk
function ok(f,yes,    msg) {
  msg = yes ? "PASSED!" : "FAILED!"
  if (yes) 
     GOLD.test.yes++ 
  else
     GOLD.test.no++;
  print "#test:\t" msg "\t" f
}
```

### near(n1,n2, ?n3 = 10^-32) : b. Returns true in "n1" is within "n3" of "n2" 
Return true if what you `got` is within `epsilon` of
what you `want` (`epsilon` defaults to 0.001).
```awk
function near(got,want,     epsilon) {
   epsilon = epsilon ? epsilon : 0.001
   return abs(want - got)/(want + 10^-32)  < epsilon
}
```

### within(n1,n2,n3) : n.  Return true if "n2" is in between "n1" and "n3".
```awk
function within(got,lo,hi) { 
   return  got >= lo && got <= hi
}
```

### rogues().  Report variables that have escaped from functions.
```awk
function rogues(    s) {
  for(s in SYMTAB) 
    if (s ~ /^[A-Z][a-z]/) 
      print "#W> Global " s>"/dev/stderr"
  for(s in SYMTAB) 
    if (s ~ /^[_a-z]/    ) 
      print "#W> Rogue: " s>"/dev/stderr"
}
```
