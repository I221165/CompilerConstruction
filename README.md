# CompilerConstruction


This compiler implementation focuses on demonstrating the foundational concepts of compiler design, particularly the front-end components. The project implements Thompson's construction algorithm to build NFAs from regular expressions, converts these NFAs to DFAs using the subset construction algorithm, and uses these DFAs for efficient lexical analysis.

## Components

The project is organized into several major components:

### 1. NFA Construction (`State` and `NFA` classes)
The NFA construction component provides the foundation for representing regular expressions as finite automata:

- **State Class**: Represents states in an automaton with:
  - Unique identifier for each state
  - Boolean flag indicating whether it's a final (accepting) state
  - Map of transitions, where keys are characters and values are lists of target states
  - Support for epsilon transitions using null as the character key

- **NFA Class**: Simple container for an automaton with:
  - Start state for beginning execution
  - End state to mark acceptance
  - Used as building blocks that can be combined to form more complex automata

This component is designed to handle the non-deterministic nature of regex matching, where multiple possible paths through the automaton can exist simultaneously.

### 2. Regex Parser (`RegexParser` class)
The Regex Parser implements Thompson's construction algorithm to convert regular expressions to NFAs:

- **Recursive Descent Parser**: Implements a grammar for regular expressions with methods for each non-terminal:
  - `expression()`: Handles alternation with the `|` operator
  - `term()`: Manages concatenation of factors
  - `factor()`: Processes repetition operators (`*` and `+`)
  - `base()`: Handles literals, parenthesized expressions, character classes, and escapes

- **Character Class Support**: Processes both standard and negated character classes:
  - `[a-z]` matches any character in the specified range
  - `[^abc]` matches any character not in the specified set

- **Construction Operations**: Implements fundamental operations for building NFAs:
  - `literal()`: Creates a simple NFA for a single character
  - `concatenate()`: Joins two NFAs sequentially
  - `union()`: Creates an NFA that matches either of two patterns
  - `kleeneClosure()`: Creates an NFA for zero or more repetitions (*)
  - `cloneNFA()`: Creates independent copies of NFAs for operations like the plus operator

This component translates the textual regex patterns into executable automata representations, forming the basis for token recognition.

### 3. NFA to DFA Conversion (`NfaToDfaConverter` class)
This component implements the subset construction algorithm to convert NFAs to more efficient DFAs:

- **DFA State**: Represents a set of NFA states, with:
  - Collection of NFA states that it encompasses
  - Final status determined by containing any final NFA state
  - Deterministic transitions (one target state per input symbol)

- **Epsilon Closure**: Computes all states reachable through epsilon transitions from a set of states:
  - Uses depth-first traversal to find all reachable states
  - Essential for handling non-determinism in the original NFA

- **Move Operation**: Computes the set of states reachable by a specific input symbol:
  - Applied before epsilon closure to handle all possible transitions

- **Conversion Process**:
  - Starts with the epsilon closure of the NFA start state
  - For each new DFA state and each possible input symbol, computes transitions
  - Creates new DFA states as needed
  - Continues until all reachable DFA states are processed

The conversion significantly improves runtime performance by eliminating non-determinism, enabling O(n) matching time for input strings of length n.

### 4. Token Definitions and Lexical Analyzer (`Lexer` class)
The lexical analyzer component converts source text into a stream of tokens:

- **Token Class**: Represents recognized lexical units with:
  - Token type (keyword, identifier, operator, etc.)
  - Lexeme (the actual text matched)
  - Line number for error reporting

- **TokenDefinition Class**: Links token types to their regex patterns:
  - Stores the token type and regex pattern
  - Compiles patterns into NFAs and then DFAs
  - Provides a matching mechanism for token recognition

- **Lexer Class**: Processes input text to produce tokens:
  - Keeps track of position and line number
  - At each position, finds the longest matching token
  - Handles whitespace and newlines
  - Reports unrecognized characters as errors
  - Builds a complete list of tokens for the parser

- **Token Types**: Supports a variety of token types including:
  - Keywords (`int`, `float`, `char`, `bool`)
  - Comments (both single-line and multi-line)
  - Character literals with escape sequences
  - Floating-point and integer literals
  - Boolean literals (`true`, `false`)
  - Operators and punctuation
  - Identifiers

This component forms the first phase of compilation, breaking the source code into meaningful tokens for syntactic analysis.

### 5. Symbol Table and Scope Management
This component tracks variables and their properties across different scopes:

- **SymbolTableEntry Class**: Stores information about declared variables:
  - Name (identifier)
  - Type (int, float, bool, etc.)
  - Scope (global or local)
  - Memory address (for code generation)

- **SymbolTable Class**: Maps identifiers to their properties:
  - Adds new entries for declarations
  - Provides lookup functionality
  - Supports displaying all entries for debugging

- **ScopeManager Class**: Handles nested lexical scopes:
  - Maintains a stack of symbol tables
  - Global scope at the bottom, with local scopes pushed and popped
  - Creates new scopes when entering blocks (`{`)
  - Removes scopes when exiting blocks (`}`)
  - Provides access to the current scope for declarations and lookups
  - Simulates memory allocation for variables

This component supports basic semantic analysis, tracking variable declarations and providing information for type checking and code generation.

### 6. Error Handling (`ErrorHandler` class)
The error handling component provides mechanisms for reporting and tracking compilation errors:

- **Error Collection**: Maintains a list of all errors encountered:
  - Stores error messages with line numbers
  - Allows bulk reporting at the end of compilation

- **Error Reporting**: Provides methods to report errors:
  - `reportError(message, line)`: Records an error with line information
  - Outputs errors to stderr for immediate feedback

- **Error Status**: Allows checking if errors occurred:
  - `hasErrors()`: Returns true if any errors were detected
  - Useful for deciding whether to proceed to later compilation phases

- **Error Display**: Supports displaying all collected errors:
  - `displayErrors()`: Outputs all recorded errors
  - Useful for final error reporting

This component ensures that compilation errors are properly tracked and reported, improving the debugging experience.

## Regular Expression Engine

The regular expression engine is a central component of this compiler. It supports:

- **Basic Operations**: Concatenation, alternation (`|`), Kleene star (`*`), and plus operator (`+`)
- **Grouping**: Parentheses for grouping expressions
- **Character Classes**: Both standard (`[a-z]`) and negated (`[^abc]`) classes
- **Escape Sequences**: Escaping special characters with backslash

The parser implements a recursive descent approach with the following grammar:

```
expression : term ('|' term)*
term       : factor+
factor     : base ('*' | '+')*
base       : literal | '(' expression ')' | character class | escape sequence
```

### Thompson's Construction

The parser uses Thompson's construction algorithm to build NFAs for regular expressions:

- **Literal**: Creates a simple two-state NFA with a transition labeled by the literal
- **Concatenation**: Joins two NFAs by connecting the end state of the first to the start state of the second
- **Alternation (Union)**: Creates a new start state with epsilon transitions to the start states of both NFAs
- **Kleene Star**: Adds epsilon transitions to allow skipping the NFA or repeating it multiple times
- **Plus Operator**: Implemented as concatenation of the NFA with its Kleene star closure

## Lexical Analyzer

The lexical analyzer uses the regex engine to tokenize source code:

1. Token types are defined with regular expression patterns
2. These patterns are compiled into DFAs
3. The lexer scans input, finding the longest matching token at each position
4. Tokens are annotated with their type, lexeme (actual text), and line number

Supported token types include:
- Keywords (`int`, `float`, `char`, `bool`)
- Comments (both single-line and multi-line)
- Character literals
- Floating-point and integer numbers
- Boolean literals
- Operators and punctuation
- Identifiers

## Symbol Table and Scope Management

The compiler includes a scope-aware symbol table:

- **Symbol Table**: Maps identifiers to their properties (type, scope, memory address)
- **Scope Manager**: Handles nested scopes using a stack of symbol tables
- **Memory Assignment**: Simulates memory allocation for variables

The implementation supports:
- Global and local scopes
- Variable declarations
- Scope entry and exit (via `{` and `}`)

## Error Handling

The error handler collects and reports errors during compilation:
- Reports unrecognized tokens with line numbers
- Maintains a list of all errors encountered
- Provides methods to check for and display errors

## Usage Example

The main class demonstrates the compiler in action with a sample source code snippet:

```java
String sourceCode = "int main()  \n"
    + "   int x = 0;  \n"
    + "   bool y = 0;  \n"
    + "   // multi-line \n comment */  \n"
    + "   {   bool l = true;  \n"
    + "       int a = 0;  \n"
    + "       x = x + y * a - 3 % 2 ^ 2;  \n"
    + "       return 0;  \n"
    + "   }";
```

The compiler:
1. Defines and compiles token patterns
2. Tokenizes the source code
3. Simulates scope management and variable declarations
4. Displays tokens, symbol tables, and any errors

This example demonstrates lexical analysis, scope handling, and basic error reporting in a compact form.
