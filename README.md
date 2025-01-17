# DSA
## Index
* [Algorithm Analysis](#algorithm-analysis)
* [Stack and its applications](#stack)
* [Queue and its types](#queue)
* [Linked List](#linked-list)
* [Linear Search](#linear-search)
* [Binary Search](#binary-search)
* [Insertion Sort](#insertion-sort)
* [Bubble Sort](#bubble-sort)
* [Counting Sort](#counting-sort)
* [Quick Sort](#quick-sort)
* [Merge Sort](#merge-sort)
* [Heap Sort](#heap-sort)
* [Binary Search Tree](#binary-search-tree)
* [Max Heap](#heap-tree)
* [AVL Tree](#avl-tree)
* [Red Black Tree](#red-black-tree)
## Algorithm Analysis
### Algorithm
* Set of rules/instructions that is to be followed to obtain a desired output from a given input

### Data Structure
* Orderly arrangement of data in computers to use it more efficiently

### Space Complexity 
* Memory space required for the successful execution

### Time Complexity
* Total times required for the successful execution

### Asymptotic Notations
* Big O Notation - Worst Case Scenaio
* Omega Notation - Best Case Scenario
* Theta Notation - Average Case Scenario

### Calculating Time Complexity
![image](https://github.com/niteshjeganathan/DSA/assets/89623604/6a1021d1-990b-4d2f-810d-cd1ebbcbd07b)

### Orders of Growth
![image](https://github.com/niteshjeganathan/DSA/assets/89623604/dba5fa15-ff9d-4fa0-8571-dee32304eb78)

## Linear Data Structures
### Stack
* LIFO ( Last in First Out )
* Can be inserted/deleted at the top of the stack
```c++
#include <iostream>
#include <vector>
using namespace std;

class Stack
{
private:
    vector<int> stack;
    int top = -1;

public:
    void push(int x)
    {
        stack.push_back(x);
        top++;
    }

    int pop()
    {
        int value = stack.back();
        stack.pop_back();
        top--;
        return value;
    }

    void traversal()
    {
        for (int i = 0; i <= top; i++)
        {
            cout << stack[i] << " ";
        }
        cout << endl;
    }
};

int main()
{
    Stack stack;
    stack.push(5);
    stack.push(10);
    stack.pop();
    stack.traversal();
}
```
### Expression Evaluation
```c++
#include <iostream>
#include <stack>
#include <map>
using namespace std;

bool evaluateExpression(string expr)
{
    stack<char> stack;
    map<char, char> mp;
    mp[']'] = '[';
    mp[')'] = '(';
    mp['}'] = '{';
    for (int i = 0; i < expr.length(); i++)
    {
        if (expr[i] == '(' || expr[i] == '{' || expr[i] == '[')
        {
            stack.push(expr[i]);
        }
        else if (mp.count(expr[i]) > 0)
        {
            if (stack.empty())
                return false;
            if (mp[expr[i]] != stack.top())
                return false;
            stack.pop();
        }
        else
        {
            continue;
        }
    }
    if (stack.empty())
        return true;
    return false;
}

int main()
{
    cout << evaluateExpression("(x+5)[{}] + 2") << endl;
}
```

### Infix to Postfix
```c++
#include <iostream>
#include <stack>
#include <map>
using namespace std;

// Higher precedence can sit on top of the stack
// If right associativity, like ^, when equal operators, we won't pop out
string convertToPostfix(string expr)
{
    map<char, int> precedence{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}, {'^', 3}, {'(', 0}};
    stack<char> operators;
    string res = "";

    for (int i = 0; i < expr.length(); i++)
    {
        if (precedence.count(expr[i]) > 0)
        {
            if (operators.empty())
            {
                operators.push(expr[i]);
                continue;
            }

            if (precedence[operators.top()] < precedence[expr[i]] || (expr[i] == '^' && operators.top() == '^'))
            {
                operators.push(expr[i]);
                continue;
            }

            if (precedence[operators.top()] >= precedence[expr[i]] && expr[i] != '^')
            {
                while (!operators.empty() && precedence[operators.top()] >= precedence[expr[i]])
                {
                    res += operators.top();
                    operators.pop();
                }
                operators.push(expr[i]);
                continue;
            }
        }
        else if (expr[i] == ')')
        {
            while (operators.top() != '(')
            {
                res += operators.top();
                operators.pop();
            }
            operators.pop();
        }
        else
        {
            res += expr[i];
        }
    }
    while (!operators.empty())
    {
        res += operators.top();
        operators.pop();
    }
    return res;
}

int main()
{
    cout << convertToPostfix("(3+5*2)*5") << endl;
}
```

### Infix to Prefix
```c++
#include <iostream>
#include <stack>
#include <map>
#include <algorithm>
using namespace std;

// If higher precedence or equal precendence (except ^ ), then add to stack
// If equal precedence and ^ or lower precendence, then pop till condition doesn't exist so
string convertToPrefix(string expr)
{
    stack<int> operators;
    map<char, int> precedence{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}, {'^', 3}, {')', 0}};
    string res = "";

    for (int i = expr.length() - 1; i >= 0; i--)
    {
        if (precedence.count(expr[i]) > 0)
        {
            if (operators.empty())
            {
                operators.push(expr[i]);
                continue;
            }

            if (precedence[operators.top()] < precedence[expr[i]] || (expr[i] != '^' && operators.top() == expr[i]))
            {
                operators.push(expr[i]);
                continue;
            }

            if (precedence[operators.top()] > precedence[expr[i]] || (expr[i] == '^' && operators.top() == '^'))
            {
                while (!operators.empty() && (precedence[operators.top()] > precedence[expr[i]] || (expr[i] == '^' && operators.top() == '^')))
                {
                    res += operators.top();
                    operators.pop();
                }
                operators.push(expr[i]);
                continue;
            }
        }
        else if (expr[i] == '(')
        {
            while (operators.top() != ')')
            {
                res += operators.top();
                operators.pop();
            }
            operators.pop();
        }
        else
        {
            res += expr[i];
        }
    }
    while (!operators.empty())
    {
        res += operators.top();
        operators.pop();
    }
    reverse(res.begin(), res.end());
    return res;
}

int main()
{
    cout << convertToPrefix("5^4^3") << endl;
}
```

### Towers of Hanoi
[https://www.youtube.com/watch?v=rf6uf3jNjbo]
```c++
#include <iostream>
using namespace std;

void towerOfHanoi(int n, char from, char to, char aux)
{
    if (n == 0)
    {
        return;
    }

    towerOfHanoi(n - 1, from, aux, to);
    cout << "Move disk " << (n - 1) << " from rod " << from << " to rod " << to << endl;
    towerOfHanoi(n - 1, aux, to, from);
}

int main()
{
    towerOfHanoi(3, 'A', 'C', 'B');
}
```

### Queue
```c++
#include <iostream>
#include <vector>
using namespace std;

class Queue
{
private:
    vector<int> queue;
    int front = -1;

public:
    void enqueue(int x)
    {
        if (front == -1)
        {
            front++;
        }
        queue.push_back(x);
    }

    void dequeue()
    {
        front++;
    }

    void traversal()
    {
        for (int i = front; i < queue.size(); i++)
        {
            cout << queue[i] << " ";
        }
        cout << endl;
    }
};

int main()
{
    Queue queue;
    queue.enqueue(5);
    queue.enqueue(10);
    queue.enqueue(15);
    queue.dequeue();
    queue.dequeue();
    queue.traversal();
}
```

### Doubly Ended Queue
```c++
#include <iostream>
#include <vector>
using namespace std;

class Queue
{
private:
    vector<int> queue;
    int front = -1;

public:
    void insertEnd(int x)
    {
        if (front == -1)
        {
            front++;
        }
        queue.push_back(x);
    }

    void insertAtBeginning(int x)
    {
        if (front == -1)
        {
            front++;
        }
        queue.insert(queue.begin(), x);
    }

    void traversal()
    {
        for (int i = front; i < queue.size(); i++)
        {
            cout << queue[i] << " ";
        }
        cout << endl;
    }

    void removeAtEnd()
    {
        queue.pop_back();
    }

    void removeAtBeginning()
    {
        front++;
    }
};

int main()
{
    Queue queue;
    queue.insertAtBeginning(5);
    queue.insertEnd(10);
    queue.insertAtBeginning(1);
    queue.insertEnd(15);
    queue.removeAtBeginning();
    queue.removeAtEnd();
    queue.traversal();
}
```

### Linked List
```c++
#include <iostream>
using namespace std;

struct Node
{
    int value;
    Node *next;

    Node(int x)
    {
        value = x;
        next = NULL;
    }
};

class LinkedList
{
private:
    Node *head;

public:
    LinkedList()
    {
        head = NULL;
    }

    void insert(int x)
    {
        if (!head)
        {
            head = new Node(x);
            return;
        }

        Node *curr = head;
        while (curr->next != NULL)
        {
            curr = curr->next;
        }

        curr->next = new Node(x);
    }

    void traverse()
    {
        Node *curr = head;
        while (curr)
        {
            cout << curr->value << " ";
            curr = curr->next;
        }
        cout << endl;
    }

    void insertAtBeginning(int x)
    {
        if (!head)
        {
            head = new Node(x);
            return;
        }

        Node *curr = new Node(x);
        curr->next = head;
        head = curr;
    }

    void remove(int x)
    {
        if (head->value == x)
        {
            Node *temp = head;
            head = head->next;
            delete (temp);
            return;
        }

        Node *curr = head;

        while (curr->next->next)
        {
            if (curr->next->value == x)
            {
                Node *temp = curr->next;
                curr->next = temp->next;
                delete (temp);
                return;
            }
            curr = curr->next;
        }

        if (curr->next->value == x)
        {
            Node *temp = curr->next;
            curr->next = NULL;
            delete (temp);
        }
    }
};

int main()
{
    LinkedList list;

    list.insert(5);
    list.insert(10);
    list.insertAtBeginning(1);
    list.remove(10);

    list.traverse();

    return 0;
}
```


## Searching and Sorting
### Linear Search
* Time Complexity
  * Best Case Scenario: O(1)
  * Average Case Scenario: O(n)
  * Worst Case Scenario: O(n)
```c++
#include <iostream>
#include <vector>
using namespace std;

int linearSearch(vector<int> array, int value)
{
    for (int i = 0; i < array.size(); i++)
    {
        if (array[i] == value)
        {
            return i;
        }
    }
    return -1;
}

int main()
{
    vector<int> array{1, 2, 3, 4, 5};

    cout << linearSearch(array, 2);
}
```

### Binary Search
* Time Complexity:
  * Best Case Scenario: O(1)
  * Average Case Scenario: O(log(n))
  * Worst Case Scenario: O(log(n))
```c++
//Approach 1: Iterative
#include <iostream>
#include <vector>
using namespace std;

int binarySearch(vector<int> array, int value)
{
    int low = 0;
    int high = array.size() - 1;
    int mid;

    while (low <= high)
    {
        mid = (low + high) / 2;
        if (array[mid] < value)
        {
            low = mid + 1;
        }
        else if (array[mid] > value)
        {
            high = mid - 1;
        }
        else
        {
            return mid;
        }
    }
    return -1;
}

int main()
{
    vector<int> array{1, 2, 3, 4, 5};
    cout << binarySearch(array, 3);
    return 0;
}

//Approach 2: Recursive
#include <iostream>
#include <vector>
using namespace std;

int BinarySearch(vector<int> array, int value, int low, int high)
{
    if (low <= high)
    {
        int mid = (low + high) / 2;

        if (array[mid] < value)
        {
            return BinarySearch(array, value, mid + 1, high);
        }
        else if (array[mid] > value)
        {
            return BinarySearch(array, value, low, mid - 1);
        }
        else
        {
            return mid;
        }
    }
    return -1;
}

int main()
{
    vector<int> array{1, 2, 3, 4, 5};
    cout << BinarySearch(array, 4, 0, 4) << endl;
    return 0;
}
```

### Insertion Sort
* Time Complexity
  * Best Case - O(n)
  * Average Case - O(n^2)
  * Worst Case - O(n^2)
```c++
#include <iostream>
#include <vector>
using namespace std;

void sort(vector<int> &array)
{
    int key;
    int j;

    for (int i = 1; i < array.size(); i++)
    {
        j = i - 1;
        key = array[i];

        while (j >= 0 && array[j] > key)
        {
            array[j + 1] = array[j];
            j--;
        }
        array[j + 1] = key;
    }
}

int main()
{
    vector<int> array{2, 3, 4, 1, 5};

    sort(array);

    for (auto i : array)
    {
        cout << i << " ";
    }
    cout << endl;
}
```

### Selection Sort
* Time Complexity
  * Best Case - O(n^2)
  * Average Case - O(n^2)
  * Worst Case - O(n^2)
```c++
#include <iostream>
#include <vector>
using namespace std;

void sort(vector<int> &array)
{
    int min;
    int minIndex;

    for (int i = 0; i < array.size() - 1; i++)
    {
        min = array[i];
        minIndex = i;

        for (int j = i + 1; j < array.size(); j++)
        {
            if (array[j] < min)
            {
                min = array[j];
                minIndex = j;
            }
        }

        array[minIndex] = array[i];
        array[i] = min;
    }
}

int main()
{
    vector<int> array{2, 3, 4, 1, 5};

    sort(array);

    for (auto i : array)
    {
        cout << i << " ";
    }
    cout << endl;
}
```

### Bubble Sort
* Time Complexity
  * Best Case - O(n)
  * Average Case - O(n^2)
  * Worst Case - O(n^2)

```c++
#include <iostream>
#include <vector>
using namespace std;

void sort(vector<int> &array)
{
    int temp;
    for (int i = 0; i < array.size() - 1; i++)
    {
        for (int j = 1; j < array.size() - i; j++)
        {
            if (array[j] < array[j - 1])
            {
                temp = array[j - 1];
                array[j - 1] = array[j];
                array[j] = temp;
            }
        }
    }
}

int main()
{
    vector<int> array{4, 5, 3, 2, 1};
    sort(array);

    for (auto i : array)
    {
        cout << i << " ";
    }
    cout << endl;
    return 0;
}
```

### Counting Sort
* Time Complexity:
  * Worst Case - O(n + m)
  * Average Case - O(n + m)
  * Best Case - O(n + m)
```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <limits>
using namespace std;

int max(vector<int> array)
{
    int highest = INT_MIN;
    for (int i = 0; i < array.size(); i++)
    {
        highest = max(highest, array[i]);
    }
    return highest;
}

vector<int> sort(vector<int> array)
{
    int len = max(array);
    unordered_map<int, int> map;

    for (auto i : array)
    {
        map[i]++;
    }

    vector<int> res;

    for (int i = 0; i < len + 1; i++)
    {
        while (map[i] != 0)
        {
            res.push_back(i);
            map[i]--;
        }
    }
    return res;
}

int main()
{
    vector<int> array{2, 3, 0, 2, 3, 2};
    array = sort(array);

    for (auto i : array)
        cout << i << " ";
    cout << endl;
}
```

### Quick Sort
* Time Complexity
  * Best Case - O(nlogn)
  * Average Case - O(nlogn)
  * Worst Case - O(n^2)
```c++
#include <iostream>
#include <vector>
using namespace std;

void swap(int &p1, int &p2)
{
    int temp = p1;
    p1 = p2;
    p2 = temp;
}

int partition(vector<int> &array, int start, int end)
{
    int pivot = array[end];

    int i = start - 1;

    for (int j = i + 1; j < end; j++)
    {
        if (array[j] <= pivot)
        {
            i++;
            swap(array[i], array[j]);
        }
    }
    swap(array[i + 1], array[end]);

    return i + 1;
}

void sort(vector<int> &array, int start, int end)
{
    if (start < end)
    {
        int p = partition(array, start, end);

        sort(array, start, p - 1);
        sort(array, p + 1, end);
    }
}

int main()
{
    vector<int> array{4, 5, 3, 2, 1};
    sort(array, 0, array.size() - 1);
    for (auto i : array)
    {
        cout << i << " ";
    }
    cout << endl;
    return 0;
}
```

### Merge Sort
* Time Complexity
  * Worst Case - O(nlogn)
  * Best Case - O(nlogn)
  * Average Case - O(nlogn)

```c++
#include <iostream>
#include <vector>
using namespace std;

void merge(vector<int> &array, int low, int mid, int high)
{
    vector<int> left(array.begin() + low, array.begin() + mid + 1);
    vector<int> right(array.begin() + mid + 1, array.begin() + high + 1);

    int i = 0, j = 0, k = low;
    while (i < left.size() && j < right.size())
    {
        if (left[i] < right[j])
            array[k++] = left[i++];
        else
            array[k++] = right[j++];
    }

    while (i < left.size())
        array[k++] = left[i++];

    while (j < right.size())
        array[k++] = right[j++];
}

void sort(vector<int> &array, int low, int high)
{
    if (low < high)
    {
        int mid = (low + high) / 2;

        sort(array, low, mid);
        sort(array, mid + 1, high);

        merge(array, low, mid, high);
    }
}

int main()
{
    vector<int> array{4, 5, 2, 1, 3};
    sort(array, 0, array.size() - 1);

    for (auto i : array)
    {
        cout << i << " ";
    }

    cout << endl;
    return 0;
}
```

### Heap Sort
* Time Complexity
  * Worst Case - O(nlogn)
  * Best Case - O(nlogn)
  * Average Case - O(nlogn)
```c++
#include <iostream>
#include <vector>
using namespace std;

class MaxHeap
{
private:
    vector<int> heap;

    void swap(int &a, int &b)
    {
        int temp = a;
        a = b;
        b = temp;
    }

    void heapifyUp(int index)
    {
        int parent = (index - 1) / 2;

        while (index > 0 && heap[index] > heap[parent])
        {
            swap(heap[index], heap[parent]);
            index = parent;
            parent = (index - 1) / 2;
        }
    }

    void heapifyDown(int index, int size)
    {
        int left = 2 * index + 1;
        int right = 2 * index + 2;
        int largest = index;

        if (left < size && heap[left] > heap[largest])
        {
            largest = left;
        }

        if (right < size && heap[right] > heap[largest])
        {
            largest = right;
        }

        if (largest != index)
        {
            swap(heap[largest], heap[index]);
            heapifyDown(largest, size);
        }
    }

public:
    void insert(int x)
    {
        heap.push_back(x);
        heapifyUp(heap.size() - 1);
    }

    void traverse()
    {
        for (auto i : heap)
        {
            cout << i << " ";
        }
        cout << endl;
    }

    void sort()
    {
        for (int i = 1; i < heap.size(); i++)
        {
            swap(heap[0], heap[heap.size() - i]);
            heapifyDown(0, heap.size() - i);
        }
    }
};

int main()
{
    MaxHeap heap;
    heap.insert(1);
    heap.insert(2);
    heap.insert(3);
    heap.insert(4);
    heap.insert(5);

    heap.traverse();

    heap.sort();
    heap.traverse();
}
```

## Trees
### Binary Search Tree
```c++
#include <iostream>
using namespace std;

struct Node
{
    int value;
    Node *left;
    Node *right;

    Node(int x)
    {
        value = x;
        left = NULL;
        right = NULL;
    }
};

class BST
{
public:
    Node *root;

    BST()
    {
        root = NULL;
    }

    Node *insert(int x, Node *root)
    {
        if (!root)
            return new Node(x);

        if (x < root->value)
        {
            root->left = insert(x, root->left);
        }
        else
        {
            root->right = insert(x, root->right);
        }
        return root;
    }

    void inorder(Node *root)
    {
        if (root)
        {
            inorder(root->left);
            cout << root->value << " ";
            inorder(root->right);
        }
    }

    Node *findMin(Node *root)
    {
        while (root->left)
            root = root->left;
        return root;
    }

    Node *remove(int x, Node *root)
    {
        if (x < root->value)
        {
            root->left = remove(x, root->left);
        }
        else if (x > root->value)
        {
            root->right = remove(x, root->right);
        }
        else
        {
            if (!root->left)
            {
                Node *temp = root->right;
                delete (root);
                return temp;
            }
            else if (!root->right)
            {
                Node *temp = root->left;
                delete (root);
                return temp;
            }
            else
            {
                Node *temp = findMin(root->right);
                root->value = temp->value;
                root->right = remove(temp->value, root->right);
            }
        }
        return root;
    }
};

int main()
{
    BST bst;
    bst.root = bst.insert(2, bst.root);
    bst.root = bst.insert(1, bst.root);
    bst.root = bst.insert(3, bst.root);

    bst.root = bst.remove(3, bst.root);

    bst.inorder(bst.root);
}
```

### Heap Tree
```c++
#include <iostream>
#include <vector>
using namespace std;

class MaxHeap
{
private:
    vector<int> heap;

    void swap(int &a, int &b)
    {
        int temp = a;
        a = b;
        b = temp;
    }

    void heapifyUp(int index)
    {
        int parent = (index - 1) / 2;
        while (index > 0 && heap[index] > heap[parent])
        {
            swap(heap[index], heap[parent]);
            index = parent;
            parent = (index - 1) / 2;
        }
    }

    void heapifyDown(int index)
    {
        int left = 2 * index + 1;
        int right = 2 * index + 2;
        int largest = index;

        if (left < heap.size() && heap[left] > heap[index])
        {
            largest = left;
        }

        if (right < heap.size() && heap[right] > heap[index])
        {
            largest = right;
        }

        if (largest != index)
        {
            swap(heap[index], heap[largest]);
            heapifyDown(largest);
        }
    }

public:
    void insert(int x)
    {
        heap.push_back(x);
        heapifyUp(heap.size() - 1);
    }

    void remove()
    {
        heap[0] = heap.back();
        heap.pop_back();
        heapifyDown(0);
    }

    void traverse()
    {
        for (auto i : heap)
        {
            cout << i << " ";
        }
        cout << endl;
    }
};

int main()
{
    MaxHeap tree;
    tree.insert(1);
    tree.insert(2);
    tree.insert(3);
    tree.insert(4);
    tree.traverse();
}
```

### AVL Tree
```c++
#include <iostream>
using namespace std;

struct Node
{
    int value;
    int height;
    Node *left;
    Node *right;

    Node(int x)
    {
        value = x;
        height = 1;
        left = NULL;
        right = NULL;
    }
};

class AVLTree
{
private:
    int height(Node *root)
    {
        if (!root)
            return 0;
        return root->height;
    }

    int max(int a, int b)
    {
        return (a > b) ? a : b;
    }

    int balance(Node *root)
    {
        if (!root)
            return 0;
        return height(root->left) - height(root->right);
    }

    Node *rightRotate(Node *root)
    {
        Node *t1 = root->left;
        Node *t2 = t1->right;

        t1->right = root;
        root->left = t2;

        root->height = max(height(root->left), height(root->right)) + 1;
        t1->height = max(height(t1->left), height(t1->right)) + 1;

        return t1;
    }

    Node *leftRotate(Node *root)
    {
        Node *t1 = root->right;
        Node *t2 = t1->left;

        t1->left = root;
        root->right = t2;

        root->height = max(height(root->left), height(root->right)) + 1;
        t1->height = max(height(t1->left), height(t1->right)) + 1;

        return t1;
    }

    Node *findMin(Node *root)
    {
        while (root->left)
            root = root->left;

        return root;
    }

public:
    Node *root;

    AVLTree()
    {
        root = NULL;
    }

    Node *insert(int x, Node *root)
    {
        if (!root)
            return new Node(x);

        if (x < root->value)
        {
            root->left = insert(x, root->left);
        }
        else if (x > root->value)
        {
            root->right = insert(x, root->right);
        }
        else
        {
            return root;
        }

        root->height = max(height(root->left), height(root->right)) + 1;
        int bal = balance(root);

        if (bal > 1 && x < root->left->value)
        {
            return rightRotate(root);
        }
        if (bal < -1 && x > root->right->value)
        {
            return leftRotate(root);
        }
        if (bal > 1 && x > root->left->value)
        {
            root->left = leftRotate(root->left);
            return rightRotate(root);
        }
        if (bal < -1 && x < root->right->value)
        {
            root->right = rightRotate(root->right);
            return leftRotate(root);
        }

        return root;
    }

    Node *remove(int x, Node *root)
    {
        if (!root)
            return root;

        if (x < root->value)
            root->left = remove(x, root->left);
        else if (x > root->value)
            root->right = remove(x, root->right);
        else
        {
            if (!root->left)
            {
                Node *temp = root->right;
                delete (root);
                return temp;
            }

            if (!root->right)
            {
                Node *temp = root->left;
                delete (root);
                return temp;
            }

            Node *temp = findMin(root->right);
            root->value = temp->value;
            root->right = remove(root->value, root->right);
            return root;
        }

        root->height = max(height(root->left), height(root->right)) + 1;
        int bal = balance(root);

        if (bal > 1 && x < root->left->value)
        {
            return rightRotate(root);
        }
        if (bal < -1 && x > root->right->value)
        {
            return leftRotate(root);
        }
        if (bal > 1 && x > root->left->value)
        {
            root->left = leftRotate(root->left);
            return rightRotate(root);
        }
        if (bal < -1 && x < root->right->value)
        {
            root->right = rightRotate(root->right);
            return leftRotate(root);
        }

        return root;
    }

    void inorder(Node *root)
    {
        if (root)
        {
            inorder(root->left);
            cout << root->value << " ";
            inorder(root->right);
        }
    }
};

int main()
{
    AVLTree tree;

    tree.root = tree.insert(1, tree.root);
    tree.root = tree.insert(2, tree.root);
    tree.root = tree.insert(0, tree.root);
    tree.root = tree.insert(3, tree.root);

    tree.inorder(tree.root);
}
```

### Red Black Tree 
* Assures O(nlogn) for it's operations just like AVL trees
* But AVL Trees take many rotations at time when the tree is very huge
* Red Black trees achieve the same time complexity with fewer rotations or at times recolourations

```c++
#include <iostream>
using namespace std;

enum Color
{
    RED,
    BLACK
};

struct Node
{
    int value;
    Color color;
    Node *left, *right, *parent;

    Node(int data) : value(data), color(RED), left(NULL), right(NULL), parent(NULL) {}
};

class RedBlackTree
{
private:
    Node *root;

    void rotateLeft(Node *&root, Node *&pt)
    {
        cout << "Rotating Left" << endl;
        Node *pt_right = pt->right;
        pt->right = pt_right->left;

        if (pt->right != NULL)
        {
            pt->right->parent = pt;
        }

        pt_right->parent = pt->parent;

        if (pt->parent == NULL)
        {
            root = pt_right;
        }
        else if (pt->parent->right == pt)
        {
            pt->parent->right = pt_right;
        }
        else if (pt->parent->left == pt)
        {
            pt->parent->left = pt_right;
        }

        pt_right->left = pt;
        pt->parent = pt_right;
    }

    void rotateRight(Node *&root, Node *&pt)
    {
        cout << "Rotating Right" << endl;
        Node *pt_left = pt->left;
        pt->left = pt_left->right;

        if (pt->right != NULL)
        {
            pt->right->parent = pt;
        }

        pt_left->parent = pt->parent;

        if (pt->parent == NULL)
        {
            root = pt_left;
        }
        else if (pt->parent->left == pt)
        {
            pt->parent->left = pt_left;
        }
        else if (pt->parent->right == pt)
        {
            pt->parent->right = pt_left;
        }

        pt_left->right = pt;
        pt->parent = pt_left;
    }

    void fixViolations(Node *&root, Node *&pt)
    {
        Node *parent_pt = NULL;
        Node *grandparent_pt = NULL;

        while ((pt != root) && (pt->color != BLACK) && pt->parent->color == RED)
        {
            parent_pt = pt->parent;
            grandparent_pt = parent_pt->parent;

            if (parent_pt == grandparent_pt->left)
            {
                Node *uncle_ptr = grandparent_pt->right;

                if (uncle_ptr != NULL && uncle_ptr->color == RED)
                {
                    grandparent_pt->color = RED;
                    parent_pt->color = BLACK;
                    uncle_ptr->color = BLACK;

                    pt = grandparent_pt;
                }
                else
                {
                    if (pt == parent_pt->right)
                    {
                        rotateLeft(root, parent_pt);
                        pt = parent_pt;
                        parent_pt = pt->parent;
                    }

                    rotateRight(root, grandparent_pt);
                    swap(parent_pt->color, grandparent_pt->color);
                    pt = parent_pt;
                }
            }
            else
            {
                Node *uncle_ptr = grandparent_pt->left;

                if (uncle_ptr != NULL && uncle_ptr->color == RED)
                {
                    grandparent_pt->color = RED;
                    parent_pt->color = BLACK;
                    uncle_ptr->color = BLACK;

                    pt = grandparent_pt;
                }
                else
                {
                    if (pt == parent_pt->left)
                    {
                        rotateRight(root, parent_pt);
                        pt = parent_pt;
                        parent_pt = pt->parent;
                    }

                    rotateLeft(root, grandparent_pt);
                    swap(parent_pt->color, grandparent_pt->color);
                    pt = parent_pt;
                }
            }
        }

        root->color = BLACK;
    }

public:
    RedBlackTree() { root = NULL; }

    void insert(int x)
    {
        Node *pt = new Node(x);
        root = BSTInsert(root, pt);

        fixViolations(root, pt);
    }

    Node *BSTInsert(Node *root, Node *pt)
    {
        if (root == NULL)
        {
            return pt;
        }

        if (pt->value < root->value)
        {
            root->left = BSTInsert(root->left, pt);
            root->left->parent = root;
        }
        else if (pt->value > root->value)
        {
            root->right = BSTInsert(root->right, pt);
            root->right->parent = root;
        }

        return root;
    }

    void inorder()
    {
        inorderHelper(root);
    }

    void inorderHelper(Node *root)
    {
        if (root != NULL)
        {
            inorderHelper(root->left);
            cout << root->value << " ";
            inorderHelper(root->right);
        }
    }
};

int main()
{
    RedBlackTree tree;

    tree.insert(10);
    tree.insert(18);
    tree.insert(7);
    tree.insert(15);
    tree.insert(16);

    tree.inorder();
}
```

