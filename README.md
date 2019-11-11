# compiler-in-python
In this project we will try to build a compiler using Python and some powerful libraries as it was explain by **Marcelo Andrade** in his post [Writing your own programming language and compiler with Python](https://blog.usejournal.com/writing-your-own-programming-language-and-compiler-with-python-a468970ae6df).
## Compilers
A compiler can be any program that translates one text into another. Since a computer can only read 1s and 0s, and humans write better Code than they do binary, compilers were made to turn that human-readable text into computer-readable *machine code*.

Compilers take source code and produce binary. They have several steps of processing to do before their programs are runnable:
1. Reads the individual characters of the source code you give it.
2. Sorts the characters into words, numbers, symbols, and operators. (These are called **_Tokens_**)
3. Takes the sorted characters and determines the operations they are trying to perform by matching them against patterns, and making a tree of the operations.
4. Iterates over evry operation in the tree made in the last step, and finally, generates the equivalent binary.

Our compiler can be divided intro three components: 
- Lexer
- Parser
- Code Generator

For the **Lexer** and **Parser** we’ll be using _RPLY_, really similar to _PLY: a Python library with lexical and parsing tools, but with a better API_. And for the **Code Generator**, we’ll use _LLVMlite_, a Python library for binding _LLVM_ components. Using LLVM, it is possible to optimize your compilation without learning compiling optimization, and _LLVMLite_ it´s a really good library to work with compilers.

![alt text](https://cdn-images-1.medium.com/max/1600/1*ttOYPPL-XJIf4zVZQUBzsQ.jpeg)
## Lexical Analysis
The first step is to split the input up character by character. This step is called **Lexical Analysis** or **Tokenization**. The major idea is that **we group characters together to form our words, identifiers, symbols, and more**. 

![alt text](https://cdn-images-1.medium.com/max/1600/1*GX7Ah6256Ya5WQwUnYGZmA.png)

We use minimal structures to define or tokens, for example:
```
print(4 + 4 - 2);
```
Our **Lexer** would divide this string into this list of **Tokens**
```
TOKEN('PRINT','print')
TOKEN('OPEN_PAREN','\(')
TOKEN('NUMBER','4')
TOKEN('SUM','+')
TOKEN('NUMBER','4')
TOKEN('SUB','-')
TOKEN('NUMBER','2')
TOKEN('SEMI_COLON',';')
```
## Parser
**Parsing**, **syntax analysis**, or **syntactic analysis** is the process of analysing a string of symbols, either in natural language, computer languages or data structures, conforming to the rules of a formal grammar. That´s why our second component in our compiler is the **Parser**. It will take the list of tokens as input and create a **AST** as output. 

![alt text](https://miro.medium.com/max/1164/1*gJCBr6E-lhYSiW0BsUU4mQ.jpeg)

Inside an `ast.py` file in which we put all clases that are going to be called on the Parser and create the AST. For example: 
`class Number()`, `class BinaryOp()`, `class Sum()`, `class Sub()`, `class Print()`.

Next we need to create the parser, in which we’ll use ParserGenerator from RPLY inside a file name `parser.py`. And finally, we’ll update our file `main.py` to combine **Parser** with **Lexer**.

We’ll see the output being the result of `print(4 + 4 — 2)`, which is equal to printing 6.
With these two components, we have a functional compiler that interprets our own language with Python. However, it still doesn’t create a machine language code and is not well optimized. To do this, we’ll need the most complex part of the compiler, code generation with LLVM.
