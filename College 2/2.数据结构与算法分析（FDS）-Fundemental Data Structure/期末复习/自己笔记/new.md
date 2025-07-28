好的，我们来深入探讨**静态查找树**，特别是**次优查找树**的获得方法、计算方法及其代码实现。

### 静态查找树（次优查找树）概述

**静态查找树**是指一旦构建完成，其结构就不再改变的查找树。树中的关键字集合是固定的，并且通常已知每个关键字的查找频率（或概率）。

**次优查找树**（Nearly Optimal Binary Search Tree），或称**加权查找树**（Weighted Binary Search Tree），是在已知每个关键字查找概率的情况下，为了最小化**平均查找长度**（ASL - Average Search Length）而构建的二叉查找树。

为什么是“次优”而不是“最优”？

理论上存在一个最优二叉查找树，它能给出最小的 ASL。然而，构建真正的最优二叉查找树的算法（如动态规划）时间复杂度较高（O(n3) 或 O(n2)），计算复杂。次优查找树是旨在以较低的复杂度（通常是 O(n2) 或 O(nlogn) 甚至更低）构建一棵接近最优 ASL 的树。

次优查找树通常采用一种**贪心策略**来构建，其核心思想是：**平均查找长度最短的二叉查找树，其根节点的权值（查找频率）应尽量大，且左右子树的权值之和应尽量接近。**

### 次优查找树的获得方法和计算方法

构建次优查找树通常使用**平衡加权的方法**，最常见的一种是基于**中值分割**的贪心策略。

**假设：**

- 我们有 n 个已排序的关键字 $K\_1 \< K\_2 \< \\dots \< K\_n$。
- 每个关键字 K_i 有一个查找概率（或频率） P_i。
- 为了处理查找失败的情况，我们通常还会引入 n+1 个虚节点 D_0,D_1,dots,D_n，分别代表查找关键字小于 K_1、介于 K_i 和 K_i+1 之间、或大于 K_n 的情况。每个虚节点 D_i 也有一个查找概率 Q_i。

为了简化，这里我们主要关注只考虑成功查找的情况（即只有 P_i）。如果考虑查找失败，需要更复杂的加权方式，但基本思想类似。

**获得方法（贪心策略）：**

1. **排序关键字：** 确保所有关键字是按升序排列的。

2. 选择根节点：

    在给定范围 

   ```
   [low, high]
   ```

    的关键字中，选择一个关键字 

   K_root

    作为当前子树的根节点，使得其左右子树的

   总查找概率

   （或总权值）尽可能接近。

   - 理想情况下，我们希望 $sum_k=lowroot−1P_kapproxsum_k=root+1highP_k $
   - 这可以通过计算所有关键字的总概率，然后遍历查找一个分裂点来实现。

3. 递归构建：

   - 以选定的 K_root 为根节点。
   - 对 K_low 到 K_root−1 的关键字集合，递归地构建其左子树。
   - 对 K_root+1 到 K_high 的关键字集合，递归地构建其右子树。

**计算根节点的策略：**

1. 计算当前子树（对应关键字 K_low 到 K_high）的总权值 W=sum_k=lowhighP_k。

2. 从 K_low 开始向右遍历，计算累积权值 W_left=P_low+P_low+1+dots+P_m。

3. 找到第一个 

   m

    (作为根节点下标) 使得 

   W_leftgeW/2

   。这个 

   K_m

    就是被选作根的关键字。

   - 另一种常用策略是，选择 K_m 和 K_m+1 中，使得 P_m 或 P_m+1 离中心点最近的那个作为根。这里我们简化为最常见的取第一个大于等于一半总权值的点。

### 最终代码实现

我们将实现一个函数来构建次优查找树，并计算其平均查找长度。

**数据结构定义：**

C

```c
#include <stdio.h>
#include <stdlib.h>

// 假设关键字是整数，概率是浮点数
typedef struct Node {
    int key;
    float probability;
    struct Node *left;
    struct Node *right;
} Node;

// 用于存储关键字和概率
typedef struct {
    int key;
    float prob;
} KeyProb;

// 比较函数，用于排序 KeyProb 数组
int compareKeyProb(const void *a, const void *b) {
    return ((KeyProb *)a)->key - ((KeyProb *)b)->key;
}

// 全局变量或通过参数传递：关键字和概率的集合
KeyProb *sorted_keys_probs; // 存储排序后的关键字和概率
int num_keys;                // 关键字总数

// 前向声明
Node* buildOptimalBST(int low, int high);
float calculateASL(Node* root, int depth);

// 辅助函数：计算子树总概率
float sumProbabilities(int low, int high) {
    float sum = 0.0;
    for (int i = low; i <= high; ++i) {
        sum += sorted_keys_probs[i].prob;
    }
    return sum;
}

/**
 * 构建次优二叉查找树的递归函数
 * low: 当前子树包含的关键字范围的起始下标 (在 sorted_keys_probs 数组中)
 * high: 当前子树包含的关键字范围的结束下标 (在 sorted_keys_probs 数组中)
 */
Node* buildOptimalBST(int low, int high) {
    if (low > high) {
        return NULL; // 空子树
    }

    if (low == high) {
        // 只有一个关键字，它就是根
        Node* newNode = (Node*)malloc(sizeof(Node));
        newNode->key = sorted_keys_probs[low].key;
        newNode->probability = sorted_keys_probs[low].prob;
        newNode->left = NULL;
        newNode->right = NULL;
        return newNode;
    }

    // 1. 计算当前子树的总概率
    float totalProb = sumProbabilities(low, high);

    // 2. 寻找最佳根节点 (贪心策略: 左右子树概率和尽可能接近)
    // 这里我们选择第一个累积概率 >= 总概率一半的关键字作为根
    float currentSum = 0.0;
    int rootIndex = low; // 默认根节点为最左边的关键字
    for (int i = low; i <= high; ++i) {
        currentSum += sorted_keys_probs[i].prob;
        if (currentSum >= totalProb / 2.0) {
            rootIndex = i;
            break; // 找到第一个使左侧累积和达到一半的关键字
        }
    }
    
    // 如果循环结束时 totalProb / 2.0 仍未达到，可能是浮点数精度问题，
    // 或所有概率都很小。此时 rootIndex 会是 high。
    // 另一种更优的根选择策略会比较 rootIndex 和 rootIndex-1，
    // 找出使左右子树权重更接近的那个。但这里为了简化，使用最简单的首次超过一半的。

    // 3. 创建根节点
    Node* root = (Node*)malloc(sizeof(Node));
    if (!root) {
        perror("Failed to allocate memory for node");
        exit(EXIT_FAILURE);
    }
    root->key = sorted_keys_probs[rootIndex].key;
    root->probability = sorted_keys_probs[rootIndex].prob;

    // 4. 递归构建左右子树
    root->left = buildOptimalBST(low, rootIndex - 1);
    root->right = buildOptimalBST(rootIndex + 1, high);

    return root;
}

/**
 * 计算二叉查找树的平均查找长度 (ASL)
 * ASL = Sum(每个节点深度 * 查找概率)
 * 根节点深度为 1
 */
float calculateASL(Node* root, int depth) {
    if (root == NULL) {
        return 0.0;
    }
    // 当前节点贡献的 ASL 部分
    float currentASL = root->probability * depth;
    // 递归计算左右子树的 ASL
    currentASL += calculateASL(root->left, depth + 1);
    currentASL += calculateASL(root->right, depth + 1);
    return currentASL;
}

// 辅助函数：打印树（中序遍历，用于验证结构）
void printTreeInorder(Node* node) {
    if (node == NULL) {
        return;
    }
    printTreeInorder(node->left);
    printf("Key: %d, Prob: %.2f\n", node->key, node->probability);
    printTreeInorder(node->right);
}

// 辅助函数：释放树的内存
void freeTree(Node* node) {
    if (node == NULL) {
        return;
    }
    freeTree(node->left);
    freeTree(node->right);
    free(node);
}

int main() {
    // 示例关键字和概率
    // 注意：这里的关键字数组需要预先根据关键字值排序
    KeyProb keys_with_probs[] = {
        {10, 0.15},
        {20, 0.10},
        {30, 0.05},
        {40, 0.20},
        {50, 0.25},
        {60, 0.08},
        {70, 0.07},
        {80, 0.10}
    };
    num_keys = sizeof(keys_with_probs) / sizeof(KeyProb);

    // 确保关键字是排序的 (虽然这里已经手动排序了，但如果是动态数据，需要qsort)
    qsort(keys_with_probs, num_keys, sizeof(KeyProb), compareKeyProb);
    
    // 将排序后的数据赋值给全局指针
    sorted_keys_probs = keys_with_probs;

    // 构建次优查找树
    Node* root = buildOptimalBST(0, num_keys - 1);

    printf("构建的次优查找树 (中序遍历):\n");
    printTreeInorder(root);

    // 计算平均查找长度 (根节点深度为 1)
    float asl = calculateASL(root, 1);
    printf("\n平均查找长度 (ASL): %.4f\n", asl);

    // 释放树的内存
    freeTree(root);

    return 0;
}
```

### 代码解释和分析

1. **`Node` 结构体：** 定义了二叉查找树的节点，包含关键字 `key`、查找概率 `probability`，以及左右子节点的指针。

2. **`KeyProb` 结构体：** 用于在数组中存储原始的关键字及其概率。

3. **`sumProbabilities(int low, int high)`：** 辅助函数，计算给定索引范围内关键字的概率总和。

4. `buildOptimalBST(int low, int high)`：

    这是构建次优查找树的核心递归函数。

   - 基线条件：
     - `low > high`：表示当前范围为空，返回 `NULL`。
     - `low == high`：表示当前只有一个关键字，它就是子树的根节点。
   - 贪心选择根节点：
     1. 计算当前范围 `[low, high]` 内所有关键字的 `totalProb`。
     2. 遍历 `i` 从 `low` 到 `high`，累加 `currentSum`。
     3. 找到第一个 `rootIndex`，使得 `currentSum` 大于或等于 `totalProb` 的一半。这个 `sorted_keys_probs[rootIndex]` 就被选作当前的根节点。这种策略的目标是尽量平衡左右子树的总概率。
   - **递归构建：** 以 `rootIndex` 为中心，递归地调用 `buildOptimalBST` 来构建左子树 (`low` 到 `rootIndex-1`) 和右子树 (`rootIndex+1` 到 `high`)。

5. `calculateASL(Node* root, int depth)`：

    递归计算平均查找长度。

   - ASL 的定义是 $ \sum (\text{节点深度} \times \text{该节点查找概率}) $。
   - 函数的 `depth` 参数记录了当前节点的深度（根节点深度设为 1）。
   - 每个节点的贡献是 `probability * depth`。
   - 通过递归调用左右子树并累加贡献，最终得到总的 ASL。

6. `main` 函数：

   - 定义了一组示例关键字和它们的查找概率。**注意：这些关键字必须是预先排序的**，因为二叉查找树的性质要求左子树的关键字小于根，右子树的关键字大于根。`qsort` 函数用于确保这一点。
   - 调用 `buildOptimalBST` 构建树。
   - 调用 `printTreeInorder` 打印树的结构（中序遍历会按关键字顺序打印）。
   - 调用 `calculateASL` 计算并打印 ASL。
   - 调用 `freeTree` 释放动态分配的内存。

### 次优查找树的复杂度

- **构建：** `buildOptimalBST` 函数的递归深度为 O(N)。在每一层递归中，`sumProbabilities` 函数需要 O(N) 时间，寻找根节点也需要 O(N) 时间。因此，总的构建时间复杂度为 O(N2)。
- **查找（平均）：** 平均查找长度（ASL）取决于树的结构和概率分布。对于构建的次优查找树，ASL 接近于 O(logN)（在平衡的情况下）。
- **空间复杂度：** O(N)，用于存储树的节点。

### 局限性

- 这种贪心策略构建的树是“次优”的，不保证是真正意义上的最优二叉查找树。真正的最优二叉查找树需要使用更复杂的动态规划算法（如 Knuth 的算法或 Garsia-Wachs 算法），其时间复杂度通常为 O(N2)。
- 此实现没有考虑查找失败（虚节点）的情况，如果考虑查找失败，ASL 的计算和根节点的选择策略会更复杂。

这个代码提供了一个构建静态次优查找树的完整框架，可以帮助您理解其基本原理和实现方法。