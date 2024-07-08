# DSA
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

