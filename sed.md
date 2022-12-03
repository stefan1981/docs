# sed stream editor
sed is a mighty tool for editing streams. sed stands for (s)tream (ed)itor.  
Lets show some examples to get familiar with sed.  
  
Lets use the following file (my.txt) for our next examples:  
```
Max Müller 1985
Jana Keller 1964
Paul Fröhlich 1993
Peter Keller 1985
Pia Stein 1972
```
  
First we want to delete all lines containing the word „Keller“  
  
```
sed '/Keller/d' my.txt
```
The sed expression is given as a string and wrapped between slashes. NOtice the d at the end of the expression. It stands for delete. At the end of the line you give the file to process. The output is like this:  
```
Max Müller 1985
Paul Fröhlich 1993
Pia Stein 1972
```
  
In the next example we do several things. We remove lines that contain the Word ‚1985‘, we replace the first occurence of lowercase ‚e‘ with an uppercase ‚E‘ and we replace all lowercase ‚i‘ with an uppercase ‚I‘:  
```
sed -e '/1985/d' -e 's/e/E/' -e 's/i/I/g' my.txt
```
this will produce the following output:  
```
Jana KEller 1964
Paul FröhlIch 1993
PIa StEIn 1972
```
You can use several expressions with the parameter -e .  
The second and third expressions are substitutions.  
Substitutions have the form ’s/Old/New/‘ starting with an ’s‘ (substitute) followed by three slashed that wraps the Old (What has to be replaced) and the New (The replacement). A trailing ‚g‘ (global) applies the substitution on all occurences in the line instead of just the first one  
  
Match every occurence of a word:  
```
sed -e ’s/\w*/“&“;/g‘ test.txt
```
  
Match the n-th occurence of a word (in this case the 3-th):  
```
sed -e ’s/\w+/(&)/3′ test.txt
```
