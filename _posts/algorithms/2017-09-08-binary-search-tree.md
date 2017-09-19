BST(Binary Search Tree)



BST는 노드 기반 이진 트리 자료구조이다.

- 노드 왼쪽 서브 트리는 해당 노드의 키보다 작은 키를 갖는 노드를 포함한다.
- 노드 오른쪽 서브 트리는 해당 노드의 키보다 큰 키를 갖는 노들르 포함한다.
- 왼쪽과 오른쪽의 서브트리 또한 각각 BST가 돼야 한다.
  그곳엔 중복된 노드들이 없어야 한다.



위의 BST의 속성은 키를 정렬하여 줘서, 검색, 최소값, 최대값 구하기 같은 작업을 빠르게 완료할 수 있다. 만약 순서가 없다면, 우리는 주어진 키에 대해 모든 키를 비교해야 한다.



### 키 검색하기

BST에서 주어진 키를 검색하기 위해선, 우리는 먼저 그 키와 루트를 비교한다. 만약 키가 루트에 존재한다면, 루트를 반환한다. 키보다 루트의 키가 더 크다면, 루트 노드의 오른쪽 서브 트리를 탐색하고, 그렇지 않으면 왼쪽 서브 트리를 탐색한다.

```java
class Node {
  int key;
  Node left, right;

  public Node(int item) {
    key = item;
    left = right = null;
  }
}
```



```java
// A utility function to search a given key in BST
public Node search(Node root, int key)
{
    // Base Cases: root is null or key is present at root
    if (root==null || root.key==key)
        return root;
 
    // val is greater than root's key
    if (root.key > key)
        return search(root.left, key);
 
    // val is less than root's key
    return search(root.right, key);
}
```



키 삽입하기

새로 삽입되는 키는 언제나 잎 노드이다. 루트부터 잎 노드에 도착할 때까지 키를 검색한다. 잎 노드를 찾았을 때, 새로운 노드는 잎 노드의 자식 노드로 추가된다.

```
         100                               100
        /   \        Insert 40            /    \
      20     500    --------->          20     500 
     /  \                              /  \  
    10   30                           10   30
                                              \   
                                              40
```

```java
// Java program to demonstrate insert operation in binary search tree
class BinarySearchTree {
 
    /* Class containing left and right child of current node and key value*/
    class Node {
        int key;
        Node left, right;
 
        public Node(int item) {
            key = item;
            left = right = null;
        }
    }
 
    // Root of BST
    Node root;
 
    // Constructor
    BinarySearchTree() { 
        root = null; 
    }
 
    // This method mainly calls insertRec()
    void insert(int key) {
       root = insertRec(root, key);
    }
     
    /* A recursive function to insert a new key in BST */
    Node insertRec(Node root, int key) {
 
        /* If the tree is empty, return a new node */
        if (root == null) {
            root = new Node(key);
            return root;
        }
 
        /* Otherwise, recur down the tree */
        if (key < root.key)
            root.left = insertRec(root.left, key);
        else if (key > root.key)
            root.right = insertRec(root.right, key);
 
        /* return the (unchanged) node pointer */
        return root;
    }
 
    // This method mainly calls InorderRec()
    void inorder()  {
       inorderRec(root);
    }
 
    // A utility function to do inorder traversal of BST
    void inorderRec(Node root) {
        if (root != null) {
            inorderRec(root.left);
            System.out.println(root.key);
            inorderRec(root.right);
        }
    }
 
    // Driver Program to test above functions
    public static void main(String[] args) {
        BinarySearchTree tree = new BinarySearchTree();
 
        /* Let us create following BST
              50
           /     \
          30      70
         /  \    /  \
       20   40  60   80 */
        tree.insert(50);
        tree.insert(30);
        tree.insert(20);
        tree.insert(40);
        tree.insert(70);
        tree.insert(60);
        tree.insert(80);
 
        // print inorder traversal of the BST
        tree.inorder();
    }
}
// This code is contributed by Ankur Narain Verma
```



시간 복잡도

검색과 삽입 작업에서 최악의 경우(worst-case)는 O(h) (h: BST의 깊이)이다. 최악의 경우엔, 루트에서 가장 깊은 잎 노드까지 탐색해야 할 것이다. 편향된 트르의 높이는 n이 될 것이고, 검색과 삽입 작업의 시간 복잡도는 O(n)이 된다.



