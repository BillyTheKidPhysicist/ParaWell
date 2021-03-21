# readme
ParaWell is a class that solves 4 problems I saw frequently when using multiprocessing. 
1. I ended up rewriting the same code over and over
2. Alot of the code was the same every time 
3. When using asynchronous methods it was annoying that I had to reorganize my results if I need to maintain the same order
4. Many methods run enormously faster on unix/linux system when one takes advantage of global funcions, but they are ugly to have in my opinion
 
ParaWell solves the above issues by being a wrapper around the Pathos multiprocessing library. It stores functions, when on a unix/linux system, in a global variable so that pickling is avoided when possible (the function is not modified and can be forked therefore without pickling).
Consider the approaches below. Both take a function, func, and asychronously apply it to a list of arguments, args, then collect the results in a list resultsList. The first method doesn't even sort the results, wich would require some method of labeling the results to match with arguments, or use global methods to improve performance. ParaWell returns the arguments paired with the results, in two lines!
'
jobs=[]
resultsList=[]
for arg in argsList:
    jobs.append(pathos.pools.ProcessPool.apipe(func,arg))
for job in jobs:
    resultsList.append(job.get())
'
vs
'
helper=ParaWell()
resultsAndArgsList=helper.parallel_Problem(func,argsList)
'

#### future improvements
1. Add examples notebook with performance comparisons.
2. Add ability to return a list of only the results, not paired with the arguments.
3. Add feature to disable global function usage
4. Add setup.py