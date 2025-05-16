---
{"dg-publish":true,"permalink":"/euclid-math-contest/geometry/"}
---


```tikz
```


1. ABCD is a square with sides of length 20. Each vertex is connected to the midpoint of the opposite sides of the square. That is B is connected to $M$ the midpoint of side AD, A is connected to N, the midpoint of side DC, and so on. These four line segments intersect to form a smaller square inside ABCD. 
	a. Find the length of AN 
	b. using the letters on the diagram, give three triangles that are similar to the triangle AND. Prove your answer.
	c. Find the length of line segments $AP,PR,RN,DR$ and MP. 
	d. Find the area of the small square. Could you have found this area without calculating any of the segments in part c)?
```tikz
\usepackage{pgfplots} \pgfplotsset{compat=1.16} 
\begin{document} 
\begin{tikzpicture} 

\draw (0,0) coordinate (A) -- (4,0) coordinate (B) -- (4,4) coordinate (C) -- (0,4) coordinate (D) -- cycle; \coordinate (M) at (0,2); \coordinate (N) at (2,0); \coordinate (K) at (2,4); \coordinate (O) at (4,2); \draw (A) -- (N); \draw (B) -- (K); \draw (C) -- (K); \draw (D) -- (N);  \draw (M) -- (C); \draw (A) -- (O);\coordinate (P) at (intersection of A--N and B--M); \coordinate (R) at (intersection of A--N and D--O);\node[above left] at (A) {A}; \node[above right] at (B) {B}; \node[below right] at (C) {C}; \node[below left] at (D) {D}; \node[left] at (M) {M}; \node[below] at (N) {N}; \node[above] at (K) {K}; \node[right] at (O) {O}; 

\end{tikzpicture}
\end{document}
```

2. In the diagram, trapezoid ABCD is drawn with AB parallel to DC, angle $A=$ angle D $=90,$ $AB=18,$ $AD=24$ and $DC=9.$ denote O by the intersection of the diagonals BD and AC. Through O draw MN parallel to AB, with M on AD and N on BC.
	a) Find the length of DB. 
	b) Find a triangle similar to ADOC. Find the length of DO and OB. 
	c) Find a triangle similar to ADOM.Find the length of DM,MA and MO. 
	d) Prove that $ON=OM.$ e) Find the length of segments CN and NB.


```tikz

\usepackage{pgfplots} \pgfplotsset{compat=1.16} 
\begin{document} 
\begin{tikzpicture}[scale=0.3]
 % Define the points A, B, C, D
    \coordinate (A) at (0,0);    % Point A
    \coordinate (B) at (18,0);   % Point B (AB parallel to DC)
    \coordinate (D) at (0,24);   % Point D
    \coordinate (C) at (9,24);   % Point C (DC parallel to AB)
    
    % Draw the trapezoid ABCD
    \draw[thick] (A) -- (B);  % Line AB
    \draw[thick] (B) -- (C);  % Line BC
    \draw[thick] (C) -- (D);  % Line CD
    \draw[thick] (D) -- (A);  % Line DA

    % Diagonal lines AC and BD
    \draw[thick] (A) -- (C);  % Diagonal AC
    \draw[thick] (B) -- (D);  % Diagonal BD

    % Intersection point O of diagonals AC and BD
    \coordinate (O) at (intersection of A--C and B--D);

    % Draw the line MN parallel to AB through point O
    \coordinate (M) at (0,16);  % Point M on AD
    \coordinate (N) at (12,16);  % Point N on BC
    \draw[thick] (M) -- (N); % Line MN

    % Optional labels
    \node[below] at (A) {A};
    \node[below] at (B) {B};
    \node[above] at (C) {C};
    \node[above] at (D) {D};
    \node[below left] at (O) {O};
    \node[above left] at (M) {M};
    \node[above right] at (N) {N};



\end{tikzpicture}
\end{document}

```


3. The circles with centers A and B are tangent to each other and have radii of 2 and 3, respectively. The line CT is tangent to the first circle at M and to the second circle at N. If the point A, B and C are situated on the same straight line.
	Find the length of segment AC.

```tikz

\usepackage{pgfplots} \pgfplotsset{compat=1.16} 
\begin{document} 
\begin{tikzpicture}[scale=0.5]
 \def\radiusA{2}  % Radius of the circle at A
    \def\radiusB{3}  % Radius of the circle at B
    
    % Define the points A, B, and C (on the same line)
    \coordinate (A) at (0,0);  % Center of the first circle
    \coordinate (B) at (\radiusA + \radiusB,0);  % Center of the second circle (tangent to A)
    \coordinate (C) at (-\radiusA - \radiusB - 4,0); % Point C on the same line
    \coordinate (T) at (\radiusA + \radiusB + 4,4); 
    % Draw the circles with centers A and B
    \draw[thick] (A) circle (\radiusA);  % Circle at A
    \draw[thick] (B) circle (\radiusB);  % Circle at B


    % Draw the tangent line CT above the circles, tangent to both circles
    \draw[thick] (C) -- (T);
    
   \draw[thick] (C) -- ({A}) -- ({B});
    % Define the tangent points M and N on circles A and B
    \coordinate (M) at (-0.3,2); % Tangent point M on circle A
    \coordinate (N) at (4.5,3); % Tangent point N on circle B
    

    \draw[thick] (A) -- (M);  % Line connecting the tangent points
     \draw[thick] (B) -- (N);  % Line connecting the tangent points
    
    % Optional labels for points
    \node[below left] at (A) {A};
    \node[below right] at (B) {B};
    \node[left] at (C) {C};
     \node[right] at (T) {T};
    \node[above left] at (M) {M};
    \node[above right] at (N) {N};


\end{tikzpicture}
\end{document}

```
