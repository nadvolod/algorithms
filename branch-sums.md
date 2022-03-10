# branch sums

## prompt

## initial questions and notes

	// returning a sum of each of the branches
	// the # of branches is determined by number of leaf nodes
	// can the binary tree be null/empty?
	// can everything be done in memory?
	// one node, 1?
	// can the tree have the same values?
  // leaf

Time = O(n) because we need to traverse all of the nodes in the tree. All the operations at the node are constant time operations.
Space complexity = affected by the list of sums that we need to return. Also affected by whether our algorithm is recursive or iterative.
- A balanced binary tree has a O(log(n)) space complexity because we're constantly eliminating half of a tree
- Worst case, in a tree that has only nodes in one branch, O(n) complexity because we are never eliminating half of the tree
