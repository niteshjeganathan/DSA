# DSA
## Index
* [Algorithm Analysis](#algorithm-analysis)
* [Stack and its applications](#stack)
* [Queue and its types](#queue)
* [Linked List](#linked-list)
* [Linear Search](#linear-search)
* [Binary Search](#binary-search)
* [Insertion Sort](#insertion-sort)
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
