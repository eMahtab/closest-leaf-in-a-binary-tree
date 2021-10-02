# Closest Leaf in a binary tree
Closest Leaf node from a given node in a binary tree

## https://leetcode.com/problems/closest-leaf-in-a-binary-tree

Given the root of a binary tree where every node has a unique value and a target integer k, return the value of the nearest leaf node to the target k in the tree.

Nearest to a leaf means the least number of edges traveled on the binary tree to reach any leaf of the tree. Also, a node is called a leaf if it has no children.

```
Constraints:

1. The number of nodes in the tree is in the range [1, 1000].
2. 1 <= Node.val <= 1000
3. All the values of the tree are unique.
4. There exist some node in the tree where Node.val == k.

```


## Approach : Create graph from the given tree and BFS
We will first create a graph from the given binary tree. Once we have the graph, all we need to do is, to find the first leaf node using BFS, starting from the sourceNode `k`.
So this problem is, finding the shortest path in an unweighted undirected graph. 

!["Closest leaf in a binary tree"](closest-leaf-in-a-binary-tree.jpg?raw=true)

# Implementation : Graph + BFS
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    TreeNode sourceNode = null;
    public int findClosestLeaf(TreeNode root, int k) {
        if(root == null)
            return -1;
        Map<Integer,List<TreeNode>> graph = new HashMap<>();
        // Create a graph representation from given binary 
        dfs(root, graph, null, k);
        // Next do BFS traversal to find the closest leaf to node k
        Queue<TreeNode> q = new ArrayDeque<>();
        Set<Integer> visited = new HashSet<>();
        // Start the BFS from the sourceNode k
        q.add(sourceNode);
        visited.add(sourceNode.val);
        while(!q.isEmpty()) {
            int size = q.size();
            for(int i = 0; i < size; i++) {
                TreeNode node = q.remove();
                if(node.left == null && node.right == null)
                    return node.val;
                List<TreeNode> neighbors = graph.get(node.val);
                // add neighbors to the queue
                for(TreeNode neighbor : neighbors) {
                    if(!visited.contains(neighbor.val)) {
                        q.add(neighbor);
                        visited.add(neighbor.val);
                    }  
                }
            }
        }
        return -1;
    }
    
    private void dfs(TreeNode node, Map<Integer, List<TreeNode>> graph, TreeNode parent, int k) {
        if(node == null)
            return;
        if(node.val == k)
            sourceNode = node;
        graph.putIfAbsent(node.val, new ArrayList<TreeNode>());
        List<TreeNode> neighbors = graph.get(node.val);
        if(node.left != null)
            neighbors.add(node.left);
        if(node.right != null)
            neighbors.add(node.right);
        if(parent != null) {
            neighbors.add(parent);
        }
        dfs(node.left, graph, node, k);
        dfs(node.right, graph, node, k);
    }
}
```

### Complexity Analysis :
```
Time Complexity: O(N) where N is the number of nodes in the given input tree.

Space Complexity: O(N), the size of the graph.
```


# References :
1. https://leetcode.com/problems/closest-leaf-in-a-binary-tree/solution

## Similar Question :
All nodes distance k in a binary tree https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree
