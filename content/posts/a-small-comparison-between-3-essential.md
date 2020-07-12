---
title: 'A small comparison between 3 essential algorithms in Kotlin'
date: 2019-09-03T10:47:00.000+02:00
draft: false
aliases: [ "/2019/09/a-small-comparison-between-3-essential.html" ]
tags : [Algorithms, Insertion Sort, Math, Kotlin, Big O notation, Merge Sort, Quick Sort]
author: "Stavro Xhardha"
---

  

[![](https://1.bp.blogspot.com/-yNEhnV7kQ0k/XW4pDHoZewI/AAAAAAAAPHY/BIDZeldy9lECb0UiO92OU6PbLj1WX20fQCLcBGAs/s1600/antoine-dautry-05A-kdOH6Hw-unsplash.jpg)](https://1.bp.blogspot.com/-yNEhnV7kQ0k/XW4pDHoZewI/AAAAAAAAPHY/BIDZeldy9lECb0UiO92OU6PbLj1WX20fQCLcBGAs/s1600/antoine-dautry-05A-kdOH6Hw-unsplash.jpg)

  
There is no doubt that sorting is still one of the most essential and important part in computer science. Everyone who knows a little about algorithms has already proven today's data to  be true, but I would give a hand to everyone who hasn't tried the results by themselves yet.  
  
There are different sorting algorithms, but there are many factors to differentiate them from one another. And the focus today will be in the execution time, which comes as a result of each algorithms time complexity.  
  
Let's jump to the algorithms first, starting with **Insertion Sort**:  
  
This is the code for **Merge Sort**:  
  
  
And this is the code for Quick Sort:  
  
The only thing that will be changing here is the size of the elements that Utils.someArrayOfInts() will return.  
  
In the first case, let's try our algorithms  with just an array of 7 integers:  
  

[![](https://1.bp.blogspot.com/-mD5v2EWV510/XW4gijSa0NI/AAAAAAAAPGs/_cqipJvU3koAbDQVKQyyaWo0-VeK9RJCQCLcBGAs/s1600/Chartfor7.png)](https://1.bp.blogspot.com/-mD5v2EWV510/XW4gijSa0NI/AAAAAAAAPGs/_cqipJvU3koAbDQVKQyyaWo0-VeK9RJCQCLcBGAs/s1600/Chartfor7.png)

  

  
Insertion sort is pretty quick comparing to other algorithms in this case. Now let's run the same algorithms, with one thousand (1000) integers. Check out my result:  
  

[![](https://1.bp.blogspot.com/-vgpsnNOLasc/XW4iM_shK8I/AAAAAAAAPHA/fmUmDSDA8Og06HfXE8TH7smWEpA5AMkzQCLcBGAs/s1600/Chartfor1k.png)](https://1.bp.blogspot.com/-vgpsnNOLasc/XW4iM_shK8I/AAAAAAAAPHA/fmUmDSDA8Og06HfXE8TH7smWEpA5AMkzQCLcBGAs/s1600/Chartfor1k.png)

  

 When having to sort 1000 items, looks like merge sort wins. And this is because insertions sort time complexity is O(n^2) (n square) while the others have an O(log(n)) time complexity (Quick sort has O(n^2) in worst case), which respectively means: For insertion sort, if the number of input items increases, so does the execution time, while the other 2 algorithms execute in lower time while the input increases. Mathematically the logn graph is quite the opposite of exponential graph which is the exact case illustrated here.  
  
And now let's check the same algorithms, with 1 million elements to sort:  
  

[![](https://1.bp.blogspot.com/-cmjXvtvSnqk/XW4lfnAAoXI/AAAAAAAAPHM/tbggyfij2ik7y1KQVba86Dz6qUn95Mq0QCLcBGAs/s1600/Chartfor1m.png)](https://1.bp.blogspot.com/-cmjXvtvSnqk/XW4lfnAAoXI/AAAAAAAAPHM/tbggyfij2ik7y1KQVba86Dz6qUn95Mq0QCLcBGAs/s1600/Chartfor1m.png)

  
**Conclusion**  
  
Looks like you should avoid using Insertion sort for a large input array and stick to Merge Sort or Quick Sort. Even though Merge Sort or Quick Sort run recursively (and recursion does slow down the program), the best algorithm for less than 30 inputs should be Insertion sort.  
  
  
**Some things to note:**  
  
_The array elements are not guaranteed that will be the same for each time an algorithm executes. They are generated randomly and placed in a position by a for loop. So we cannot tell exactly in this case which from Merge sort or Quick sort is better. We are focusing only in execution time, but the way the elements are at first state in the array, does matter also._  
_Another thing to keep in mind is to forget using sorting algorithms to real life projects. Keep in mind that Java/Kotlin has already implemented the best solution for sorting arrays which is called DualPivotQuicksort that is an improvement of the basic Quick sort above. _  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } I also have more algorithms in Kotlin. You can check them [here](https://github.com/coroutineDispatcher/algorithms_course/tree/master/src/kotlin_version/arrays).