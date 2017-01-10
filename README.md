# Practise code from various websites including codechef, hackerearth, hackerrank along with the concepts

## Algorithm analysis
* **Dominance Relation while deciding the asymptotic bound**:

    > We say that f (n) dominates g(n) if &nbsp; &nbsp; ![image for dominance](/images/algo_analysic_dominance_rel.png)
    
    > Let’s see this definition in action. Suppose &nbsp; ![image for fn](/images/algo_analysic_fn.png) &nbsp; and &nbsp; ![image for gn](/images/algo_analysis_gn.png) &nbsp;.  
     Clearly f (n) > g(n) for all n, but it does not dominate
      since  
      ![image for example](/images/algo_analysis_dominance_ex_1.png)  
      This is to be expected because both functions are in the
      class &nbsp; ![image for theta](/images/algo_analysis_theta_rel.png).  
      What about f (n) = &nbsp; ![image for n_square](/images/algo_analysis_n_square.png) &nbsp;  and g(n) = &nbsp; ![image for n cube](/images/algo_analysis_n_cube.png) &nbsp; ? Since  
        ![image for ex2](/images/algo_analysis_dominance_ex_2.png)
        
        
* **Problem for finding the big oh**

    Problem: Is &nbsp; ![big oh example 1](/images/big_oh_ex_1.png) ?  
    Solution:  
    For solving these kinds of problem we should always go by the definition.
    > So, f(n) = O(g(n)) if and only if there exists a constant c such that for all sufficiently large n,  f(n) <= c * g(n).
    
    We can see that &nbsp; ![image for example 2](/images/big_oh_ex_2.png) &nbsp; for any c >= 2.
     >Also, &nbsp; ![image for omega gn](/images/omega_gn.png) &nbsp; iff there exists a constant c > 0 such that for all sufficiently large n f(n) > c * g(n).
     
    This would be satisfied for any 0 < c <= 2.  
    Together the big oh and omega bounding imply &nbsp; ![big on example 1](/images/big_oh_ex_1.png).


## Important data structures concepts:
* **Segment Trees**:
> Segment trees are data structures which are particularly helpful in range query and 
> range update.
> The time complexity of building the tree is O(n). As we need to update the value of 
> each node and their are (2*n -1) nodes in the tree.
> The point which should be noted is that the time complexity of query and update is
> only O(lg n).


* **Lazy Propagation**:
> Lazy propagation refers to a mechanism where we do not update all the nodes in a 
> given range. Rather than that, we lazily update the parent node of a range and mark
> its children for `lazy update`.

> For example: (Please refer to the image given below to understand better.)
> Suppose you have to update all the nodes in range [0:2], (this symbol means, both are included)
> We don't have to update all the nodes in that given range if we use lazy propagation. We only
> have to update node representing (0:2) (which is node = 1 in below image) and mark it immediate children lazy.
> 	* We need an auxilary space for storing the nodes which have been marked lazy.
>   * We need to update the node values only when searching.

![Image for segment tree]
(/images/segment_tree.jpg)

* Code for building a `segment tree`:
```c++
    void build(int node, int start, int end, int count[], int coin[]) {
	if (start == end) {
		count[node] = coin[start];
		return;
	}
	int mid = (start + end) / 2;
	build((node * 2 + 1), start, mid, count, coin);
	build((node * 2 + 2), mid + 1, end, count, coin);
	count[node] = count[node * 2 + 1] + count[node * 2 + 2];
}
```

* Code for `range update`:
```c++
	void updateRange(int node, int start, int end, int l, int r, int val, int count[], int lazy[]) {
		// In this code we are using lazy propagation. For more details about lazy propagation read above.
		if (lazy[node] != 0) {
			if (start == end) {
				count[node] ^= 1;
			}
			else {
				if (count[node] > 0) {
					count[node] = ((end - start + 1) * lazy[node] - count[node]);
				}
				else {
					count[node] += (end - start + 1) * lazy[node];
				}
				lazy[node * 2 + 1] ^= lazy[node];
				lazy[node * 2 + 2] ^= lazy[node];
			}
			lazy[node] = 0;
		}
		if (start > end || start > r || end < l) return;

		if (start >= l && end <= r) {
			if (start == end) {
				count[node] ^= 1;
			}
			else {
				if (count[node] > 0) {
					count[node] = ((end - start + 1) * val - count[node]);
				}
				else {
					count[node] += (end - start + 1) * val;
				}
				lazy[node * 2 + 1] ^= val;
				lazy[node * 2 + 2] ^= val;
			}
	    return;
		}
		int mid = (start + end) / 2;
	  int left = (node * 2) + 1;
	  int right = (node * 2) + 2;
		updateRange(left, start, mid, l, r, val, count, lazy);
		updateRange(right, mid + 1, end, l, r, val, count, lazy);
		count[node] = count[node * 2 + 1] + count[node * 2 + 2];
	}
```
* Code for `range query`:
```c++
	int searchRange(int node, int start, int end, int l, int r, int count[], int lazy[]) {
		// Check if this node is marked for lazy updation and if yes, udpate it.
		if (lazy[node] != 0) {
			if (start == end) {
				count[node] ^= 1;
			}
			else {
				if (count[node] > 0) {
						count[node] = ((end - start + 1) * lazy[node] - count[node]);
				}
				else {
						count[node] += (end - start + 1) * lazy[node];
				}
				lazy[node*2 + 1] ^= lazy[node];
				lazy[node*2 + 2] ^= lazy[node];
			}
			lazy[node] = 0;
		}
		if (start > end || start > r || end < l) return 0;
		if (start >= l && end <= r)  return count[node];
		int mid = (start + end) / 2 ;
		int p1 = searchRange((node * 2) + 1, start, mid, l, r, count, lazy);
		int p2 = searchRange((node * 2) + 2, mid + 1, end, l, r, count, lazy);
		return p1 + p2;
	}
```
 
## Mathematical formulas and proofs
* **Sum of 1 to n**
![image_for_arithmetic](/images/arithmatic_sequence.png)
  
    Proof:
    > Suppose S(n) = 1 + 2 + . . . + n     &nbsp; &nbsp; &nbsp; &nbsp; (eq 1)  
    > We can also say that, S(n) = n + n-1 + n-2 + . . . + 1 &nbsp; &nbsp; &nbsp; &nbsp; (eq 2)   
    > Adding eq 1 and eq 2, we have,
    > 2 S(n) = (n + 1) + (n + 1) + . . . (n + 1)  
    > We should note that each number is (n + 1) and there are n elements. So the sum would be: n * (n + 1).
    > So ![image for arithmetic2](/images/arithmetic_seq_1.png)
  
* **Sum of square of a sequence**  
![image for arithmetic2](/images/arithmetic_seq_square.png)
 
    Proof:  
    > We know that  &nbsp; &nbsp; ![image_for_lemma](/images/arithmetic_lemma.png) &nbsp; &nbsp; (can be proved using mathematical induction). So  &nbsp; &nbsp; ![image for first step](/images/arithmetic_sqr_proof_step_1.png) &nbsp; &nbsp; which can be written as &nbsp; &nbsp; ![image for step2](/images/arithmetic_sqr_proof_step_2.png). &nbsp; &nbsp;  
    > By reducing the above equation we will get &nbsp; &nbsp; ![image for arithmetic2](/images/arithmetic_seq_1.png)
 
* **Sum of cubes of a sequence**   
![image for arithmetic3](/images/arithmetic_seq_cubes.png)
 
    Proof:
    > We can prove this by induction. There is another way of proving this by using [link](https://proofwiki.org/wiki/Sum_of_Sequence_of_Cubes)

* **Logarithm and its use**

    The `Harmonic numbers` arise as a special case of
    arithmetic progression, namely
    H(n) = S(n, −1). They reflect the sum of the progression
    of simple reciprocals, namely  
    ![log harmonic](/images/log_harmonic.png)
      
    > The Harmonic numbers prove important because they usually
    explain “where the log comes from” when one magically pops
    out from algebraic manipulation. For example, the key to
    analyzing the average case complexity of Quicksort is the
    summation &nbsp; ![log harmonic sum](/images/log_harmonic_sum.png)  
     Employing the Harmonic number identity immediately
     reduces this to &nbsp; ![image for n log n](/images/theta_n_log_n.png)

    _Important properties_:  
        ![log property 1](/images/log_property_1.png)  
        ![log_property 2](/images/log_property_2.png)  
    
    _Observations_:  
        
     * **The base of the logarithm has no real impact on the growth rate**: 
        
        Compare the following three values:   
             ![log value example](/images/log_value_example.png)  
        A big change in the base of the logarithm produces 
        little difference in the value of the log. Changing
        the base of the log from a to c involves dividing by
        &nbsp; ![log c a](/images/log_c_a.png). This conversion factor is lost to
        the Big Oh notation whenever a and c are constants.
        Thus we are usually justified in ignoring the base of
        the logarithm when analyzing algorithms.
        
     * **Logarithms cut any function down to size**:  
        The growth rate of the logarithm of any polynomial
        function is O(lg n). This follows because  
        ![log power reduction](/images/log_power_reduction.png)
            
    #### Importance of even split  
    
    Problem:   
    How many queries does binary search take on
    the million-name Manhattan phone book if each split
    was 1/3 to 2/3 instead of 1/2 to 1/2?
    
    Solution:   
    When performing binary searches in a
    telephone book, how important is it that each query
    split the book exactly in half? Not much. For the
    Manhattan telephone book, we now use &nbsp;
    ![image for even split](/images/log_base_example_1.png) &nbsp; queries in the worst case,
    not a significant change from &nbsp;
    ![image for even split](/images/log_base_example_2.png) &nbsp;. The power of binary search
    comes from its logarithmic complexity, not the base of
    the log. 

## Mathematical concepts and Algorithms
* **Finding the number of factors of a given number**  
    
    > Theorem: The number of factors of a given number can be expressed as (x + 1) * (y + 1) * (z + 1) where x, y, z are the number of repetition of the prime factors of that number.  
     Ex:  
     &nbsp; &nbsp; Let the number be 105.  
     &nbsp; &nbsp; The factors of this number are 1, 3, 5, 7, 15, 21, 35, 105. Which is 8.  
     &nbsp; &nbsp; Now consider the prime factors of 105.  
     &nbsp; &nbsp; They are 3, 5, 7. So the number of repetition for each of them is 1.  
     &nbsp; &nbsp; So we can say that the number of  
     &nbsp; &nbsp; factors would be (1 + 1) * (1 + 1) * (1 + 1) = 8

* **Algorithm for calculating polynomial equations:**  

    > ![image for equation 1](/images/pol_equation.png)
    > 
    > We need to calculate p(x)

    One naive algorithm or brute force algorithm would be:
    
    ```c++
     int p = a[0] ;
     int xpower = 1;
     for (int i = 1; i <= n; ++i) {
        xpower = x ∗ xpower;
        p = p + a i ∗ xpower;
     }
    ```
    > In this algorithm we have to perform 2*n multiplications and n additions. Which is not that good.
    
    > One better algorithm is `Horner's Method`:
    
    ```c++
    int res = 0;
    for (int i = n; i <= 0; --i) {
        res = (res * x) + a[i];
    }
    return res;
    ```
    > This algorithm assumes the fact that,
    > ![image for equation1](/images/pol_equation.png) can be written as
    > ![image for equation2](/images/pol_equation_1.png)
    > If we go on like that we will have `a[n]` and next one will be
    > `a[n-1]*x`.
    
* **Fast Exponentiation algorithm**
    
    The simplest algorithm performs n − 1 multiplications, by computing a × a ×
    . . . × a. However, we can do better by observing that 
    n = &nbsp; ![image for floor and ceil](/images/floor_and_ceil.png)  
    If n is even, then &nbsp; ![image for relation](/images/algo_fast_exp1.png) &nbsp;. If n is odd, then &nbsp; ![image for relation](/images/algo_fast_exp2.png)  
    
    > In either case, we have halved the size of our exponent
      at the cost of, at most, two multiplications, so
      O(lg n) multiplications suffice to compute the final
      value.

```c++
    int power(a, n) {
        if (n == 0) return 1;
        x = power(a, floor(n/2))
        if (n % 2 == 0) // n is even
            return x * x
        else
            return a * (x * x)
    }
```      
      
