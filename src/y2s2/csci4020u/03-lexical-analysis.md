# Lexical Analysis

+ **Alphabet** - A _finite_ collection of symbols called **Tokens** denoted
  with $\Sigma$.
+ **Token** - An individual symbol. A unit of source code that consists of a
  **Lexeme** and a **Token Type**.
+ **Token Type** - A category of **Tokens**.
+ **Strings** - Sequences of symbols from the alphabet $\Sigma$.
  - We concern ourselves with _finite_ strings.
  - The set of all finite strings is denoted by $\Sigma^*$
  - A snippet of code is considered a string over some alphabet of lexemes.
+ **Lexeme** - The graphical/visual representation of a **Token**. A substring
  in the source code.
+ **Language** - A subset of strings in $\Sigma^*$:
  $$ L \sube \Sigma^* $$
+ If some string $s \in L$, we say that $s$ is valid with respect to the
  language $L$.
+ Strings can have different encodings but still represent the same thing.
  - e.g. ASCII chars, binary, token set, etc. encodings.
+ Conversion between different encodings (and thus alphabets) is possible.

## Tokenization

+ Taking the encoded string of a language and converting it to a sequence of
  tokens.
+ Tokens can belong to _categories_ also called _token types_.
+ These token types can be defined to best describe strings of a language.

The goal is to convert the source code of a language ($\Sigma^*_{\text{chars}}$) into 
a token stream ($\Sigma^*_{\textit{lang}}$). The exact set of tokens used is specific
to the language.

For example, Java source written with ASCII chars gets tokenized into a stream
of tokens:
```java
  // Some tokens in this example are: 'class', '{', '}'
  // We could define an ID token type that represent all possible Java
  // identifiers.
  class MyClass {
    int myId;
  }
```
+ We encode $\Sigma_{\textit{lang}}$ as a pair of Token Type and Lexeme to
  ensure we can reconstruct the original string.
+ If we denote all possible tokens as $\textbf{Tok}$, the lexical analyzer, or
  lexer, is a function:
  $$ \text{lex} : \Sigma^*_{\text{chars}} \rarr \textbf{Tok}^* $$

## General Design of a Lexical Analyzer

+ Token types can be defined with reqular expresion syntax.
+ Strings are scanned left to right.
  - Track the current position.
  - Try to match patterns at the current position. Multiple may apply.
  - Decide on how to solve ambiguity:
    * e.g. Longest match wins.
    * First rule defined wins.

