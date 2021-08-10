# Greedy algorithm

A greedy algorithm, as the name suggests, always makes the choice that seems to be the best at that moment. This means that it makes a locally-optimal choice in the hope that this choice will lead to a globally-optimal solution.

!!! note
    The main purpose of greedy algorithms is to achieve optimization where an objective is to be **maximized** or **minimized**. It try to find the minimum or maximum of something.

At every step of the algorithm, it just picks the best option (i.e. the maximum or minimum). It doesn't look ahead or look behind to decide the global optimal solution. It just makes a local decision and hopes that it will lead to the globally optimum solution which we are looking for eventually. We hope by the end of the algorithm's execution the collection of locally optimum solutions from every step together achieve the minimum or maximum overall. Sometimes this gives you the right answer to the problem, but a lot of times you can't solve stuff this way.

> You must also prove that if greedy gives you the optimum solution globally. You can disprove greedy algorithms by providing a simple counter example.

!!! info
    When the greedy method doesn’t work, we look forward to something called dynamic programming method.

## Example - finding largest sum

![Finding largest sum](assets/GreedySearchPathExample.gif)

Our goal is to find the largest sum, which is a **maximize problem**. Lets see how greedy algorithm proceeds. We start at the 7^th^ node.

- [Step 1]: Greedy compares 3 and 12. So at this moment or in other words to make locally optimal choice which is to find the maximum value between 3 and 12. The greedy chooses 12.
- [Step 2]: We are at 12^th^ node. Now greedy compares 5 and 6. And chooses 6 which is a maximum value.

At the end, greedy gives you a global solution of largest sum 7 + 12 + 6 = 25

This is wrong.

The actual path which gives you accurate global solution of largest sum is 7 + 3 + 99 = 109

Hence, the greedy approach is successful to make locally optimal choices at each step but it fails to provide the global optimal solution.

## Advantages and disadvantages

- It is quite easy to come up with a greedy algorithm (or even multiple greedy algorithms) for a problem.
- Analyzing the run time for greedy algorithms will generally be much easier than for other techniques (like Divide and conquer). In the Divide and conquer technique, at each level of recursion the number of subproblems increases.
- The difficult part is that for greedy algorithms you have to work much harder to understand correctness issues. Even with the correct algorithm, it is hard to prove why it is correct. Proving that a greedy algorithm is correct is more of an art than a science. It involves a lot of creativity.

## Real life example

Greed is not always good, you may end up with a non-optimal solution (using more fuel than you could have). This is the natural trade-off for being a short-term visionary rather than a long-term visionary. Let me give you an elementary example where it fails. See the following directed network:

![Real life example](assets/greedy-real-life-example.png)

You want to go to the restaurant from home and you have two ways to reach. You want to use fuel as low as you can by taking way which has minimum distance overall. Going by the intuition or greedy approach, you would choose first A because between A and B, A is minimum but then you are stuck with the road of length 99. So you end up moving 100 units rather than a possible 10 if you had selected B first. So greedy algorithm fails in this case.

> If you apply the same intuition at every junction, computer scientists would declare you used Greedy Algorithm to reach the restaurant. Simple, isn’t it? No need to calculate exhaustively all road lengths in advance, just minimum at a given junction.

## How to create a greedy algorithm?

Your algorithm needs to follow this property:

> At that exact moment in time, what is the optimal choice (minimum or maximum) to make?

And that's it. There isn't much to it. Greedy algorithms are generally easier to code than divide & conquer or dynamic programming.

## Job sequencing problem

Given an array of jobs where every job has a deadline and associated profit if the job is finished before the deadline. It is also given that every job takes 1 unit of time, so the minimum possible deadline for any job is 1. How to maximize total profit if only one job can be scheduled at a time.

```
Input: Five Jobs with following deadlines and profits

JobID   Deadline  Profit
  a       2        100
  b       1        19
  c       2        27
  d       1        25
  e       3        15

Output: c, a, e is a sequence of jobs which gives maximum profit.
```

Since we want to know maximum profit to be earned. So it is a maximize problem and let solve it using greedy approach.

### Important points to understand problem better

1. Each job takes 1 unit of time to complete.
2. At a time only one job can be handled.
3. The deadline is numbers of units the job can wait to be finished. This includes completion time of 1 unit too. For example, "a" has 2 units of deadline, so it can wait 1 unit of time and other 1 unit of time is for doing the job.
4. Important point, since we have maximum deadline unit is 3. So think, after 3 units of time elapsed all jobs who have deadline time less than 3 units will be invalid because their deadlines are ended before 3 units of time. Therefore, our time slot array will have size of 3. Which also means the we can do only **3 jobs**.
5. While adding job to slot. We first look for the slot which is equal to the job's deadline. For example, 'a' has deadline 2, so we add it to the slot 2 not slot 1 because it can wait 1 unit of time and other 1 unit is for its completion. We left the first slot empty because if the next job has a deadline 1 then we can add the job in first slot otherwise all jobs with deadline 1 will become invalid, they can no longer be done because their deadline will be over.
6. Since we are adding jobs to the slot randomly. So if slot is found occupied then we try to find the empty slot by looking towards the left of occupied slot where we were trying to add the job. This is because we cannot add the job to the slot which has value greater than job's deadine. For example, job 'a' has deadline 2 so we can add it to the slot 1 and 2 but we cannot add it to the slot 3 because at slot 3 the job's deadline will be over.

```js
let jobs = [
             ['a', 2, 100],
             ['b', 1, 19],
             ['c', 2, 27],
             ['d', 1, 25],
             ['e', 3, 15]
           ];

// 1st: We want maximum profit so sort an array in decreasing order.
jobs.sort((a, b)=>{
  return b[2] - a[2];
});

// 2nd: Size of time slots = Maximum deadline value.
// Initially fill them with null value.
const maxDeadline = 3;
let slots = Array(maxDeadline).fill(null);
let isSlotsFull = 0;

// 3rd: Add jobs in slot.
for(var i=0; i<jobs.length; i++) {
  // We can only add 3 jobs, so when
  // all slots are full then there is
  // no need to check more jobs.
  if(isSlotsFull > maxDeadline) {
    break;
  }

  // 1st slot = 0 index
  // 2nd slot = 1 index
  // nth slot = n-1 index
  let slotIndex = jobs[i][1] - 1;
  // Add job to the slot if not occupied
  // and then check the next job.
  if(slots[slotIndex] === null) {
    slots[slotIndex] = jobs[i][0];
    isSlotsFull++;
    continue;
  }
  // If slot is occupied then
  // look left of occupied slot
  // until 0 index. But exit as soon
  // as, if found the slot before reaching
  // index 0.
  for(var j=slotIndex-1; j===0; j--) {
    if(slots[j] === null) {
      slots[j] = jobs[i][0];
      isSlotsFull++;
      break;
    }
  }
}
console.log(slots); // c, a, e
```
