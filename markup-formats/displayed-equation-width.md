# A displayed equation is no wider than a line of text, and a wider one is broken before a relation, an arrow or an operator into aligned lines

A displayed equation fits the measure of the text around it, the width of one line of prose.
The measure of a page set at 10pt is 345pt, the text width of the LaTeX class `article`, and holds about 66 characters, the length that Bringhurst gives as the ideal of the range from 45 to 75 characters.
A wider display overflows the margin of a printed page.
On a screen a wider display shows only part of its width at a time, and the reader scrolls sideways to see the rest.

A display that is too wide is broken into lines, each no wider than the measure.
The break points follow an order of preference.

1. Break before a relation symbol, such as $=$, $\le$, $\sqsubseteq$ or $\iff$, and align the lines at the relation.
2. In a chain of maps, break before an arrow, and align the arrows.
3. Otherwise break before a binary operator, such as $\circ$, $\otimes$ or $+$, and indent the continuation past the relation of its line.
4. Put a type annotation such as $\colon X \to Y$ on a line of its own, aligned with the relation.
5. Stack two equations that stand side by side, one per line, aligned at their relations.

A break inside a pair of delimiters is the last resort, because the reader has to match the delimiters across lines.
Naming a long subterm once in the text and using the name in the display is often better than breaking it.
The breqn manual states the same preference: by default each relation symbol after the first starts a line, and a break at a relation is preferred over a break at a binary operator.

> Rejected: the middle-four interchange of the tensor product of enriched categories on one line of 613pt, almost twice the measure:

$$
\begin{aligned}
m_{W,X,Y,Z}
= \alpha^{-1}_{W,Y,X \otimes Z} \circ \bigl( \mathrm{id}_W \otimes \alpha_{Y,X,Z} \bigr) \circ \bigl( \mathrm{id}_W \otimes (\sigma_{X,Y} \otimes \mathrm{id}_Z) \bigr)
\circ \bigl( \mathrm{id}_W \otimes \alpha^{-1}_{X,Y,Z} \bigr) \circ \alpha_{W,X,Y \otimes Z}
\colon (W \otimes X) \otimes (Y \otimes Z) \to (W \otimes Y) \otimes (X \otimes Z).
\end{aligned}
$$

> Replacement: three aligned lines, the second broken before $\circ$ and the third holding the type,

$$
\begin{aligned}
m_{W,X,Y,Z}
&= \alpha^{-1}_{W,Y,X \otimes Z} \circ \bigl( \mathrm{id}_W \otimes \alpha_{Y,X,Z} \bigr) \circ \bigl( \mathrm{id}_W \otimes (\sigma_{X,Y} \otimes \mathrm{id}_Z) \bigr)
\\
&\qquad \circ \bigl( \mathrm{id}_W \otimes \alpha^{-1}_{X,Y,Z} \bigr) \circ \alpha_{W,X,Y \otimes Z}
\\
&\colon (W \otimes X) \otimes (Y \otimes Z) \to (W \otimes Y) \otimes (X \otimes Z).
\end{aligned}
$$

---

Source: Yujun Huang's request of 2026-10-07; Bringhurst, *The Elements of Typographic Style*, Hartley and Marks, section 2.1.2, for the measure of 45 to 75 characters; the text width of the LaTeX class `article` at 10pt, 345pt, printed by `\the\textwidth`; the breqn manual of Downes, Høgholm and Robertson, shipped with TeX Live as `breqn.pdf`, the options `compact` and `indentstep`, for the order of break points.
