
# Converting Python to S-Expressions

`pysx` converts a Python module into an S-Expressified version of that module.

It adheres roughly to the data structure defined in the [AST module for Python](https://docs.python.org/3/library/ast.html).

The output fits the following the following grammar:


```
<mod> ::= (Module <stmt>*)

<stmt> ::=

        (FunctionDef
           (name <identifier>)
           (args <expr>*)
           (body <stmt>*)
           (decorator_list <expr>*)
           (returns <expr?>))

      | (ClassDef
           (name <identifier>)
           (bases <expr>*)
           (keywords <keyword>*)
           (starargs <expr?>)
           (kwargs <expr?>)
           (body <stmt>*)
           (decorator_list <expr>*))

      | (Return <expr?>)

      | (Delete <expr>*)

      | (Assign (targets <expr>*) (value <expr>))

      | (AugAssign <expr> <operator> <expr>)

      | (For (target <expr>) (iter <expr>) (body <stmt>*) (orelse <stmt>*))
      | (While (test <expr>) (body <stmt>*) (orelse <stmt>*))
      | (If (test <expr>) (body <stmt>*) (orelse <stmt>*))

      | (With [<withitem>*] <stmt>*)

      | (Raise)  |  (Raise <expr>)  | (Raise <expr> <expr>)

      | (Try (body <stmt>*)
             (handlers <excepthandler>*)
             (orelse <stmt>*)
             (finalbody <stmt>*))

      | (Assert <expr>)  |  (Assert <expr> <expr>)

      | (Import <alias>)
      | (ImportFrom (module <identifier?>)
                    (names <alias>*)
                    (level <int?>))

      | (Global <identifier>+)
      | (Nonlocal <identifier>+)

      | (Expr <expr>)
      | (Pass) 
      | (Break)
      | (Continue)


<expr> ::=
       (BoolOp <boolop> <expr>*)
     | (BinOp <expr> <operator> <expr>)
     | (UnaryOp <unaryop> <expr>)

     | (Lambda <arguments> <expr>)

     | (IfExp <expr> <expr> <expr>)

     | (Dict (keys <expr>*) (values <expr>*))

     | (Set <expr>*)

     | (ListComp <expr> <comprehension>*)
     | (SetComp <expr> <comprehension>*)
     | (DictComp <expr> <expr> <comprehension>*)
     | (GeneratorExp <expr> <comprehension>*)

     -- the grammar constrains where yield expressions can occur
     | (Yield)  | (Yield <expr>)
     | (YieldFrom <expr>)

     -- need sequences for compare to distinguish between
     -- x < 4 < 3 and (x < 4) < 3
     | (Compare (left        <expr>) 
                (ops         <cmpop>*)
                (comparators <expr>*))

     | (Call (func <expr>)
             (args <expr>*)
             (keywords <keyword>*)
             (starags <expr?>)
             (kwargs <expr?>))

     | (Num <number>)
     | (Str <string>)

     | Bytes(bytes s)

     | (NameConstant True|False|None)

     | (Ellipsis)

     -- the following expression can appear in assignment context:
     | (Attribute <expr> <identifier>)
     | (Subscript <expr> <slice>)
     | (Starred <expr>)

     | (Name <identifier>)
     | (List <expr>*)

     | (Tuple <expr>*)


<expr?> ::= <expr>
         | #f


<slice> ::= (Slice <expr?> <expr?> <expr?>)
         |  (ExtSlice <slice>*) 
         |  (Index <expr>)

<boolop> ::= And | Or 

<operator> ::= Add | Sub | Mult | Div | Mod | Pow | LShift 
               | RShift | BitOr | BitXor | BitAnd | FloorDiv

<unaryop> ::= Invert | Not | UAdd | USub

<cmpop> ::= Eq | NotEq | Lt | LtE | Gt | GtE | Is | IsNot | In | NotIn

<comprehension> ::= [for <expr> in <expr> if <expr>*]

<excepthandler> ::= [except <expr?> <identifier?> <stmt>*]

<arguments> ::= (Arguments
                   (args <arg>*)
                   (arg-types <expr>*)
                   (vararg <arg?>) 
                   (kwonlyargs <arg>*)
                   (kwonlyarg-types <expr>*)
                   (kwdefaults <expr>*)
                   (kwarg <arg?>) 
                   (defaults <expr>*))
 
<arg> ::= <identifier>

<arg?> ::= <arg>
        | #f

<keyword> ::== [<identifier> <expr>]

<alias> = (identifier name, identifier? asname)

<withitem> ::= [<expr> <expr?>]

<int?> ::= <int> | #f
```


