# Inspecting and Manipulating Text Data with Unix Tools - Part 2

* Do not remove this line (it will not be displayed)
{:toc}

## Aho, Weinberger,Kernighan = `awk` 

Awk is a scripting language used for manipulating data and generating reports. The awk command programming language requires no compiling and allows the user to use variables, numeric functions, string functions, and logical operators. 

Awk is a utility that enables a programmer to write tiny but effective programs in the form of statements that define text patterns that are to be searched for in each line of a document and the action that is to be taken when a match is found within a line. Awk is mostly used for pattern scanning and processing. It searches one or more files to see if they contain lines that matches with the specified patterns and then perform the associated actions. 

WHAT CAN WE DO WITH AWK? 

1. AWK Operations: 
   - Scans a file line by line 
   -  Splits each input line into fields 
   -  Compares input line/fields to pattern 
   -  Performs action(s) on matched lines 

2. Useful For: 
   -  Transform data files 
   -  Produce formatted reports 

3. Programming Constructs: 
   - Format output lines 
   - Arithmetic and string operations 
   - Conditionals and loops 

**Syntax**:

```bash
awk options 'selection_criteria {action}' input-file >  output-file
```