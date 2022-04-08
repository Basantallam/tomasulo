# This project is a simulation of how the Tomasulo architecture works.

## Steps to Develop: 

1) Studied the Tomasulo Architecture and we understood how the algorithm works.
2) Divided the algorithm into 4 main chunks: Issue, Start Execution, End Execution and Write Result, and we assigned each of the chunks to one or two members.
3) Chose the main data structures we’ll be using.
4) Created a front end for each of our arrays, and we ensured that any update that occurs to the arrays is reflected on our front end using React hooks and useState.
5) Wrote the “skeleton” of our code as function headers, we specified the parameters of each function and its return type. We wrote the intended functionality of each function as a comment but we did not implement it at first. This helped us divide our big problem into smaller subproblems that are easier to solve. Moreover, it helped us see the bigger picture and organise our thoughts. This is known as “Stepwise Refinement”
6) Each team member was then assigned some of the functions, and we implemented them and tested them individually (unit testing).
7) After testing and debugging all the functions individually, we started doing “integration testing”, where we tested the interaction of our functions together and ensured smooth execution.
 


## Steps to Run:

Terminal: 
```
cd src
npm install 
npm start
```
Wait till it runs on localhost.

## Steps to Test:

1) Enter instructions one by one and click add instruction after each instruction you write (you can alternatively add all instructions in the textbox at once and click the button when you’re done.)
2) Specify the latencies of each operation (ADD/SUB, MUL/DIV, LD & STR)
3) Scroll down, and click execute program.  You’ll be directed to the next page.
4) You’ll have the reservation stations, state of each instruction, and the registers displayed at cycle number 1. 
5) Click the “next” button to go to the next cycle.


## Approach:
The main data structure we used is arrays of json objects and these arrays are:
“main” has all the instructions
“add” is for the ADD/SUB reservation station
“mul” is for the MUL/DIV reservation station
“load” is for the LD reservation station
“store” is for the STR reservation station
“registers” is the register file
“memory” is the memory

## We have 4 main functions: issue, startExecution, endExecution and writeResult.

### In the "issue" function we do the following:
- We have a pointer initially pointing at the first instruction.
- We check if there is an available spot in the reservation station the instruction needs.
- If there is an available spot, we set the Issue time of this instruction to the current cycle, and we put the instruction in the available spot, mark it as busy.
- We also check the registers that the instruction needs, and we update Q or V in our spot depending on whether the register value is ready or not.
- Lastly, we check the register the instruction will write back to after it’s done, and we put the tag of the reservation station spot in the register’s Q section indicating that the value of this register is pending.
- If the instruction was issued, we increment the pointer.

### In the "start execution" function, we do the following:
- We loop on all the stations and check their Q values.
- If all Q values of a row of a station are empty, this means this instruction can start execution immediately (if it hasn’t already). 
- We set the start time of this instruction to the current cycle
- We also calculate the result of execution and store it temporarily in a variable called temp that is not displayed in the front end. 

### In the "end execution" function, we do the following:
- We loop on all reservation stations and check if the current cycle = instruction latency + start time.
- If this is the case, we update the instructions end time to the current cycle.


### In the "write result" function, we do the following:
- Find the first  instruction that finished execution but did not write back yet.
- Set its write back time to the current cycle.
- Update the registers and reservation stations that are dependent on this result (registers and reservation stations carrying my tag.)
- Empty the spot of the instruction in the reservation station and set the busy to 0.

## Code Structure:

- Programming Language: javascript.
- Framework: React
# We have ave 4 js pages :
- index: that is called on start then it calls
- App.js: which declares different routes: one for taking input(Instructions and latency and one for running Tomasulo)
- main.js: it is the main page opened first by the user to take instructions written by the user one by one Or the whole set of instructions at once. It takes all latencies as well. Then when the user inputs all instructions he presses start execution to begin the Tomasulo algorithm
- Anim.js: this page displays the Tomasulo Algorithm cycle by cycle when the user presses next the page update its content with that of the next cycle 

In the Anim.js, we have the doCycle function, which is a caller function that calls the rest of our functions and initiates the steps executed in every cycle
```
    function doCycle() {
        cycle=cycleFront+1;
        setCycleFront(cycleFront + 1);
        startExecution();
        issue();
        writeResult();
        endExecution();
    }
    ```
    ## Test Cases:
```
LD ,F1,3		

LD ,F2,6	

LD,F3,1		

ADD ,F1,F2,F2	

SUB ,F1,F2,F3			

STR,F1,2
```
### Example Output:

<img width="490" alt="tomasulo" src="https://user-images.githubusercontent.com/30272808/162545638-3dfa1bee-c25b-4fed-84f9-da714b83f076.png">


