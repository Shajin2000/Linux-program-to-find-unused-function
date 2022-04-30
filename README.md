# Linux-program-to-find-unused-function
We will first instrument the code with Coco’s Coveragescanner tool. The result is an instrumented executable, and also a .csmes file which contains a database of code lines and their source locations.

$ csgcc -o prog example.c
Next, we run the instrumented program with specific arguments to cause it to execute a code path. This will create an execution report (.csexe file) which contains information about which code was executed. This file will be imported into the generated (.csmes file). As it turns out, with these arguments, the functions B and D are not hit by the test case.

$ ./prog TRUE FALSE TRUE FALSE
We can import the .csexe file into the .csmes file with the following command.

$ cmcsexeimport -m prog.csmes -e prog.csexe -t execution
The final step is to generate the report for the unused functions. Hereby we use the option –function-coverage to specify the coverage level. ‘–format-unexecuted=’ prints the unused functions into the file result.txt by first writing the file( ‘%f‘)  and then then line (‘%l‘).

$ cmreport --function-coverage -m prog.csmes --format-unexecuted='%f %l' -t result.txt
Listing Unused Functions
The result.txt file contains now:

.../example.c 10
.../example.c 22
If we want to analyze more runs of the program, we can just run the ./prog command with different arguments. The executions will result in additional data concatenated to the .csexe file.
