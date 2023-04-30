Download Link: https://assignmentchef.com/product/solved-mpp-assignment-3-static-checker
<br>



Contents

1 Specification                                                                                             2

2 Static Checker                                                                                          2

2.1 Redeclared Variable/Function/Procedure/Parameter:      3

2.2 Undeclared Identifier/Function/Procedure:  ………………………………………. 3

2.3 Type Mismatch In Statement:  …………………………………………………….. 3

2.4 Type Mismatch In Expression:  …………………………………………………… 4

2.5 Function not return:  ……………………………………………………………….. 4

2.6 Break/Continue not in loop:  ……………………………………………………….. 4

2.7 Unreachable statement:  …………………………………………………………… 4

2.8 No Entry Point:  …………………………………………………………………….. 5

2.9 Unreachable function:  …………………………………………………………….. 5

3 Submissions                                                                                              5

4 Plagiarism                                                                                                5

5 Change Log                                                                                               5




Assignment 3version 1.2

After completing this assignment, you will be able to

<ul>

 <li>explain the principles how a compiler can check some semantic constraints such as type compatibility, scope constraints,… and</li>

 <li>write a medium ( 300-500LOC) Scala program to implement that.</li>

</ul>

1 Specification

In this assignment, you are required to write a static checker for a program written in MP. To complete this assignment, you need to:

<ul>

 <li>Read carefully the specification of MP language</li>

 <li>Download and unzip file assignment3.zip</li>

 <li>If you are confident on your Assignment 2, copy your MP.g4 into src/main/mc/parser and your <a href="http://ASTGeneration.py">py</a> into src/main/mc/astgen and you can test your Assign­ment 3 using MP input like the first three tests (400-402).</li>

 <li>Otherwise (if you did not complete Assignment 2 or you are not confident on your Assignment 2), don’t worry, just input AST as your input of your test (like test 403-405).</li>

 <li>Modify <a href="http://StaticCheck.py">py</a> in src/main/mc/checker to implement the static checker andmodify <a href="http://CheckSuite.py">CheckSuite.py</a> in src/test to implement 100 testcases for testing your code.</li>

</ul>

2 Static Checker

A static checker plays an important role in modern compilers. It checks in the compiling time if a program conforms to the semantic constraints according to the language specifica­tion. In this assignment, you are required to implement a static checker for MP language. The input of the checker is in the AST of a MP program, i.e. the output of the assignment 2. The output of the checker is nothing if the checked input is correct, otherwise, an error message is released and the static checker will stop immediately

For each semantics error, students should throw corresponding exception given in <a href="http://Stat-icError.py">Stat­</a><u>icError.py</u> inside folder src/main/mc/checker/ to make sure that it will be printed out the




same as expected. Every test-case has at most one kind of error. The semantics constraints required to check in this assignment are as follows.

2.1 Redeclared Variable/Function/Procedure/Parameter:

An identifier must be declared before used. However, the declaration must be unique in its scope. Otherwise, the exception Redeclared(&lt;kind&gt;,&lt;identifier&gt;) is raised, where &lt;kind&gt; is the kind of the &lt;identifier&gt; in the second declaration. The scope of an identifier (variable, function, procedure, parameter) is informally described as in Section 8 of MP specification.

2.2 Undeclared Identifier/Function/Procedure:

The exception Undeclared(&lt;kind&gt;,&lt;identifier&gt;) is raised when there is an &lt;identifier&gt; is used but its declaration cannot be found. The &lt;kind&gt; is Identifier when the &lt;identifier&gt; is used as a variable in an expression. The &lt;kind&gt; is Procedure when the &lt;identifier&gt; is used as a procedure name in a call statement. In the case that the &lt;identifier&gt; is used as a function name in a call expression, the &lt;kind&gt; is Function.

2.3 Type Mismatch In Statement:

A statement must conform the corresponding type rules for statements, otherwise the ex­ception TypeMismatchInStatement(&lt;statement&gt;) is thrown.

The type rules for statements are as follows:

– The type of a conditional expression in an if statement must be boolean.

– The type of &lt;identifier&gt;, &lt;expression 1&gt; and &lt;expression 2&gt; in a for statement must be integer.

– The type of condition expression in while statement must be boolean.

– In an assignment statement, there are many sub-assignments whose left-hand side (LHS) can be in any type except string and array type. The right-hand side (RHS) is either in the same type as that of the LHS or in the type that can coerce to the LHS type. In MP, just the integer can coerce to the float.

– The return statement of a procedure must be without any expression while the return statement of a function must be with an expression whose type can be coerced to the return type of the enclosed function. When the return type of a function is an array, the expression of all return statement in the function must be in array type whose lower bound, uppper bound and element type must be the same as those of the return type of the function.




– For a procedure call &lt;procedure name&gt;(&lt;args&gt;), the number of the actual parame­ters must be the same as that of the formal parameters of the corresponding procedure. The rule for an assignment is applied to parameter passing where a formal param­eter is considered as the LHS and the corresponding actual parameter is the RHS. An exception is an array which can be used as an actual parameter to pass to the corresponding parameter in an array type when their lower bound, upper bound and element type are the same.

2.4 Type Mismatch In Expression:

An expression must conform the type rules for expressions, otherwise the exception TypeMis­matchInExpression(&lt;expression&gt;) is raised.

The type rules for expression are as follows:

– For an array subcripting E1[E2], E1 must be in array type and E2 must be integer.

– For a binary and unary expression, the type rules are described in the MP specifica­tion.

– The rule type for a function call is appled similar to that of procedure. 2.5 Function not return:

A function must return something in every its execution paths otherwise the exception FunctionNotReturn(&lt;function name&gt;) will be thrown.

2.6 Break/Continue not in loop:

A break/continue statement must be inside directly or indirectly a loop (for or while statement) otherwise the exception Break/ContinueNotInLoop() will be raised.

2.7 No Entry Point:

There must be a procedure void main () somewhere in a MP program. Otherwise, the exception NoEntryPoint() is raised.

The following errors are for gifted students:

2.8 Unreachable statement:

An unreachable statement is a statement in a function/procedure which the control flowcannot reach. For example, the statement after a return statement is an unreachable state-ment. The value of an expression is ignored when determining an unreachable statement.




For instance, the stmt in if (false) stmt is reachable as the value of the condition expres­sion is ignored. The exception UnreachableStatement(&lt;statement&gt;) will be raised when the &lt;statement&gt; is the first unreachable statement.

2.9 Unreachable function/procedure:

A function/procedure, except main function, must be invoked by another function/procedure, otherwise, the exception Unreachable(&lt;kind&gt;,&lt;name&gt;) is raised.

3 Submissions

This assignment requires you submit 2 files: <a href="http://StaticCheck.py">StaticCheck.py</a> containing class Static-Checker with the entry method check, and <a href="http://CheckSuite.py">CheckSuite.py</a> containing 100 testcases.

The deadline is announced in course website and that is also the place where you MUST submit your code. The website <a href="http://www.cse.hcmut.edu.vn/onlinejudge">www.cse.hcmut.edu.vn/onlinejudge</a> is the place you can check your code before submitting in course website.

4 Plagiarism

<ul>

 <li>You must complete the assignment by yourself and do not let your work seen by someone else.</li>

</ul>

If you violate any requirement, you will be punished by the university rule for plagiarism.




5 Change Log