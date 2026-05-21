There are currently three files in the repository:

1) Pass_1.ipynb
This file extracts all the stated objective from every column and classifies them as either financial or sustainable.
It returns an excel file with all stated objectives for each firm.
Current Model- Claude Sonnet 4-6. 

2) Pass_2.ipynb
Pass 2 takes in the output from Take 1 and removes all the duplicates. It translates all the funds to compare goals
and is allowed to room to make the translation across languages and direct meangings
Current Model- Claude Sonnet 4-6. 



3) Pass_3.ipynb
Pass 3 takes the output from Pass 2 and conducts a sanity check for all of the goals. It can change the goals based on what it finds 
from every column for each firm and takes note if it does or not. Additonally, it returns a level of confidence as well as general findings. 

An example- Pass_3 found that many firms had some sort of sustainable goal but was not being reported as such. Thus, in its general findings, it gave itself
a rating of high confidence but noted that there are most likely goals missing. 
Current Model- Claude Sonnet 4-6. 

The latest outputs can be found here: 
https://drive.google.com/drive/folders/1NIxNe_B3JqpZuQgqfuBQKVYZ9fmSol6p?usp=sharing


Models Used
Prior to May 17th - Claude Sonnet 4.5
May 17th - Present: Claude Sonnet 4.6


Major Issues:
Quotation marks in any column would be confused when the JSON format would try to output the text, as we would get incorrect syntax, this lead to the addition of additional checking and parsing 
(which solved 6/9 errors in our test case) and the inclusion of the final line in our Pass 1 system prompt (IMPORTANT…) (which solved 3/9 errors in our test case). 
Combined this resolved all issues stemming from pass 1. It subsequently solved the two outstanding issues in Pass 3. Only 1 firm states that it has no goals, this will be looked at further.


