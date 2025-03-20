# CompilerConstruction


Welcome to ZZ Compiler Frontend! This is a simple but powerful compiler that helps computers understand programming languages. Think of it as a translator that converts human-readable code into something computers can understand.

## 🎯 What Does This Compiler Do?

Imagine you're writing a program like this:
```java
int main() {
    int x = 5;
    return x;
}
```

Our compiler helps the computer understand this code by:
1. Breaking it into small pieces (tokens)
2. Keeping track of variables (like `x`)
3. Making sure everything is written correctly
4. Finding any mistakes in the code

## 🌟 Main Components

### 1. The Lexer (Code Reader) 📖
The lexer is like a smart reader that:
- Reads your code line by line
- Identifies different parts of your code:
  - Keywords (like `int`, `return`)
  - Numbers (like `5`)
  - Variable names (like `x`)
  - Special symbols (like `{`, `}`, `=`)
- Tells you if it finds anything it doesn't understand

### 2. The Symbol Table (Variable Tracker) 📝
The symbol table is like a notebook that:
- Remembers all the variables you create
- Keeps track of what type they are (number, text, etc.)
- Makes sure you don't use the same name twice
- Helps find variables when you need them

### 3. The Error Handler (Problem Finder) 🔍
The error handler is like a helpful assistant that:
- Finds mistakes in your code
- Tells you exactly where the problem is
- Explains what went wrong
- Helps you fix the errors

## 🛠️ How It Works

### Step 1: Reading Your Code
```java
int x = 5;
```
The lexer breaks this into pieces:
- `int` (keyword)
- `x` (variable name)
- `=` (operator)
- `5` (number)
- `;` (punctuation)

### Step 2: Keeping Track of Variables
When it sees `int x = 5;`, it:
1. Creates a new entry in the symbol table
2. Remembers that `x` is a number
3. Gives it a special memory location
4. Makes sure you can use it later

### Step 3: Finding Mistakes
If you write something wrong, like:
```java
int x = "hello";  // Wrong! Can't put text in a number variable
```
The error handler will tell you:
- What's wrong
- Which line it's on
- How to fix it

## 📋 How to Use It

1. **Get the Code**
```bash
git clone https://github.com/yourusername/zz.git
cd zz
```

2. **Make It Ready to Run**
```bash
javac src/*.java
```

3. **Run It**
```bash
java -cp src zz.Main
```

## 📝 Example Program

Here's a simple program you can try:
```java
int main() {
    int x = 5;
    int y = 10;
    return x + y;
}
```

When you run this, the compiler will:
1. Read each line
2. Remember the variables `x` and `y`
3. Make sure everything is correct
4. Show you what it found

## 🎓 Learning More

This compiler uses some cool computer science concepts:
- **DFA (Deterministic Finite Automaton)**: A fancy way to read code
- **Symbol Table**: A smart way to remember variables
- **Scope**: A way to organize where variables can be used

