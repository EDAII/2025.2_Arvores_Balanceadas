# 2025.2_Arvores_Balanceadas

**Estruturas de Dados 2**

---

## 👥 Dupla

| Matrícula | Aluno                                                          |
| --------- | -------------------------------------------------------------- |
| 180028847 | [Vinicius Gabriel R S Brito](https://github.com/vini051)       |
| 180029240 | [Wesley Pedrosa dos Santos](https://github.com/wesleysantos00) |

---

## ℹ️ Sobre

Terceiro trabalho da disciplina de Estruturas de Dados 2, com o propósito de avaliar e aplicar os conhecimentos em **árvores balanceadas**. Consiste na solução de problemas do LeetCode que exploram abordagens para solução de problemas, evidenciando a assimilação prática dos conceitos teóricos vistos na matéria.

## 📝 Questões Desenvolvidas

Foram solucionadas cinco questões do LeetCode focadas em problemas que envolvem estruturas de árvores:

1. [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
2. [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)
3. [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)
4. [129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)
5. [1382. Balance a Binary Search Tree](https://leetcode.com/problems/balance-a-binary-search-tree/description/)

---

## 🎥 Vídeo de Apresentação

Neste vídeo, apresentamos um resumo completo do trabalho desenvolvido, abordando os principais pontos discutidos ao longo do projeto.

[Assista no YouTube](https://youtu.be/Bludg3eMOcU)

---

## 💻 Códigos das Soluções

1. [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

```python
from collections import deque
from typing import Optional, List

# Definition for a binary tree node.
class TreeNode:
    def _init_(self, val: int = 0, left: 'Optional[TreeNode]' = None, right: 'Optional[TreeNode]' = None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        res: List[List[int]] = []
        q: deque[Optional[TreeNode]] = deque([root])

        while q:
            level: List[int] = []
            for _ in range(len(q)):
                node = q.popleft()
                if node:
                    level.append(node.val)
                    q.append(node.left)
                    q.append(node.right)
            if level:
                res.append(level)

        return res
```

2. [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        mid = len(nums) // 2
        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid+1:])
        return root
```

3. [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
            def height(node):
                if node is None:
                    return 0
                left = height(node.left)
                if left == -1:
                    return -1
                right = height(node.right)
                if right == -1:
                    return -1
                if abs(left - right) > 1:
                    return -1
                return max(left, right) + 1
            return height(root) != -1
```

4. [129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def _init_(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        total = ""

        def dfs(root, total):
            if root:
                if root.left is None and root.right is None:
                    return int(total + str(root.val))

                total += str(root.val)
                return dfs(root.left, total) + dfs(root.right, total)
            return 0

        return dfs(root, total)
```

5. [1382. Balance a Binary Search Tree](https://leetcode.com/problems/balance-a-binary-search-tree/description/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def balanceBST(self, root: TreeNode) -> TreeNode:
        def get_height(node): #para calcular o fator de balanceamento
            return node.height if node else 0
        
        def update_height(node): #pra atualizar a altura sempre que modificar a arvore
            node.height = 1 + max(get_height(node.left), get_height(node.right))
        
        def get_balance(node): #calcular o fator de balanceamento
            return get_height(node.left) - get_height(node.right) if node else 0
        
        def right_rotate(y): #rotacao a direita
            x = y.left
            T2 = x.right
            x.right = y
            y.left = T2
            update_height(y)
            update_height(x)
            return x
        
        def left_rotate(x): #rotacao a esqeurda
            y = x.right
            T2 = y.left
            y.left = x
            x.right = T2
            update_height(x)
            update_height(y)
            return y
        
        def insert_avl(node, key): #insere novo node
            if not node:
                new_node = TreeNode(key)
                new_node.height = 1
                return new_node
            if key < node.val:
                node.left = insert_avl(node.left, key)
            else:
                node.right = insert_avl(node.right, key)
            
            update_height(node)
            balance = get_balance(node)
            
            if balance > 1 and key < node.left.val:     #rotacao simples a direita
                return right_rotate(node)
            if balance < -1 and key > node.right.val:   #rotacao simples a esquerda
                return left_rotate(node)
            if balance > 1 and key > node.left.val:     #rotacao dupla a direita
                node.left = left_rotate(node.left)
                return right_rotate(node)
            if balance < -1 and key < node.right.val:   #rotacao dupla a esquerda
                node.right = right_rotate(node.right)
                return left_rotate(node)
            return node
        
        def collect(node, arr): #obtem os nodes em ordem
            if node:
                collect(node.left, arr)
                arr.append(node.val)
                collect(node.right, arr)
        
        def set_height(node): #inicializa height de cada node
            if not node:
                return 0
            node.height = 1 + max(set_height(node.left), set_height(node.right))
            return node.height
        
        set_height(root)
        
        values = []
        collect(root, values)
        avl_root = None
        for v in values:
            avl_root = insert_avl(avl_root, v)
        
        return avl_root
```
