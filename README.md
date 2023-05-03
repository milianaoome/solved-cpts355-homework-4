Download Link: https://assignmentchef.com/product/solved-cpts355-homework-4
<br>
<ul>

 <li>integer constants, e.g. 1, 2, 3, -4, -5</li>

 <li>boolean constants, e.g. true, false</li>

 <li>array constants, e.g. [1 2 3 4], [-1 2 3 -4], [1 x 3 4 add 2 sub],</li>

</ul>

[1 2 x 4]  where x is a variable. For simplicity we will assume that SPS arrays are not nested (can’t have subarrays).

<ul>

 <li>name constants, e.g. /fact: start with a / and letter followed by an arbitrary sequence of letters and numbers</li>

 <li>names to be looked up in the dictionary stack, e.g. fact: as for name constants, without the /</li>

 <li>code constants: code between matched curly braces { … }</li>

 <li>built-in operators on numbers: add, sub, mul, eq, lt, gt</li>

 <li>built-in operators on boolean values: and, or, not (we will call these psAnd, psOr, and psNot)</li>

 <li>built-in operators on array values: length, get, getinterval, put, putinterval, forall. See the lecture notes for more information on array functions (you will implement forall operator in Part2).</li>

 <li>built-in conditional operators: if,  ifelse (you will implement if/ifelse operators in Part2)</li>

 <li>built-in loop operator: repeat (you will implement repeat operator in Part 2).</li>

 <li>stack operators: dup, copy, count, pop, clear, exch, mark, cleartomark, counttomark</li>

 <li>dictionary creation operator: dict; <u>takes one operand from the operand stack, ignores it, and</u> <u>creates a new, empty dictionary on the operand stack </u>(we will call this psDict) &#x25aa; dictionary stack manipulation operators: begin, end.</li>

</ul>

− begin requires one dictionary operand on the operand stack; end has no operands.

<ul>

 <li>name definition operator: def.</li>

 <li>defining (using def; we will call this psDef) and calling functions</li>

 <li>stack printing operator (prints contents of stack without changing it): stack</li>

</ul>

<h1>Part 1 – Requirements</h1>

In Part 1 you will build some essential pieces of the interpreter but not yet the full interpreter. The pieces you build will be driven by Python test code rather than actual Postscript programs. The pieces you are going to build first are:

<ol>

 <li>The operand stack</li>

 <li>The dictionary stack</li>

 <li>Defining variables (with <strong>def</strong>) and looking up names</li>

 <li>The operators that don’t involve code arrays: all of the operators <strong>except </strong><strong>repeat loop operator, </strong><strong>if/ifelse operators, </strong><strong>forall operator, and calling functions</strong> (You will complete these in Part 2)</li>

</ol>

<h1>1. The Operand Stack – opstack</h1>

The operand stack should be implemented as a Python list. The list will contain Python integers, strings, and later in Part 2 code arrays. Python integers and lists on the stack represent Postscript integer constants and array constants. Python strings which start with a slash / on the stack represent names of Postscript variables. When using a list as a stack, assume that the top of the stack is the end of the list (i.e., the pushing and popping happens at the end of the list).

<h1>2. The Dictionary Stack – dictstack</h1>

The dictionary stack is also implemented as a Python list. It will contain Python dictionaries which will be the implementation for Postscript dictionaries. The dictionary stack needs to support adding and removing dictionaries at the top of the stack (i.e., end of the list), as well as defining and looking up names.

<h2>3. define and lookup</h2>

You will write two helper functions, define and lookup, to define a variables and to lookup the value of a variable, respectively.

The define function adds the “name:value” pair to the top dictionary in the dictionary stack. Your psDef function ( i.e., your implementation of the Postscript def operator) should pop the name and value from operand stack and call the “define” function.

You should keep the ‘/’ in the name constant when you store it in the dictStack.

<strong>def </strong>define(name, value):

<strong>pass </strong>

<strong>    </strong><em>#add name:value pair to the top dictionary in the dictionary stack</em><em>.  </em>

The lookup function should look-up the value of a given variable in the dictionary stack. In Part 2, when you interpret simple Postscript expressions, you will call this function for variable lookups and function calls.

<em> </em><strong>def </strong>lookup(name):

<strong>pass </strong>

<strong>    </strong><em># return the value associated with name </em>

<em>    # What is your design decision about what to do when there is no definition for “name”? If “name” is not defined, your program should not break, but should give an appropriate error message.</em>

<strong> </strong>

<h1>4. Array constants</h1>

In our SPS interpreter we will represent array constants as Python lists. Remember that, the operators and variables in arrays  will be evaluated before the array constant is pushed onto the stack.  For example,  the SPS array [1 2 3 true 5] will be represented as the Python list  [1,2,3,True,5] when pushed onto the opstack. Additional examples:

<ul>

 <li>SPS array [1 2 3 add 5] will be represented as Python list [1,5,5]</li>

 <li>SPS array [1 true false and not 5] will be represented as Python list [1,true,5]</li>

 <li>SPS array [1 x y 5] will be represented as Python list [1,2,3,5] where x’s value is 2 and y’s value is 3.</li>

</ul>




<strong>Important note: </strong>In part-1, we will assume that array constants are already evaluated and include only integers and boolean values when they are pushed onto the stack. In part-2, when we interpret SPS code, we will evaluate the constant arrays before we push them onto the stack.

<strong>             </strong>

<h1>5. Operators</h1>

Operators will be implemented as <strong>zero-argument Python functions</strong> that manipulate the operand and dictionary stacks. For example, the add operator could be implemented as follows.

#pop 2 values from stack; check if they are numerical (int type); add them; push the result back to stack.  def add():     if len(opstack) &gt; 1:         op2 = opPop()         op1 = opPop()         if(isinstance(op1,int) and isinstance(op2,int)):             opPush(op1+op2)         else:             print(“Error: add – one of the operands is not a numerical value”)             opPush(op1)             opPush(op2)                  else:

print(“Error: add expects 2 operands”)




<ul>

 <li>The begin and end operators are a little different in that they manipulate the dictionary stack in addition to or instead of the operand stack. Remember that the dict operator (i.e., psDict function)affects only the operand stack.</li>

</ul>

(<strong>Note about </strong><strong>dict</strong>: Remember that the dict operator takes an integer operand from the operand stack and pushes an empty dictionary to the operand stack (affects only the operand stack). The initial size argument is ignored – Postscript requires it for backward compatibility of dict operator with the early Postscript versions).

<ul>

 <li>The def operator (i.e., psDef function) takes two operands from the operand stack: a string (recall that strings that start with “/” in the operand stack represent names of postscript variables) and a value. It changes the dictionary at the top of the dictionary stack so that the string is mapped to the value by that dictionary. Notice that def does not change the number of dictionaries on the dictionary stack!</li>

</ul>

<strong>You may start your implementation using the below skeleton code </strong>(given in

HW4_part1_skeleton.py). <strong>Please make sure to use the function names given below.  </strong>




#————————- 10% ————————————- # The operand stack: define the operand stack and its operations opstack = []  #assuming top of the stack is the end of the list




# Now define the HELPER FUNCTIONS to push and pop values on the opstack

# Remember that there is a Postscript operator called “pop” so we choose

# different names for these functions.

# Recall that `pass` in python is a no-op: replace it with your code.

def opPop():     pass

# opPop should return the popped value.

# The pop() function should call opPop to pop the top value from the opstack, but it will ignore the popped value.

def opPush(value):

pass




#————————– 20% ————————————- # The dictionary stack: define the dictionary stack and its operations dictstack = []  #assuming top of the stack is the end of the list




# now define functions to push and pop dictionaries on the dictstack, to  # define name, and to lookup a name

def dictPop():

pass

# dictPop pops the top dictionary from the dictionary stack.

def dictPush(d):     pass

#dictPush pushes the dictionary ‘d’ to the dictstack.

#Note that, your interpreter will call dictPush only when Postscript

#“begin” operator is called. “begin” should pop the empty dictionary from      #the opstack and push it onto the dictstack by calling dictPush.

def define(name, value):

pass

#add name:value pair to the top dictionary in the dictionary stack.

#Keep the ‘/’ in the name constant.

#Your psDef function should pop the name and value from operand stack and      #call the “define” function.

def lookup(name):     pass

# return the value associated with name

# What is your design decision about what to do when there is no definition for “name”? If “name” is not defined, your program should not break, but should give an appropriate error message.




#————————— 10% ————————————-

# Arithmetic, comparison, and boolean operators: add, sub, mul, eq, lt, gt, and, or, not

# Make sure to check the operand stack has the correct number of parameters

# and types of the parameters are correct. def add():     pass  def sub():     pass  def mul():     pass  def eq():     pass  def lt():     pass  def gt():     pass  def psAnd():     pass  def psOr():     pass




def psNot():     pass




#————————— 25% ————————————- # Array operators: define the string operators length, get, getinterval, put, putinterval def length():

pass  def get():     pass  def getinterval():     pass  def put():     pass  def putinterval():

pass




#————————— 15% ————————————-

# Define the stack manipulation and print operators: dup, copy, count, pop, clear, exch, mark, cleartomark, counttotmark def dup():     pass  def copy():     pass  def count():     pass  def pop():     pass  def clear():     pass  def exch():     pass  def mark():     pass  def cleartomark():     pass  def counttomark():     pass  def stack():     pass




#————————— 20% ————————————-

# Define the dictionary manipulation operators: psDict, begin, end, psDef

# name the function for the def operator psDef because def is reserved in Python.

Similarly, call the function for dict operator as psDict.

# Note: The psDef operator will pop the value and name from the opstack and call your own “define” operator (pass those values as parameters). # Note that psDef()won’t have any parameters.

def psDict():

pass  def begin():     pass  def end():     pass  def psDef():     pass




<strong>Important Note:</strong> For all operators you need to implement basic checks, i.e., check whether there are sufficient number of values in the operand stack and check whether those values have correct types.

For example,

<ul>

 <li>def operator: the operand stack should have 2 values where the second value from top of the stack is a string starting with ‘/’</li>

 <li>get operator : the operand stack should have 2 values; the top value on the stack should be an integer and the second value should be an array constant.</li>

</ul>

Also, see the add implementation on page 3.

You will be deducted points if you don’t do error checking. <strong> </strong>

<h1>4. Testing Your Code</h1>

We will be using the unittest Python testing framework in this assignment.  See  <u>https://docs.python.org/3/library/unittest.html</u> for additional documentation.

The file HW3Sampletests_part1.py provides sample test cases for the SPS operators.  This file imports the HW4_part1 module (HW4_part1.py file) which will include your implementations of the SPS operators.

For example:

def test_lookup(self):

opPush(‘/n1’)         opPush(3)         psDef()

self.assertEqual(lookup(‘n1’),3)

def test_add(self):

opPush(1)         opPush(2)         add()

self.assertEqual(opPop(),3)




<strong>You should provide one additional test method for each of the following operators.  </strong>

<h2>get, getinterval, put, putinterval, def, begin, end, cleartomark, counttomark</h2>

Your tests should be different than the provided tests.  Failing to test your operators thoroughly may result in bugs in your operator implementations. And those may cause issues in your part2 implementation as well. So, please make sure to unit test all your operator implementations carefully.

In Python unittest framework, each test function has a “test_” prefix. To run all tests, execute the following command on the command line.

python -m unittest HW4Sampletests_part1

You can run tests with more detail (higher verbosity) by passing in the -v flag:

python -m unittest -v HW4Sampletests_part1

<strong> </strong>

<h1>Main Program</h1>

In this assignment, we simply write some unit tests to verify and validate the functions. If you would like to execute the code, you need to write the code for the “main” program. Unlike in C or Java, this is not done by writing a function with a special name. Instead the following idiom is used. This code is to be written at the left margin of your input file (or at the same level as the def lines if you’ve indented those.

<strong> if</strong> __name__ == ‘__main__’:

…code to do whatever you want done…

<strong>PART 2</strong>

(can’t have subarrays). (If the array includes variables or operands, you need to first evaluate its elements before  you push it onto the stack. This will be done in <strong>part2.</strong>)

<ul>

 <li>name constants, e.g. /fact: start with a / and letter followed by an arbitrary sequence of letters and numbers</li>

 <li>names to be looked up in the dictionary stack, e.g. fact: as for name constants, without the /</li>

 <li>code constants: code between matched curly braces { … }</li>

 <li>built-in operators on numbers: add, sub, mul, eq, lt, gt</li>

 <li>built-in operators on boolean values: and, or, not (we will call these psAnd, psOr, and psNot)</li>

 <li><sup>built-in operators on array values: </sup>length, get, getinterval, put, putinterval, forall. See the lecture notes for more information on array functions</li>

</ul>

(you will implement forall operator in <strong>Part 2</strong>).

<ul>

 <li>built-in conditional operators: if,  ifelse (you will implement if/ifelse operators in</li>

</ul>

<strong>Part2</strong>) §             built-in loop operator: repeat  (you will implement repeat operator in <strong>Part 2</strong>).

<ul>

 <li>stack operators: dup, copy, count, pop, clear, exch, mark, cleartomark, counttomark</li>

 <li>dictionary creation operator: dict; <u>takes one operand from the operand stack, ignores it, and</u> <u>creates a new, empty dictionary on the operand stack </u>(we will call this psDict) dictionary stack manipulation operators: begin, end. § begin requires one dictionary operand on the operand stack; end has no operands.</li>

 <li>name definition operator: def.</li>

 <li>defining (using def; we will call this psDef) and calling functions</li>

 <li>stack printing operator (prints contents of stack without changing it): stack</li>

</ul>

<h1>Part 2 – Requirements</h1>

In Part 2 you will continue building the interpreter, making use of everything you built in Part 1. The pieces needed to complete the interpreter are:

<ol>

 <li>Parsing “Simple Postscript” code</li>

 <li>Handling of code-arrays</li>

 <li>Handling the <strong>if </strong>and <strong>ifelse</strong> operators (write the Python methods psIf and psIfelse)</li>

 <li>Handling the <strong>repeat </strong>and<strong> forall </strong>operators (write the Python method psRepeat and forall)</li>

 <li>Function calling</li>

 <li>Interpreting input strings (code) in the simple Postscript language.</li>

</ol>

<h2>1. Parsing</h2>

Parsing is the process by which a program is converted to a data structure that can be further processed by an interpreter or compiler. To parse the SPS programs, we will convert the continuous input text to a list of tokens and convert each token to our chosen representation for it. In SPS, the tokens are:

numbers with optional negative sign, multi-character names (with and without a preceding /), array constants enclosed in parenthesis (i.e., [] ) and the curly brace characters (i.e., “}” and “{“).  We’ve already decided about how some of these will be represented: numbers as Python numbers, names as Python strings, booleans as Python booleans, array constants as Python lists, etc. <strong>For code-array, we will represent tokens falling between the braces using a Python dictionary </strong>(a dictionary that includes a list of tokens).

<h2>2-5.  Handling of code-arrays: if/ifelse, repeat, forall operators, and function calling</h2>

Recall that a code-array is pushed on the stack as a single unit when it is read from the input. Once a code-array is on the stack several things can happen:

<ul>

 <li>if it is the top item on the stack when a def is executed (i.e. the code array is the body of a</li>

</ul>

function), it is stored as the value of the name defined by the def.

<ul>

 <li>if it is the body part of an if/ifelse operator, it is <u>recursively interpreted</u> as part of the evaluation of the if/ifelse. For the if operator, the code-array is interpreted only if the “condition” argument for if operator is true. For the ifelse operator, if the “condition” argument is true, first code-array is interpreted, otherwise the second code-array is evaluated.</li>

 <li>if it is the body part of a repeat operator, it is <u>recursively interpreted</u> as part of the evaluation of the repeat loop.</li>

 <li>finally, when a function is called ( when a name is looked up its value is a code-array), the function body (i.e., the code-array) is recursively interpreted . (We will get to interpreting momentarily).</li>

</ul>

<h2>6. Interpreter</h2>

A key insight is that a complete SPS program is essentially a code-array. It doesn’t have curly braces around it, but it is a chunk of code that needs to be interpreted. This suggests how to proceed:

<ul>

 <li>Convert the SPS program (a string of text) into a list of tokens and store it in a dictionary.</li>

 <li>Define a Python function “interpretSPS” that takes one of these dictionaries (code-arrays) as input and processes the tokens.</li>

 <li>Interpret the body of the if/ifelse, repeat, and forall operators recursively.</li>

 <li>When a name lookup produces a code-array as its result, recursively interpret it, thus implementing Postscript function calls.</li>

</ul>




<h1>Implementing Your Postscript Interpreter</h1>

<h2>I.Parsing</h2>

Parsing converts an SPS program in the form a string to a program in the form of a code-array. It will work in two stages:

<ol>

 <li><strong> Convert all the string to a list of tokens. </strong>Given:</li>

</ol>

“/square {dup mul} def  0 [-5 -4 3 -2 1]

{square add} forall 55 eq false and stack”




will be converted to




[‘/square’, ‘{‘, ‘dup’, ‘mul’, ‘}’, ‘def’, ‘0’, ‘[-5 -4 3 -2 1]’,

‘{‘, ‘square’, ‘add’, ‘}’, ‘forall’, ’55’, ‘eq’, ‘false’, ‘and’, ‘stack’]




Use the following code to tokenize your SPS program.







import re def tokenize(s):

return re.findall(“/?[a-zA-Z][a-zA-Z0-9_]*|[[][a-zA-Z-?0-9_s!][a-zA-Z-?09_s!]*[]]|[-]?[0-9]+|[}{]+|%.*|[^ t
]”, s)

<h2>2. Convert the token list to a code-array</h2>

The output of tokenize is a list of tokens. To differentiate code-arrays from array constants, we will store this token list in a dictionary with key ‘codearray’. The nested-code arrays will also be included as a dictionary. We need to convert the above example to:

{‘codearray’: [‘/square’, {‘codearray’: [‘dup’, ‘mul’]}, ‘def’,

0, [-5, -4, 3, -2, 1],

{‘codearray’: [‘square’, ‘add’]}, ‘forall’,

55, ‘eq’, False, ‘and’, ‘stack’]

}

Notice how in addition to grouping tokens between curly braces into code-arrays, we’ve also converted the strings that represent numbers to Python numbers, the strings that represent booleans to Python boolean values, and the strings that represent constant arrays to Python lists.

The main issue in how to convert to a code-array is how to group things that fall in between matching curly braces. There are several ways to do this. One possible way is find the matching opening and closing parenthesis (“{“ and “}”) recursively, and including all tokens between them in a Python list.

Here is some starting code to find the matching parenthesis using an iterator. Here we iterate over the characters of a string (rather than a list of tokens) using a Python iterator and we try to find the matching curly braces. This code assumes that the input string includes opening and closing curly braces only (e.g., “{{}{{}}}”)

<em># The it argument is an iterator. The sequence of return characters should </em>

<em># represent a string of properly nested {} parentheses pairs, from which </em>

<em># the leasing ‘{‘ has been removed. If the parentheses are not properly </em>

<em># nested, returns False. </em><strong>def </strong>groupMatching(it):

res = []     <strong>for </strong>c <strong>in </strong>it:         <strong>if </strong>c == <strong>‘}’</strong>:             <strong>return </strong>res         <strong>else</strong>:

<em># Note how we use a recursive call to group the inner matching </em>

<em>            # parenthesis string and append it as a whole to the list we are </em>

<em>            # constructing. Also note how we have already seen the leading </em>

<em>            # ‘{‘ of this inner group and consumed it from the iterator.             </em>res.append(groupMatching(it))     <strong>return False  </strong>

<em># Function to parse a string of { and } braces. Properly nested parentheses # are arranged into a list of properly nested lists. </em>

<strong>def </strong>group(s):     res = []     it = iter(s)     <strong>for </strong>c <strong>in </strong>it:         <strong>if </strong>c==<strong>‘}’</strong>:  <em>#non matching closing parenthesis; return false             </em><strong>return False         else</strong>:             res.append(groupMatching(it))     <strong>return </strong>res




So, group(<strong>“{{}{{}}}”</strong>) will return  [[[], [[]]]]

Here we use an iterator constructed from a string, but the iter function will equally well create an iterator from a list. Of course, your code has to deal with the tokens between curly braces and include all tokens between 2 matching opening/closing curly braces inside the code-arrays .

To illustrate the above point, consider this modified  groupMatching and group (now called groupMatch and parse) which also handle the tokens before the first curly braces and between matching braces.




<em># The it argument is an iterator. </em>

<em># The tokens between ‘{‘ and ‘}’ is included as a sub code-array (dictionary). If the </em>

<em># parenteses in the input iterator is not properly nested, returns False. </em>def groupMatch(it):

res = []     for c in it:         if c == ‘}’:

return {‘codearray’:res}         elif c=='{‘:

<em># Note how we use a recursive call to group the tokens inside the </em>

<em>            # inner matching parenthesis. </em>

<em>            # Once the recursive call returns the code-array for the inner  </em>

<em>            # parenthesis, it will be appended to the list we are constructing  </em>

<em>            # as a whole. </em>            res.append(groupMatch(it))         else:             res.append(c)     return False




<em> </em>

<em># Function to parse a list of tokens and arrange the tokens between { and } braces  </em>

<em># as code-arrays. </em>

<em># Properly nested parentheses are arranged into a list of properly nested dictionaries. </em>def parse(L):     res = []     it = iter(L)     for c in it:

if c==’}’:  <em>#non matching closing parenthesis; return false since there is  </em>

<em>                    # a syntax error in the Postscript code. </em>

return False         elif c=='{‘:

res.append(groupMatch(it))         else:

res.append(c)     return {‘codearray’:res}

<strong> </strong>

<strong> </strong>

parse([‘b’, ‘c’, ‘{‘, ‘a’, ‘{‘, ‘a’, ‘b’, ‘}’, ‘{‘, ‘{‘, ‘e’, ‘}’, ‘a’, ‘}’, ‘}’])




returns

{‘codearray’: [‘b’, ‘c’, {‘codearray’: [‘a’, {‘codearray’: [‘a’, ‘b’]},

{‘codearray’: [ {‘codearray’: [‘e’]}, ‘a’ ]} ]} ]}




<strong>Your parsing implementation </strong>

Start with the groupMatch and parse functions above (also included in the given skeleton code); <strong>update the </strong><strong>parse code </strong>so that the strings representing numbers/booleans/arrays  are converted to Python integers/booleans/lists.




[‘/square’, ‘{‘, ‘dup’, ‘mul’, ‘}’, ‘def’, ‘0’, ‘[-5 -4 3 -2 1]’,   ‘{‘, ‘square’, ‘add’, ‘}’, ‘forall’, ’55’, ‘eq’, ‘false’, ‘and’, ‘stack’] should return:

{‘codearray’: [‘/square’, {‘codearray’: [‘dup’, ‘mul’]}, ‘def’,

0, [-5, -4, 3, -2, 1],

{‘codearray’: [‘square’, ‘add’]}, ‘forall’,

55, ‘eq’, False, ‘and’, ‘stack’] }

<h2>II.Interpret code-arrays</h2>

We’re now ready to write the interpret function. It takes a code-array as argument, and changes the state of the operand and dictionary stacks according to what it finds there, doing any output indicated by the SPS program (using the stack operator) along the way. Note that your interpretSPS function needs to be recursive: interpretSPS will be called recursively when a name is looked up and its value is a code-array (i.e., function call), or when the body of the if , ifelse,repeat, and forall operators are interpreted.

<h2>Interpret the SPS code</h2>

<em># This will probably be the largest function of the whole project,  </em>

<em># but it will have a very regular and obvious structure if you’ve followed the plan of the assignment. </em>

<em># Write additional auxiliary functions if you need them.  </em>def interpretSPS(code): <em># code is a code array</em>     pass

Finally, we can write the interpreter function that treats a string as an SPS program and interprets it.




def interpreter(s): <em># s is a string </em>    interpretSPS(parse(tokenize(s)))

<h1>Testing</h1>

Sample unit tests for the interpreter are attached to the assignment dropbox. <strong>You should provide 5 additional test methods in addition to the provided tests.</strong> Make sure that your tests include several operators. You will loose points if you fail to provide tests or if your tests are too simple.

<h2>First test the parsing</h2>

Before even attempting to run your full interpreter, make sure that your parsing is working correctly. Make sure you get the correct parsed output for the testcases (see pages 7 through 12).

<strong>When you parse: </strong>

<ul>

 <li>Make sure that the integer constants are converted to Python integers.</li>

 <li>Make sure that the boolean constants are converted to Python booleans.</li>

 <li>Make sure that constant arrays are represented as Python lists. –         Make sure that code-arrays are represented as dictionaries.</li>

</ul>

<strong>Finally, test the full interpreter</strong>.  Run the test cases on the GhostScript shell to check for the correct output and compare with the output from your interpreter.

When you run your tests make sure to clear the opstack and dictstack.

input1 = “””

/square {dup mul} def

0 [-5 -4 3 -2 1]

{square add} forall

55 eq false and

“”” tokenize(input1) will return:




[‘/square’, ‘{‘, ‘dup’, ‘mul’, ‘}’, ‘def’, ‘0’,

‘[-5 -4 3 -2 1]’, ‘{‘, ‘square’, ‘add’, ‘}’, ‘forall’, ’55’, ‘eq’,

‘false’, ‘and’]

parse(tokenize(input1)) will return:




{‘codearray’: [‘/square’, {‘codearray’: [‘dup’, ‘mul’]}, ‘def’, 0, [-5,-4,

3, -2, 1], {‘codearray’: [‘square’, ‘add’ ]}, ‘forall’, 55, ‘eq’, False, ‘and’]}




After interpreter(input1) is called the opstack content will be:




[False]

input2 = “””             /x 1 def

/y 2 def

1 dict begin

/x 10 def

1 dict begin /y 3 def x y end

/y 20 def             x y             end             x y         “”” tokenize(input2) will return:




[‘/x’, ‘1’, ‘def’, ‘/y’, ‘2’, ‘def’, ‘1’, ‘dict’, ‘begin’, ‘/x’, ’10’,

‘def’, ‘1’, ‘dict’, ‘begin’, ‘/y’, ‘3’, ‘def’, ‘x’, ‘y’, ‘end’, ‘/y’,

’20’, ‘def’, ‘x’, ‘y’, ‘end’, ‘x’, ‘y’]

parse(tokenize(input2)) will return:




{‘codearray’: [‘/x’, 1, ‘def’, ‘/y’, 2, ‘def’, 1, ‘dict’, ‘begin’, ‘/x’,

10, ‘def’, 1, ‘dict’, ‘begin’, ‘/y’, 3, ‘def’, ‘x’, ‘y’, ‘end’, ‘/y’, 20,

‘def’, ‘x’, ‘y’, ‘end’, ‘x’, ‘y’]}




After interpreter(input2) is called the opstack content will be:




[10, 3, 10, 20, 1, 2]




input3 = “””

<ul>

 <li>2 1 3 2 2 3 5 5] dup</li>

</ul>

3

<ul>

 <li>2 1 4 2 3 4 5 1] 6 3 getinterval</li>

</ul>

Putinterval

“”” tokenize(input3) will return:




[‘[3 2 1 3 2 2 3 5 5]’, ‘dup’, ‘3’,

‘[4 2 1 4 2 3 4 5 1]’, ‘6’, ‘3’, ‘getinterval’, ‘putinterval’]

parse(tokenize(input3)) will return:




{‘codearray’: [[3, 2, 1, 3, 2, 2, 3, 5, 5], ‘dup’, 3, [4, 2, 1, 4, 2, 3,

4, 5, 1], 6, 3, ‘getinterval’, ‘putinterval’]}




After interpreter(input3) is called the opstack content will be:




[[3, 2, 1, 4, 5, 1, 3, 5, 5]]







input4 = “””

/a [1 2 3 4 5] def             a {dup mul} forall         “”” tokenize(input4) will return:




[‘/a’, ‘[1 2 3 4 5]’, ‘def’, ‘a’, ‘{‘, ‘dup’, ‘mul’, ‘}’, ‘forall’]

parse(tokenize(input4)) will return:




{‘codearray’: [‘/a’, [1, 2, 3, 4, 5], ‘def’, ‘a’, {‘codearray’: [‘dup’,

‘mul’]}, ‘forall’]}




After interpreter(input4) is called the opstack content will be:




[1, 4, 9, 16, 25]










input5 = “””             a [10 20 30 40 50] def             [4 2 0] {a exch get} forall

“”” tokenize(input5) will return:




[‘/a’, ‘[10 20 30 40 50]’, ‘def’, ‘[4 2 0]’, ‘{‘, ‘a’, ‘exch’, ‘get’, ‘}’,

‘forall’]  parse(tokenize(input5)) will return:




{‘codearray’: [‘/a’, [10, 20, 30, 40, 50], ‘def’, [4, 2, 0], {‘codearray’:

[‘a’, ‘exch’, ‘get’]}, ‘forall’]}




After interpreter(input5) is called the opstack content will be:




[50, 30, 10]




input6 = “””            /N 5 def

N { N N mul /N N 1 sub def} repeat

“”” tokenize(input6) will return:




[‘/N’, ‘5’, ‘def’, ‘N’, ‘{‘, ‘N’, ‘N’, ‘mul’, ‘/N’, ‘N’, ‘1’, ‘sub’,

‘def’, ‘}’, ‘repeat’]

parse(tokenize(input6)) will return:




{‘codearray’: [‘/N’, 5, ‘def’, ‘N’, {‘codearray’: [‘N’, ‘N’, ‘mul’, ‘/N’,

‘N’, 1, ‘sub’, ‘def’]}, ‘repeat’]}




After interpreter(input6) is called the opstack content will be:




[25, 16, 9, 4, 1]

input7 = “””             /n 5 def

/fact {

0 dict begin                 /n exch def                 n 2 lt

{ 1}

{n 1 sub fact n mul }                 ifelse                 end

} def             n fact         “”” tokenize(input7) will return:

[‘/n’, ‘5’, ‘def’, ‘/fact’, ‘{‘, ‘0’, ‘dict’, ‘begin’, ‘/n’, ‘exch’,

‘def’, ‘n’, ‘2’, ‘lt’, ‘{‘, ‘1’, ‘}’, ‘{‘, ‘n’, ‘1’, ‘sub’, ‘fact’, ‘n’, ‘mul’, ‘}’, ‘ifelse’, ‘end’, ‘}’, ‘def’, ‘n’, ‘fact’]

parse(tokenize(input7)) will return:




{‘codearray’: [‘/a’, [10, 20, 30, 40, 50], ‘def’, [4, 2, 0], {‘codearray’:

{‘codearray’: [‘/n’, 5, ‘def’, ‘/fact’, {‘codearray’: [0, ‘dict’, ‘begin’, ‘/n’, ‘exch’, ‘def’, ‘n’, 2, ‘lt’, {‘codearray’: [1]}, {‘codearray’: [‘n’,

1, ‘sub’, ‘fact’, ‘n’, ‘mul’]}, ‘ifelse’, ‘end’]}, ‘def’, ‘n’, ‘fact’]}  After interpreter(input7) is called the opstack content will be:




[120]

input8 = “””           /fact{                 0 dict                 begin

/n exch def                     1

n  {n mul /n n 1 sub def} repeat                 end             } def

6 fact         “”” tokenize(input8) will return:




[‘/fact’, ‘{‘, ‘0’, ‘dict’, ‘begin’, ‘/n’, ‘exch’, ‘def’, ‘1’, ‘n’, ‘{‘,

‘n’, ‘mul’, ‘/n’, ‘n’, ‘1’, ‘sub’, ‘def’, ‘}’, ‘repeat’, ‘end’, ‘}’,

‘def’, ‘6’, ‘fact’]

parse(tokenize(input8)) will return:




{‘codearray’: [‘/fact’, {‘codearray’: [0, ‘dict’, ‘begin’, ‘/n’, ‘exch’,

‘def’, 1, ‘n’, {‘codearray’: [‘n’, ‘mul’, ‘/n’, ‘n’, 1, ‘sub’, ‘def’]},

‘repeat’, ‘end’]}, ‘def’, 6, ‘fact’]}




After interpreter(input8) is called the opstack content will be:




[720]




input9 = “””

/sumArray { 0 exch {add} forall  } def

/x 5 def

/y 10 def

[1 2 3 add 4 x] sumArray

<ul>

 <li>7 8 9 y] sumArray</li>

 <li>2 5 mul 1 add 12] sumArray</li>

</ul>

“”” tokenize(input9) will return:




[‘/sumArray’, ‘{‘, ‘0’, ‘exch’, ‘{‘, ‘add’, ‘}’, ‘forall’, ‘}’, ‘def’,

‘/x’, ‘5’, ‘def’, ‘/y’, ’10’, ‘def’, ‘[1 2 3 add 4 x]’, ‘sumArray’,

‘[x 7 8 9 y]’, ‘sumArray’,'[y 2 5 mul 1 add 12]’, ‘sumArray’]

parse(tokenize(input9)) will return:




{‘codearray’: [‘/sumArray’, {‘codearray’: [0, ‘exch’, {‘codearray’:

[‘add’]}, ‘forall’]}, ‘def’, ‘/x’, 5, ‘def’, ‘/y’, 10, ‘def’, [1, 2, 3,

‘add’, 4, ‘x’], ‘sumArray’, [‘x’, 7, 8, 9, ‘y’], ‘sumArray’, [‘y’, 2, 5,

‘mul’, 1, ‘add’, 12], ‘sumArray’]}




After interpreter(input9) is called the opstack content will be:




[15, 39, 33]

input10 = “””

1 2 3 4 5 count copy 15 5 {exch sub} repeat 0 eq

“”” tokenize(input10) will return:




[‘1’, ‘2’, ‘3’, ‘4’, ‘5’, ‘count’, ‘copy’, ’15’, ‘5’, ‘{‘, ‘exch’, ‘sub’,

‘}’, ‘repeat’, ‘0’, ‘eq’]

parse(tokenize(input10)) will return:




{‘codearray’: [1, 2, 3, 4, 5, ‘count’, ‘copy’, 15, 5, {‘codearray’: [‘exch’, ‘sub’]}, ‘repeat’, 0, ‘eq’]}




After interpreter(input10) is called the opstack content will be:




[1, 2, 3, 4, 5, True]

input11 = “””

/xor {true eq {true eq {false} {true} ifelse } {true eq {true}

{false} ifelse } ifelse } def

true [true false and false true or false false] {xor} forall

“”” tokenize(input11) will return:




[‘/xor’, ‘{‘, ‘true’, ‘eq’, ‘{‘, ‘true’, ‘eq’, ‘{‘, ‘false’, ‘}’, ‘{‘, ‘true’, ‘}’, ‘ifelse’, ‘}’, ‘{‘, ‘true’, ‘eq’, ‘{‘, ‘true’, ‘}’, ‘{‘, ‘false’, ‘}’, ‘ifelse’, ‘}’, ‘ifelse’, ‘}’, ‘def’, ‘true’, ‘[true false

and false true or false false]’, ‘{‘, ‘xor’, ‘}’, ‘forall’]

parse(tokenize(input11)) will return:




{‘codearray’: [‘/xor’, {‘codearray’: [True, ‘eq’, {‘codearray’: [True,

‘eq’, {‘codearray’: [False]}, {‘codearray’: [True]}, ‘ifelse’]},

{‘codearray’: [True, ‘eq’, {‘codearray’: [True]}, {‘codearray’: [False]},

‘ifelse’]}, ‘ifelse’]}, ‘def’, True, [True, False, ‘and’, False, True, ‘or’, False, False], {‘codearray’: [‘xor’]}, ‘forall’]}




After interpreter(input11) is called the opstack content will be:




[False]