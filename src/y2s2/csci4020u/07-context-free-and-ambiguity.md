# CFGs & Ambiguity

+ **Symbols** - Part of the alphabet.
  - $\Sigma_0$ - **Terminals**, the tokens. Generally lower case throughout notes.
  - $\Sigma_1$ - **Syntactic Variables**, Non-Terminals. Upper case throughout notes.
+ The alphabet is $\Sigma = \Sigma_0 + \Sigma_1$.
+ **Start Symbol** - Generally denoted $S$.
+ **Production Rules** - $A : \alpha$
  - $A \in \Sigma_1$ - The head of the production.
  - $\alpha \in (\Sigma_0 \cup \Sigma_1)$ - The body of the production.
+ **CFG** - A collection of productions and a start symbol.
  - Denoted $G$.
  - $G$ defines a language $L(G) \sube \Sigma^*_0$.
+ **Productions** describe how the head can be replaced by its body.
  - Ex: $aEb \quad \substack{\rarr\\E:aEb}\quad aaEbb$
+ **Sentential Form** - Given a CFG with start symbol $S$, $\alpha$ is a
  sentential form if $S$ produces $\alpha$ ($S \implies \alpha$).
+ **Sentence** - $\alpha$ is a sentence of $S$ if $\alpha$ is a sentential form
  of $S$ and only contains terminal symbols.
  - $L(S)$ is all the sentences of $S$.
+ **CFGs** are strictly more powerful than REs.

## Parse Trees

+ Grammar is syntax.
+ Strings are the programs.
+ Parse trees are the interpretations.
+ Can represent the productions applied to match a string as a tree.
  - Root is the start symbol.
  - Intermediate nodes are Syntactic Variables.
  - Leaf nodes are Terminals.

For the grammar G:
$$
\begin{align*}
  S :&~ aSSb \\
    &| ~ 0 \\
    &| ~ 1
\end{align*}
$$
The derivation for $a01b$ and its tree are as follows:
$$
\begin{align*}
           &S \\
  \implies &aSSb \\
  \implies &a0Sb \\
  \implies &a01b
\end{align*}
$$
```
         S
         |
 +----+--+--+----+
 |    |     |    |
 |    S     S    |
 |    |     |    |
'a'  '0'   '1'  'b'
```

---

# Ambiguity
