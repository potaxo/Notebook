##### `Tldr:  programs that operate on programs
---
## Buliding systems
![[Pasted image 20250924215257.png|300]]

> You define a number of _dependencies_, a number of _targets_, and _rules_ for going from one to the other. [missing semester](https://missing.csail.mit.edu/2020/metaprogramming/)

![[Pasted image 20250924193238.png|400]]
- `.tex`: Writing a paper may use LaTeX, a difficult programming language to construct papers, and the source code file is .tex.
> %` :  `%` will match the same string on the left and on the right, For example, if the target `plot-foo.png` is requested, `make` will look for the dependencies `foo.dat` and `plot.py`.
- `$` will match any character.
-  `$@` will be complied as *target*, in this picture is plot-data.png

![[Pasted image 20250924205435.png|400]]
- **plot.py** use matplotlib coordinating with data.bat, generating a png named by the args after -o
- `.dat` means data structure files.
``` shell
$ make
./plot.py -i data.dat -o plot-data.png
pdflatex paper.tex
... lots of output ...
```
- You will find that even if in make file(the first picture), we write ... first, but [[Make]] will figure out how to coordinate them.

---
## Make
- Make will always do the minimal amount of work needed, and in each make, it will remake if it checking dependency changed.
#### Pros:
- If you're doing experiment, you don't need to write the commands again and again
- Good for test
---
## Dependency


---
Relevant Link: [[CLI]] 
Date: 2025-09-24 
Time: 19:28