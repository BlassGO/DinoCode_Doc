## Structure
> DinoCode uses blocks of code stored in memory. These blocks can be considered as functions.
  * **Blocks**
    
    Blocks have a unique identifier (`TAG`). This label must always start with a colon `:`, and can have any alphanumeric character and underscores (a-z, A-Z, 0-9, _).
    ```shell
    :This_is_a_label
    ```
   Each `TAG` separates a section of code, and can be called (in different contexts) during execution.
       > * Blanks before the `TAG` are not allowed.
       > * Duplicate `TAG`s will simply be overwritten.
       > * A section ends when a new `TAG` appears.
       > * A section can only be called after its definition, sections cannot be called from a line that comes before its `TAG`.
    
    All sections are kept in memory except for `:main`, which is executed directly at runtime and allows control of the other sections.
       > Considering this, the standard structure of a DinoScript would look like this:
      ```javascript
      :custom_tag

      :custom_tag_2

      :custom_tag_3

      :main
         Print "Welcome!"
      ```

     
## Comments
> Comments are text that should be ignored (not interpreted).
  * **Simple one line comment** (#Comment)
    ```shell
    #Just ignore

    Message With "test" #Just ignore
    ```
  * **Multi-Line Comment** (#* Lines *#)
    ```
    #*  just ignore
    and that
    and that  *#
    ```
    ```
    #*
     Line 1
     Line 2
     Line 3
    *#
    ```
    > **WARNING:** It is not possible to contain a `TAG` section in a multi-line comment, once previous sections have already been defined, this is because `TAG` Sections are not processed and are only stored in memory.
       >
       > This will work:
          #*
          :ignore_this_tag
          code
          code
          *#
          :NOT_ignore_this_tag
       > This too:
          #:ignore_this_tag
          :NOT_ignore_this_tag
       >
       > But this will NOT work as expected, instead `#*` will be stored in memory inside `:previous_tag` and `code code *#` will be stored in the second TAG (`:This_tag_will_NOT_be_ignored`).
          :previous_tag
          #*
          :This_tag_will_NOT_be_ignored
          code
          code
          *#
          :other_tag

## Data Types
> To define a `DATA` it is necessary to save it in a `Variable`, Variables in DinoCode do not require a previous declaration as in many languages, it is enough to assign a value to any Variable.
   > ```javascript
   > Set VARIABLE With DATA
   > ```
   > If you want to clear a Variable you can use this logic:
   > ```javascript
   > Set VARIABLE With
   > ```
> 5 types of DATA are supported:
>
> |          DATA         |                          EXAMPLE                         |
> |:---------------------:|:--------------------------------------------------------:|
> |         Integer        |                     Set X With 256                |
> | Float<br>(Decimal) |                    Set Y With 7.324                 |
> |         Strings        |               Set HUH With "Hello World!"              |
> |       Booleans       | Set TEST With TRUE<br>Set TEST With FALSE |
> |        Arrays       |      Set LIST With "First" "Second" "Third"     |
>
> **NOTE:** Arrays in DinoCode start from position 1 and not from 0 as in other languages, this facilitates the logic of use.

## Arguments
> Arguments are parameters that you want to send to `Native Functions` or `TAG` Sections.

> Two types of arguments are supported:
> * Double quotes [Literal Arguments](#delimiter).
> ```javascript
> Print "Hello world!"
> ```
> * [Expressive Arguments](#expressions).
> ```javascript
> Print % 5 + 5 %
> ```

> When Arguments are sent to `TAG` Sections, they are stored in an Array for ease of use and ordering. This Arrangement shares the same name as the Section identifier, that is, its `TAG`.
> ```javascript
> :Huh
>    Print Huh[1]
>    Print Huh[2]
> 
> :main
>    Huh "Literal Argument 1" "Literal Argument 2"
> ```
>
> Some DinoCode Native Functions support the use of [CONNECTORS](#connectors) , connectors can also be used when passing Arguments to `TAG` Sections.
> 
> **->** When using Native Connectors, the Array name will not respect the Connector name if it used another language like Spanish, its standard name will be in English.
> ```javascript
> :Huh
>    Print Huh[1]
>    Print Huh[2]
>    Print With[1]
> 
> :main
>    Huh "Literal Argument 1" "Literal Argument 2" With "Connector Argument 1"
> ```

## Expressions
> Expressions are special code references that have a `Resolution`, ie an expression will produce a result that may be smaller or extremely larger than the initial expression.

<!-- tabs:start -->

<!-- tab:%Expression% -->
The Percentage Expressions `% %` are used to give `numbers`, `strings` or `variables` additional processing and get their result.

**->** Additionally, they are the only ones that can access external Variables defined within the main program.

### Variables
> By default, no additional Expression is required to refer to a Variable:
> ```javascript
> Set NAME With "BlassGO"
> Message "Information" With NAME
> ```
> 
> However, it is also possible to get the value of a Variable within a [Literal Argument](#delimiter) using `Expressions` and thus build more complex information.
> ```javascript
> Set NAME With "BlassGO"
> Message "Information" With "User name: %NAME%"
> ```
> 
> Variable concatenation is allowed within an Expression.
> ```javascript
> Set NAME With "Blass"
> Set LAST_NAME With "GO"
> Message "Information" With "User name: % NAME LAST_NAME %"
> ```
> 
> In addition, new strings can be concatenated within the same Expression.
> ```javascript
> Set NAME With "Blass"
> Set LAST_NAME With "GO"
> Message "Information" With "User name: % NAME " and " LAST_NAME %"
> ```

### Arithmetic operators
> As detailed in [Data types](#data-types) both integers and decimal numbers are supported, this implies that if the Result of a percentage Expression belongs to either of these two, it can be used without the need to enclose it in a [Literal Argument](#delimiter)
> 
> ```javascript
> Message "Result" With % 5.6 + 265 %
> ```
> 
> | Sign |              Operation             |     Example     |
> |:-----:|:----------------------------------:|:---------------:|
> |   +   |                Addition                |     % 5+5 %     |
> |   -   |                Subtraction               |     % 10-5 %    |
> |   *   |           Multiplication           |   % 7.8 * 67 %  |
> |   /   |              Division              |     % 3/2 %     |
> |   //  | Integer Division<br>(No decimals) |     % 3//2 %    |
> |   **  |            Potentiation            |    % 3**21 %    |
> |  ( )  |             Grouping             | % (3//2+4)**4 % |

<!-- tab:$(Expression) -->
Evaluated or calling expressions `$( )` are used to call any existing Function, this includes `AHK Native Functions`, `DinoCode Native Functions` and `TAG` Sections.

**->** Unlike Percent Expressions, the result of Evaluated Expressions is usually considered `LITERAL`, it is generally not necessary to enclose them in a [Literal Argument](#delimiter).

**->** If this Expression is used from a Percentage Expression `% %` or similar, it is necessary to anticipate the output `DATA` type. Percentage Expressions are more sensitive, and if `$( )` produces a STRING, it is expected to be enclosed in quotes.

>
> This example uses the native AHK function `StrLen` to get the length of the string and store it in the `LENGTH` variable.
> ```javascript
> Set LENGTH With $(StrLen "How many characters does this string have?")
> Message "Information" With "The string has a length of: %LENGTH%"
> ```

<!-- tabs:end -->


## Counters
> They add or subtract a single unit (1), to any numerical variable. They are shorteners that save the approach of a complete mathematical operation.
>

> **Up Counter:**
> ```javascript
> :MAIN
>    Set X With 1
>    X++
>    Print X
> ```
> The previous example would be equivalent to:
> ```javascript
> :MAIN
>    Set X With 1
>    Set X With % X + 1 %
>    Print X
> ```

> **Down Counter:**
> ```javascript
> :MAIN
>    Set X With 1
>    X--
>    Print X
> ```
> The previous example would be equivalent to:
> ```javascript
> :MAIN
>    Set X With 1
>    Set X With % X - 1 %
>    Print X
> ```



## Conditionals
> Conditional blocks are pieces of code of one or several lines, which will only be executed if the imposed condition is TRUE.
>
> **->** In DinoCode, the indentation (or left margin) is used to determine which lines are part of the conditional block, taking the beginning of the condition as a reference.
>
> ```javascript
> If <condition>
>    conditional line
>    conditional line
> line out of condition
> ```

> To determine whether a condition is `TRUE` or `FALSE`, the interpreter resolves the condition to a Percentage Expression `% %` and parses the result following this logic:
>
> |         **Result**         | **TRUE** | **FALSE** |
> |:-----------------------------:|:-------------:|:---------:|
> | Any value other than 0 |       X       |           |
> |               0               |               |     X     |
> |          Null (empty)         |               |     X     |


> 
> * **Relational Operators**
> 
>    They establish if there is some kind of relationship between two `DATA`.
>    
>    | **Operator** | **Example** |                                    **Relationship**                                    |
>    |:------------:|:-----------:|:----------------------------------------------------------------------------------:|
>    |      ==      |     x==y    |                            x is completely equal to y                            |
>    |       =      |     x=y     | x is equal to y <br>(If they are STRINGS, they are not case sensitive) |
>    |      !=      |     x!=y    |                                 x is different from y                                |
>    |       >      |     x>y     |                                  x is greater than y                                  |
>    |       <      |     x<y     |                                  x is less than y                                  |
>    |      >=      |     x>=y    |                              x is greater than or equal to y                              |
>    |      <=      |     x<=y    |                              x is less than or equal to y                              |
>    |      ~=      |     x~=y    |      When y is a Regex pattern,<br>it checks if the pattern matches x      |

> 
> * **Logical Operators**
> 
>    Mainly used to join several Conditions as a single Condition.
>    
>    | **Operator** |    **Example**   |                                      **Explanation**                                      |
>    |:------------:|:----------------:|:-----------------------------------------------------------------------------------------:|
>    |    && AND    |  (x=y) && (x>4)  | If (x=y) is True **AND** (x>4) is also True,<br>the entire condition is True |
>    |    \|\| OR   | (x=y) \|\| (x>4) |     If (x=y) is True **OR** (x>4) is True,<br>the entire condition is True     |
>    |     ! NOT    |      !(x=y)      |                   If (x=y) is True, reverse it to FALSE or vice versa                   |


> In this example, it checks if `X` is equal to `5`, since the condition is true, `X` is printed.
> ```javascript
> :MAIN
>    Set X With 5
>    If X=5
>       Print X
> ```
> In this example, it checks if `STRING` is equal to `Hello world`, since the condition is NOT met, nothing is printed.
> ```javascript
> :MAIN
>    Set STRING With "H u h"
>    If STRING="Hello world"
>       Print STRING
> ```
> In this example, it is checked if `X` is equal to `5` and if `STRING` is equal to `Hello world`, since this second one is not fulfilled, the whole condition is FALSE, therefore, nothing is printed.
> ```javascript
> :MAIN
>    Set X With 5
>    Set STRING With "H u h"
>    If (x=5) && (STRING="Hello world")
>       Print X
>       Print STRING
> ```

> 
> * **If - Else**
> 
>   `Else` sets an optional block, which will be executed only if the condition of the previous block failed.
>
>     ```javascript
>     If <condition>
>        conditional line
>        conditional line
>     Else
>        conditional line
>        conditional line
>     line out of condition
>     ```
>    
>    In addition, `Else` can start a new condition, being able to create a sequence of several conditional blocks, where only one of them will be executed.
>
>     ```javascript
>     If <condition>
>        conditional line
>        conditional line
>     Else If <condition>
>        conditional line
>        conditional line
>     Else If <condition>
>        conditional line
>        conditional line
>     Else
>        conditional line
>        conditional line
>     line out of condition
>     ```

> In this example, only the condition of the third block is met, therefore only that block will be executed.
> 
> **->** Although the first condition is TRUE, it has a logical negation operator `!`, which reverses its result to FALSE.
> ```javascript
> :MAIN
>    Set X With 5
>    Set STRING With "H u h"
>    If !(X=5)
>       Print X
>    Else If (STRING="Hello world")
>       Print STRING
>    Else If (X=5) && (STRING="H u h")
>       Print X 
>       Print STRING
>    Else
>       Print "I can't find anything"
> ```

## Loops
> They are a type of [CONDITIONAL](#conditionals) block, with the difference that their block will be executed as many times as necessary, until its condition is FALSE.
>
> ```javascript
> While <condition>
>    conditional line
>    conditional line
> line out of loop
> ```

> In this example, it checks if `X` is equal to `5`, as the condition is always met, an infinite loop will be created, that is, the Script will be trapped inside the loop and will not continue.
>
> **->** When defining conditions for loops, you must carefully analyze the logic of the condition.
> ```javascript
> :MAIN
>    Set X With 5
>    While X=5
>       Print X
>    Print "Out of the Loop"
> ```

> In this example, it checks if `X` is less than or equal to `5`, such that if the value of `X` exceeds `5`, the condition will be FALSE and the loop will end.
> 
> **->** To make sure that the value of `X` increments, you can use the `++` operator, which adds `1` to any variable.
>
> ```javascript
> :MAIN
>    Set X With 1
>    While X<=5
>       Print X
>       X++
>    Print "Out of the Loop"
> ```


> DinoCode supports a `Until` loopback, which instead of looping as long as the condition is TRUE, loops as long as the condition is FALSE.
>
> ```javascript
> :MAIN
>    Set X With 0
>    Until X=5
>       X++
>       Print X
>    Print "Out of Loop"
> ```

> * **Break**
> 
>   Break has a unique value for loops, allowing the loop to be stopped from within its block.
>
> ```javascript
> :MAIN
>    Set X With 1
>    While TRUE
>       If X<=5
>          Print X
>       Else
>          Break
>       X++
>    Print "Out of Loop"
> ```

> **->** When using additional Expressions, it should be noted that only Expressions calling `$( )` are kept on each iteration. Percentage Expressions `% %` are resolved on the first line read, keeping only their initial Resolution but NOT the Expression itself, to preserve it, it must be placed inside `$( )`.
> ```javascript
> :CHECK
>    If (CHECK[1]<=CHECK[2])
>       Return TRUE
>    Else
>       Return FALSE
> :MAIN
>    Set X With 1
>    While $(CHECK X 5)
>       Print X
>       X++
>    Print "Out of Loop"
> ```



## Functions
> DinoCode `TAG` Sections can work in two ways:
  * **As function**
     
     This call is assumed to be `LOCAL`, ie it will work in a separate space that will not affect the block from which it was called. Therefore, any `DATA` defined during its execution will be cleared on completion. This is the default call for any Section.

     > In this example, `:LOCAL_SECTION` is called from `:MAIN` and prints the locally defined `X` value, then attempts to make a second print of the same `X` value, however this second print will output an Error, since `X` does not exist inside `:MAIN` and its value disappeared at the end of the execution of `:SECTION_LOCAL`.
     > ```javascript
     > :LOCAL_SECTION
     >    Set X With 7.8
     >    Print X
     > 
     > :MAIN
     >    LOCAL_SECTION
     >    Print X
     > ```
     > 
     > In this example, each print of `X` will have a different value, corresponding to the one defined in its Section.
     > ```javascript
     > :LOCAL_SECTION
     >    Set X With 7.8
     >    Print X
     > 
     > :MAIN
     >    Set X With 1000
     >    LOCAL_SECTION
     >    Print X
     > ```

     One of the main advantages of functions is that they can receive Arguments (Parameters), and return a Resolution.
     > In this example, two numeric Arguments are sent to `:SUM` from `:MAIN` and their sum is printed.
     > ```javascript
     > :SUM
     >    Print % SUM[1] + SUM[2] %
     > 
     > :MAIN
     >    SUM 1000 7.8
     > ```
     >
     > In this example, `:SUM` is used only to [RETURN](#return) the sum of the two numerical Arguments that it received and this Returned Result is stored in the `RESULT` Variable of `:MAIN` to properly print it.
     > ```javascript
     > :SUM
     >    Return SUM[1] + SUM[2]
     > 
     > :MAIN
     >    Set RESULT With $(SUM 1000 7.8)
     >    Print RESULT
     > ```
     

  * **As Extension**
     
     This call is assumed as an extension of the block from which it was called, that is, it will NOT work locally, instead, the `DATA`s are shared and any modification to them will affect both blocks. This also applies to new data defined during its execution.

     On the other hand, it does not support Arguments.
     > ```javascript
     > :DATA
     >    Set Y With 10
     >    Set Z With 2.5
     > 
     > :MAIN
     >    Set X With 66
     >    Section DATA
     >    Print % (X+Y)*Z %
     > ```

   * **Global**
     > A Variable can be globalized, in such a way that it NEVER works locally (it can be modified or defined from within any function).
     > ```javascript
     > GLOBAL Variable Variable ...
     > ```
     > In this example, `X` is defined as a `GLOBAL` variable, and therefore can be used even after the execution of the function.
     > ```javascript
     > :LOCAL_SECTION
     >    Global X
     >    Set X With 7.8
     >    Print X
     > :MAIN
     >    LOCAL_SECTION
     >    Print X
     > ```


## Return
Return is a special action used mainly in Functions, it allows a Section to return a `DATA` and end its execution. In DinoCode this action uses the same logic as [Percentage Expressions](#expressions), being able to return numbers, strings or variables, but additionally supports the return of complete Arrays.

> ```javascript
> :MULTI
>    Return "Result: " MULTI[1] * MULTI[2]
> 
> :MAIN
>    Print $(MULTI 1000 7.8)
> ```
> 
> As mentioned in [Arguments](#arguments), parameters passed to Functions become Arrays, it is possible to take advantage of this to stack more information and return a larger Array.
> ```javascript
> :DATA
>    Set DATA[3] With "Additional data"
>    Return DATA
> 
> :MAIN
>    Set INFO With $(DATA "Blass" "GO")
>    Print INFO[1]
>    Print INFO[2]
>    Print INFO[3]
> ```
>
> **TIP:**  Return can be used within a `$( )` Expression and while it is equivalent to using a Percentage Expression (in most cases), it can be useful.
> ```javascript
> :MAIN
>    Set X With 1000
>    Set Y With 7.8
>    Print $(Return "Result: " X * Y)
> ```

## Connectors
> Connectors in DinoCode are a concept that allows you to create `Natural Sentences` (close to human natural language). They function as a binding to the `Main Function` and bundle their own Arguments.
> 
> **->** Connectors can have any alphanumeric characters and underscores (same as `TAG`).

> Native Connectors:
> 
> | **Connector** |   **Support**  | **Nickname List** |
> |:------------:|:--------------:|:-------------------:|
> |      With     | English/Spanish |       =, Con       |
> |      In      | English/Spanish |          En         |
> |     To    | English/Spanish |       Hacia, unto      |
>

> In addition to the Native Connectors, it is possible to add new connectors:
>
> ```javascript
> :TAG -> Connector, Connector, Connector, ...
> ```
> In this example, a new Connector `Subtract` is defined for the `Add` function, so the function can generate different results depending on the Connectors involved during its call.
> 
> ```javascript
> :Add -> Subtract
>    If Subtract[1]
>       Message "Result" With % Add[1] + With[1] - Subtract[1] %
>    Else
>       Message "Result" With % Add[1] + With[1] %
> 
> :main
>    Add 10 With 5
>    Add 2.3 With 100 Subtract 90
>
> ```
> It is possible to override Native Connectors.
> ```javascript
> :Add -> With, Subtract
>    If Subtract[1]
>       Message "Result" With % Add[1] + With[1] - Subtract[1] %
>    Else
>       Message "Result" With % Add[1] + With[1] %
> 
> :main
>    Add 2.3 With 100 Subtract 90
>
> ```
> 
> **->** This is a very practical example of the use of Connectors, from the Native Function `SubStr`, a new simplified Function (`Extract`) is created.
> 
> ```javascript
> :Extract -> From, To, Using
>    Return "$(SubStr Using[1] From[1] To[1])"
> 
> :main
>    Set STRING With "Hello world!"
>    Message "Result" With $(Extract from 1 to 5 using STRING)
> 
> ```

## Nicknames
> Nicknames allow the same Function to be called with different names.
> 
> **->** They do not have character restrictions, that is, even though the name of a Function does not allow special characters, its Nicknames can have them.
>
> ```javascript
> :TAG -> TAG = [Nickname_1, Nickname_2, Nickname_3]
> ```

> In this example, the nickname `+` is assigned to the function `Add`, allowing a call with either name.
> 
> ```javascript
> :Add -> Add = [+]
>    Message "Result" With % Add[1] + With[1] %
> 
> :main
>    Add 2.3 With 100
>    + 2.3 With 100
>
> ```
> 
> Connectors also support nicknames, they can even have the same nickname as the main Function.
> 
> **->** If a nickname is defined for a Native Connector, using its standard (English) name, its native nicknames in other languages ​​will be supported, in addition to the new nicknames.
> 
> ```javascript
> :Add -> Add = [+], With = [+]
>    Message "Result" With % Add[1] + With[1] %
> 
> :main
>    Add 2.3 With 100
>    + 2.3 + 100
>
> ```
>
> Using this logic, it is possible to support multiple languages ​​(although it is recommended not to abuse their use, since they represent an additional expense during the recognition of the Function).
> ```javascript
> :Add -> Add = [Sumar, Добавлять, 追加], With = [к, と]
>    Message "Result" With % Add[1] + With[1] %
> 
> :main
>    Add 2.3 With 100
>    Sumar 2.3 Con 100
>    Добавлять 2.3 к 100
>    追加 2.3 と 100
>
> ```

## Delimiter
> By default, double quotes `" "` group parameters that should be taken as literal text (so if you want to add a literal double quote inside a group of double quotes, you must [Escape it](#escape)).
```javascript
Message "T i t l e" With "Huh!"
```
   > * Inside a group of quotes it is still possible to use Expressions: `$( )`, `% %`.
   > * Resolution of any Expression within a group of quotes will be contained, even if these results contain multiple double quotes they will not break the grouping.

## Escape
> It allows "hiding" characters, in such a way that even if the character has some special value for the interpreter, it is ignored. Its use is exclusively focused on [Literal Arguments](#delimiter)
  * **Single character escape**
    > Any character preceded by a backslash `\` will be escaped.
    >
    > **->** It is possible to change the escape character `\` to any other, for example, if you want to change it to `/`, it would be enough with: `Escape /`
    >
    > ```javascript
    > Message "Test" With "Escaped Characters: \" \% \$"
    > ```
    > If the escaped character is alphabetic, it will be considered as a special character.
    > ```javascript
    > Message "Test" With "First Line\nSecond Line\n\tThird Line with Tab"
    > ```
    > **WARNING**: There is a difference between using a Lowercase and an Uppercase alphabetic escaped character. On Linux distros, the `\n` character known as "linefeed" is often used to represent a newline, however, on Windows operating systems, a combination of "carriage return"-->`\r`, next to "linefeed"-->`\n` is more appreciated. For convenience in DinoCode the use of `\n` does NOT actually represent a "linefeed", but the combination `\r\n`. To use a real linefeed, it can be persisted by changing the character to Upper Case, ie `\N`.

## Threads
> It allows executing `TAG` Sections asynchronously, that is, the section will use a separate execution space.
> 
> **->** The `THREADS` are considered extensions of the block, that is, the data is shared (NOT LOCAL).
> ```javascript
> Thread TAG TIME
> ```

> In this example, the `ALERT` section starts as a secondary `THREAD` that will be called every `1000` milliseconds (1s), and by not stopping the flow of the Script, it gives way to the Message of the main THREAD (`MAIN`) , then, after a second, a second message will be displayed that corresponds to the new THREAD created, and said THREAD is deleted (Avoiding continuing with the call every 1000 milliseconds).
> ```javascript
> :ALERT
>   Msg "Second" With "Secondary THREAD message"
>   Thread ALERT Delete
> :MAIN
>   Thread ALERT 1000
>   Msg "First" With "Main THREAD message"
> ```
>
> This is a much more practical example of using THREADS, the `INCREASE` section acts as a counter that will increment every `100` milliseconds and destroy the secondary THREAD only when 100 units are reached.
> ```javascript
> :INCREASE
>   COUNT++
>   Print COUNT
>   IF (COUNT >= 100)
>      Thread INCREASE Delete
>      Msg "Thread 2" With "The counter will NO longer increment."
> :MAIN
>   Set COUNT With 0
>   Thread INCREASE 100
>   Msg "Thread 1" With "This message does not prevent the counter from being incremented."
> ```

## Evaluator
> Allows you to `evaluate` strings as percentage expressions `% %`.
> 
> **->** Multiple Expressions are supported in the same call, each must be separated by a comma `,`.
> 
> **->** Returns an Array, where each element corresponds to each Expression evaluated.
> 
> ```javascript
> Eval "Expression 1, Expression 2, Expression 3, ..."
> ```

> To include a `$( )` expression within the evaluation, the special characters must be escaped `\$( )`.
> ```javascript
> :MAIN
>    Set HUH With "Huh?"
>    Set RESULTS With $(Eval "5+10, (2*5)**6, HUH, \$(Input)")
>    Print RESULTS[1]
>    Print RESULTS[2]
>    Print RESULTS[3]
>    Print RESULTS[4]
>
> ```


## Graphic interface
> Allows you to take advantage of AHK's ability to create graphical user interfaces (GUIs).
>
> **->** Natively only `Gui` function is supported, existing tags or special functions for GUI control in AHK may not be available.


> **->** Some basic examples are shown below, for more information [CLICK HERE](https://www.autohotkey.com/docs/v1/lib/Gui.htm ':ignore').
> 
> In this example, a GUI is built with a unique identifier `custom3`, adding a `PRESS ME` button that is bound to the `OKAY` section (which will be called on any event within the button). Finally, the window is displayed `Show` with a dimension of `200x100` pixels.
> ```javascript
> :OKAY
>    Message "Ole" With "Hello world!"
> :MAIN
>    Gui "custom3: Destroy"
>    Gui "custom3: Add, Button, gOKAY, PRESS ME"
>    Gui "custom3: Show, w200 h100, My window"
>    Wait_window "My window"
>
> ```
> <p align="center"> <img width="300" height="183" src="https://raw.githubusercontent.com/BlassGO/DinoCode_Doc/main/images/gui_en.jpg"></p>
> 
> In this example, a GUI is built with a unique identifier `MyGUI`, where an EditBox (which will serve as data input) is added with a Global Variable `CONTENT`, and a `SUBMIT` button that is bound to the section `RECEIVE` (which will request the GUI to update any defined Variables). Finally, the window is displayed `Show` with a dimension of `200x100` pixels.
> ```javascript
> :RECEIVE
>    Gui "MyGUI: Submit, NoHide"
>    Message "Received" With CONTENT
> :MAIN
>    Gui "MyGUI: Destroy"
>    Gui "MyGUI: Add, Edit, vCONTENT"
>    Gui "MyGUI: Add, Button, gRECEIVE, SUBMIT"
>    Gui "MyGUI: Show, w200 h100, My window"
>    Wait_window "My window"
>
> ```
> <p align="center"> <img width="300" height="183" src="https://raw.githubusercontent.com/BlassGO/DinoCode_Doc/main/images/gui2_en.jpg"></p>


## Modulation
> DinoCode allows the importation of external libraries, these libraries can define new functions or variables.
> ```javascript
>  Import Extension.config "Folder\Extension 2.config"
> ```


## JavaScript/VBS
   DinoCode supports hosting of external languages ​​belonging to Active Scripting (ActiveX), through specific blocks that share values ​​with each other.

   > * **Supported:**
   >    * JavaScript (JS)
   >    * VBScript (VBS)
   >
   > * **Features:**
   >     * The blocks have access to the Variables/Arrays defined in DinoCode.
   >     * The new Variables that are defined inside the block will be shared with DinoCode.
   >     * The DATA of the shared Variables/Arrays will be kept in sync.
   >
   > * **REMARKS:**
   >    * The new Variables/Arrays to be shared with DinoCode must NOT be formally declared (`let, dim, ..`), otherwise they will work locally within the block and their value will disappear.
   >    * Currently only JavaScript can share new Arrays, VBScript only Variables, but Arrays defined within DinoCode can be shared with any block.


   ```javascript
   :main
        Set HUH With "From DinoCode"
        Set NUMBERS With 1 2 "Show me" 4

        Use JavaScript
            java_array = [1, 2, 3, 4]
            shell = new ActiveXObject("WScript.Shell")
            shell.Popup("JavaS---> " + HUH)              //Shows "From DinoCode"
            HUH = "From JavaScript"                      //Update HUH
        Message "Updated" With HUH                       #Shows "From JavaScript"
        Message "New" With java_array[2]                 #Arrays in JS start from position 0, so it will show "3"
    
        Use VBScript
            RESULT=MsgBox (NUMBERS(3),48, "Huh")         'Show "Show me"
            HUH="From VBScript"
        Message "Received" With " %HUH% \n AND 'RESULT' ended with %RESULT%"
   
   ``` 

   ## Experimental
> Experimental features are implementations that have not been approved as a standard for DinoCode, this implies possible BUGs or unexpected results. It is recommended not to abuse their use, since they could be improved or removed in the future.

  * **Block stacking in a single line**
    > By default, DinoCode uses a line-to-line interpretation. The `;` delimiter allows several lines to be stacked on a single line, however, it does not function as a line break, instead alerts the interpreter that from that point on, each segment must be resolved into different threads, always respecting the synchrony of each segment, but NOT necessarily that of its Expressions.
    > 
    > **->** Conditionals, loops, and in general, blocks that use indentation are not supported.
    >
    > Its use is ideal, as long as the Percentage Expressions `% %` existing in each Segment, DO NOT refer to `DATA` defined within the stack, since these are resolved long before the processing of each Segment
    >
    >
    > The following will NOT work:
    > ```javascript
    > :MAIN
    >    Print $(Set X With 66; Return "%X%")
    > ```
    >
    > Functional example:
     > ```javascript
    > :MAIN
    >    Print $(Set X With 66; Return X)
    >    Print $(Set Y With 10; Set Z With 2.5; Return "Result: " (X+Y)*Z)
    > ```

  * **Dynamic call**
    > `% %` Percentage Expressions can be used within the name of a Function or Connector (during its call), allowing the use of dynamic or partial names.
    > 
    > ```javascript
    > :MAIN
    >    Set huh With "Print"
    >    %huh% "Hello world!"
    > ```
