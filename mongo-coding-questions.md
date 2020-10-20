# Coding Question MongoDB

[TOC]



## 1. Reverse a number without using strings

```
Input:  num
(1) Initialize rev_num = 0
(2) Loop while num > 0
     (a) Multiply rev_num by 10 and add remainder of num  
          divide by 10 to rev_num
               rev_num = rev_num*10 + num%10;
     (b) Divide num by 10
(3) Return rev_num
```

```c++
#include <bits/stdc++.h> 
  
using namespace std; 
/* Iterative function to reverse digits of num*/
int reversDigits(int num) 
{ 
    int rev_num = 0; 
    while(num > 0) 
    { 
        rev_num = rev_num*10 + num%10; 
        num = num/10; 
    } 
    return rev_num; 
} 
  
/*Driver program to test reversDigits*/
int main() 
{ 
    int num = 4562; 
    cout << "Reverse of no. is "
         << reversDigits(num); 
    getchar(); 
    return 0; 
} 
```

```python
rev = 0
  
while(n > 0): 
    a = n % 10
    rev = rev * 10 + a 
    n = n // 10
      
print(rev) 
```

## 2. Design a high traffic forum/website

There are many considerations when you are designing a high traffic website.

- *Use SSD for r/w speed*: What type of server you are using? If you are using server with array of SSD disks that would help you a lot
- *CDN the static data*: Check if the data you are retrieving from the database can me made static. if yes then do it.
- *Cache data using Redis*: If the data you are fetching from database changes infrequently than cache it and fine tune caching mechanism
- For the dynamic data, optimize the queries in the database
- If you have multiple css, combine into one css so that number of requests reduces
- If you have multiple JavaScript, combine them into one to reduce number of requests
- If your images are not changing dynamically on the pages, cache them so that there loading is fast.
- The same applies to JavaScript and css, you can cache them.
- Check that you don’t have info and debug logs enabled on production environment. They are good on development and test environment
- Selection of type of database means relational or non relational

---------------------



### What is a High Traffic Website?

Tags: [Systems Design](https://theartofmachinery.com/tags/systems/) , [Servers](https://theartofmachinery.com/tags/servers/) and [Performance](https://theartofmachinery.com/tags/performance/)

Terms like “high traffic” are hazardous when designing online services because salespeople, business analysts and engineers all have different perspectives about what they mean. If we’re talking about, say, a high-stakes online poker room, then “high traffic” for the business side will be very low compared to what it is for the technical side. However, all these people will be in a meeting room together making decisions, using the same words to mean different things. It’s obvious how that can lead to bad (and sometimes expensive) choices.

A lot of my day job is talking to business stakeholders and figuring out the technical solutions they need, so this is a problem I have to deal with. So I’ve got my own purely technical way to think about traffic levels for online services.



#### Scalability vs performance

First, let’s be clear about two concepts that come up a lot in online service design.

For online services, *performance is all about how well (usually how fast) the system can handle a single request or unit of work.* *Scalability is about the volume or size of work that can be handled*. For online services, scalability is usually about the number of user requests that can be handled within a timeframe, while for batch jobs we typically care about the size of the dataset we can process. Sometimes we want the system capacity to grow and shrink based on demand, but sometimes we don’t care as long as we can handle the full range of workloads we expect.

Scalability and performance often get confused because they commonly work together. For example, suppose you have some online service using a slow algorithm and you improve performance by replacing the algorithm with another that does the same job with less work. Primarily that’s a performance gain, but as long as the new algorithm doesn’t use more memory or something, you’ll be able to handle more requests in the same time.

There’s a counterexample I’ve discussed before: [internal caches versus external cache servers](https://theartofmachinery.com/2016/07/30/server_caching_architectures.html). Using an external cache server like Redis means your app has to make network calls to do cache lookups, so it has performance overhead. On the other hand, if *your app is replicated across multiple machines, a shared, external cache is more effective than a per-app, in-memory cache.* The external cache is more scalable.

#### Response time/Latency

When designing systems, it’s helpful to start by thinking about latency or response time requirements, even if we have to make some up and revise them later. Adding more RAM, more caches, more machines or more disk can solve a lot of problems, but latency problems tend to be fundamental to the system design. For example, suppose you’re designing an online game, and you want latencies of under 100ms for all users. Straight away, the speed of light limit means you can’t have one central server supporting a global game, regardless of whatever algorithms or hardware you throw at the problem.

There’s another reason it’s useful to focus on server response time in practice. If you have a simple, single-function website, such as a mortgage calculator, then the response time can be estimated based on technical things like the hardware specs and code quality. But that’s not how typical online services are built. The online service industry tends to emphasize adding more and more features for less development cost. That means [webpages tend to expand in complexity](https://mobiforge.com/research-analysis/the-web-is-doom) using the easiest code possible, and only get optimized when they become too slow and users churn. Even the typical mortgage calculator site will end up bloated with advertising and tracking functionality. So the response time of a website in my day job depends mostly on the client’s budget and priorities, not on technical factors, regardless of whether it’s an ecommerce site or a cutting-edge data application.

#### Looking at a single worker

Okay, so now imagine a simple web app that gets about one request an hour that takes about 5s to process (ignoring static assets because they’re not the bottleneck). That app has an obvious performance problem because many users will give up before 5s. But there’s no scaling problem because the server will practically never hit capacity limits and drop requests. Even if traffic rises, the performance problem is the bottleneck that takes priority over any hypothetical scalability problems.

That’s a simple insight that we can take further. Lets say we target 100ms per request, and our simple web app processes requests one at a time serially (i.e., no scaling). With 86,400 seconds in a day, a naïve calculation says we can handle 86,400 / 0.1 = 864,000 requests per day before we have scaling problems.

Of course, the real world isn’t that simple.

First, there will be slower and faster requests, and [requests that arrive at random won’t balance themselves nicely](https://theartofmachinery.com/2020/01/27/systems_programming_probability.html). They’ll come in clusters that fill up queues, and the backlog will cause large spikes in response time. (There’s a handy rule that says [if you want to keep response time under control, you should target about 80% usage of theoretical total capacity](https://www.johndcook.com/blog/2009/01/30/server-utilization-joel-on-queuing/).)

Then there’s diurnal variation. Some local websites get nearly all of their traffic during business hours, or about a third of the day. Even a very global website can easily have 2-3 times more traffic at peak than at trough because populations aren’t distributed evenly around the world (a lot of internet users live in East Asia and North/South America, but not in the huge Pacific ocean). The actual ratio depends on many factors that are hard to predict.

But even if we can’t easily get exact capacity estimates, this simple model is the justification for splitting websites into three traffic levels.

#### The three traffic levels

The first level is for sites that get well under 100k dynamic requests a day. Most websites are at this level, and a lot will stay that way while being totally useful and successful. They can also pose complex technical challenges, both for performance and functionality, and a lot of my work is on sites this size. But they don’t have true scalability problems (as opposed to problems that can be solved purely by improving performance).

If a website gets bigger, it can get into the “growing pains” level, which is roughly around 100k-1M dynamic requests a day. This is where scalability problems start to appear. Often the site is at least a bit scalable (thanks to, e.g., async or multithreaded programming), but Web developers scaling a site through this level keep discovering new surprise pain points. Things that a smaller site can get away with start turning into real problems in this level. Some of the biggest challenges are actually sociotechnical, with the team that builds and manages the site needing to learn to think about it in new ways.

The next level is after leaving the 1M dynamic requests a day boundary behind. I think of sites at this level as being high traffic. Let me stress that that’s a technical line, not a value judgment or ego statement. The world’s biggest websites are orders of magnitude bigger than that, while most of the world’s useful websites are smaller. But the line matters because you simply can’t run a site at that scale without treating it like a high traffic site. You can get away with it at low traffic levels, you can fumble through it at the growing pains level, but at high traffic levels you just have to work differently. Coincidentally, it’s around this traffic level where it makes more sense to talk about requests per second than requests per day.

By the way, don’t focus too much on the exact traffic levels above. They’re very rough and honestly I picked them because they’re convenient round numbers that happen to be reasonable for typical websites. The real values depend on the target response time and all the other factors, of course. What I’m trying to explain is 1) that these levels exist, 2) why they exist and 3) what to expect if you’re trying to grow an online service.

#### Going to more levels

What happens with sites that get even bigger? Once the problems at one set of bottlenecks are fixed, the site should just scale until it hits a new set of bottlenecks, either because the application has changed significantly, or just because of a very large increase in traffic. For example, once the application servers are scalable, the next scaling bottleneck could be database reads, followed by database writes. However, the basic ideas are the same, just applied to a different part of the system.

Working on a very high-traffic site can be a lot less “exciting” than working on a plain high-traffic site, simply because most major problems need to be solved to get to very high traffic levels in the first place.

#### Scaling when you don’t have scaling problems

Some developers try to make online services scalable long before they have any scalability problems on the horizon, usually by adding exotic databases or broker servers or something. In particular, startup founders are often especially concerned that their technical assets might not scale to meet their business ambitions. It’s understandable, but it’s a dangerous trap for a couple of reasons.

One is that Paul Graham’s classic [Do Things That Don’t Scale](http://www.paulgraham.com/ds.html) essay applies to your technology stack, too. You can’t beat bigger companies with scale, but your competitive advantage is that you can choose to *not* solve the scalability problems that bigger companies are forced to with every step they take. That’s what makes smaller companies agile, and a startup that worries too much about scalability is just a big enterprise without the big to back it up.

The other problem is that premature scalability solutions can easily backfire. If you don’t have real scalability problems to test your solutions against, it’s hard to be sure you’re correctly solving a real problem. In fact, rapidly growing services tend to change requirements rapidly, too, so the risk of a scalability “solution” turning into technical debt is high. If you keep trying to add scalability to a part of the system that’s already scalable enough, chances are the next scaling bottleneck will appear somewhere else, anyway.

Architectures that err on the side of too simple are easier to scale in the long run than architectures that are too complex.

To be more concrete, I personally can’t think of a low-traffic online service I’ve worked on that couldn’t have been implemented cleanly enough using a simple, monolithic web app (in whatever popular language) in front of [a boring relational database](https://theartofmachinery.com/2017/10/28/rdbs_considered_useful.html), maybe with a search index like Xapian or Elasticsearch. [Many major websites aren’t much different from that.](https://nickcraver.com/blog/2016/02/17/stack-overflow-the-architecture-2016-edition/) It’s not the only valid architecture, but it’s a tried-and-tested one.

Having said all that, sometimes low-traffic sites need things that are sometimes sold as scalability solutions. For example, replicating an app behind a load balancer can help you deploy whenever you want without downtime. One fintech service I worked on split credit card code into its own service, making PCI DSS compliance simpler. In all these cases there’s a clear problem other than scalability that’s being solved, and that’s what avoids overengineering.

I often wish I had a systematic way to just figure out all the technical requirements for an online service in my head. But the real world is complicated and messy, and sometimes the only practical way to be sure is to experiment. However, every piece of software starts with ideas, and this is how I think about scalability for online service ideas during the early design phase.

## Web App System design considerations:

- Security (CORS)
- Using CDN
  - A content delivery network (CDN) is a system of distributed servers (network) that deliver webpages and other Web content to a user based on the geographic locations of the user, the origin of the webpage and a content delivery server.
  - This service is effective in speeding the delivery of content of websites with high traffic and websites that have global reach. The closer the CDN server is to the user geographically, the faster the content will be delivered to the user.
  - CDNs also provide protection from large surges in traffic.
- Full Text Search
  - Using Sphinx/Lucene/Solr - which achieve fast search responses because, instead of searching the text directly, it searches an index instead.
- Offline support/Progressive enhancement
  - Service Workers
- Web Workers
- Server Side rendering
- Asynchronous loading of assets (Lazy load items)
- Minimizing network requests (Http2 + bundling/sprites etc)
- Developer productivity/Tooling
- Accessibility
- Internationalization
- Responsive design
- Browser compatibility



## 3 Software engineering best practices 

### What are the best practices in software engineering?

- Develop iteratively
- Manage requirements
- Use component architecture
- Model software visually
- Verify quality
- Control change

Best practices are a set of empirically proven approaches to software development. When used in combination they strike at the root causes of software development problems. They are called "best practices" not because we can precisely quantify their value but rather they are observed to be commonly used in industry by successful organizations.

#### Best Practices address the root causes of poor software development

##### 1. Develop Iteratively

- Critical risks are resolved before making large investments
- Initial iterations enable early user feedback
- Testing and integration are continuous
- Objective milestones provide short term focus

##### 2. Manage Requirements

- Requirements are dynamic - expect them to change during software development
- User's own understanding of the requirements evolves over time
- Gain agreement with user on what the system should do and not how
- Maintain forward and backward traceability of requirements

##### 3. Use Component Based Architecture

- Using components permits reuse
- Choice of thousands of commercially available components
- Improved maintainability and extensibility
- Promotes clean division of work among teams of developers

##### 4. Visually Model Software

- Visual modeling improves our ability to manage software complexity
- Capture the structure and behavior of components
- Hide or expose details as appropriate for the task
- Promote unambiguous communication

##### 5. Verify Software Quality

- What is quality? - The characteristic of producing a product which meets or exceeds agreed upon requirements by some agreed upon objective measures.
- Software problems are 100 to 1000 times more costly to find and repair after deployment
- Develop test suites for each iteration and test for -
  - Functionality
  - Reliability
  - Performance

##### 6. Control Changes to Software

- Without explicit control parallel development degrades to chaos
- Decompose the architecture into subsystems and assign responsibility of each subsystem to a team. Establish secure workspaces for each team i.e. each team is isolated from changes made in other workspaces.
- Establish an enforceable change control mechanism where
  - Change requests are prioritized
  - Impact of the change request is assessed
  - Plan put in place to introduce change in a particular iteration

##### Best Practices Reinforce Each Other

Each of the best practices reinforces and enables the others. Although it is possible to use one best practice without the others, it is not recommended. The whole is much greater than the sum of the parts.



## 4 Linked list operations

```c++
// Definition for singly-linked list.
struct SinglyListNode {
    int val;
    SinglyListNode *next;
    SinglyListNode(int x) : val(x), next(NULL) {}
};

class MyLinkedList {
private:
    SinglyListNode *head;
public:
    /** Initialize your data structure here. */
    MyLinkedList() {
        head = NULL;
    }
};

/** Helper function to return the index-th node in the linked list. */
SinglyListNode* getNode(int index) {
    SinglyListNode *cur = head;
    for (int i = 0; i < index && cur; ++i) {
        cur = cur->next;
    }
    return cur;
}
/** Helper function to return the last node in the linked list. */
SinglyListNode* getTail() {
    SinglyListNode *cur = head;
    while (cur && cur->next) {
        cur = cur->next;
    }
    return cur;
}

/** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
void addAtHead(int val) {
    SinglyListNode *cur = new SinglyListNode(val);
    cur->next = head;
    head = cur;
    return;
}

/** Append a node of value val to the last element of the linked list. */
void addAtTail(int val) {
    if (head == NULL) {
        addAtHead(val);
        return;
    }
    SinglyListNode *prev = getTail();
    SinglyListNode *cur = new SinglyListNode(val);
    prev->next = cur;
}

/** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
void addAtIndex(int index, int val) {
    if (index == 0) {
        addAtHead(val);
        return;
        }
    SinglyListNode *prev = getNode(index - 1);
    if (prev == NULL) {
        return;
    }
    SinglyListNode *cur = new SinglyListNode(val);
    SinglyListNode *next = prev->next;
    cur->next = next;
    prev->next = cur;
}
/*It is worth noting that we have to get the (index - 1)th node or the last node before we add the new node (except adding at the head) and it takes O(N) time to get a node by index, where N is the length of the linked list. It is different from adding a new node after a given node.
*/

/** Delete the index-th node in the linked list, if the index is valid. */
void deleteAtIndex(int index) {
    SinglyListNode *cur = getNode(index);
    if (cur == NULL) {
        return;
    }
    SinglyListNode *prev = getNode(index - 1);
    SinglyListNode *next = cur->next;
    if (prev) {
        prev->next = next;
    } else {
        // modify head when deleting the first node.
        head = next;
    }
}
/*Similar to the add operation, it takes O(N) time to get the node by the index which is different from deleting a given node. However, even if we already get the node we want to delete, we still have to traverse to get its previous node.*/
```

```python
class Node(object):

    def __init__(self, val):
        self.val = val
        self.next = None


class MyLinkedList(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.head = None
        self.size = 0

    def get(self, index):
        """
        Get the value of the index-th node in the linked list. If the index is invalid, return -1.
        :type index: int
        :rtype: int
        """
        if index < 0 or index >= self.size:
            return -1

        if self.head is None:
            return -1

        curr = self.head
        for i in range(index):
            curr = curr.next
        return curr.val

    def addAtHead(self, val):
        """
        Add a node of value val before the first element of the linked list.
        After the insertion, the new node will be the first node of the linked list.
        :type val: int
        :rtype: void
        """
        node = Node(val)
        node.next = self.head
        self.head = node

        self.size += 1

    def addAtTail(self, val):
        """
        Append a node of value val to the last element of the linked list.
        :type val: int
        :rtype: void
        """
        curr = self.head
        if curr is None:
            self.head = Node(val)
        else:
            while curr.next is not None:
                curr = curr.next
            curr.next = Node(val)

        self.size += 1

    def addAtIndex(self, index, val):
        """
        Add a node of value val before the index-th node in the linked list.
        If index equals to the length of linked list, the node will be appended to the end of linked list.
        If index is greater than the length, the node will not be inserted.
        :type index: int
        :type val: int
        :rtype: void
        """
        if index < 0 or index > self.size:
            return

        if index == 0:
            self.addAtHead(val)
        else:
            curr = self.head
            for i in range(index - 1):
                curr = curr.next
            node = Node(val)
            node.next = curr.next
            curr.next = node

            self.size += 1

    def deleteAtIndex(self, index):
        """
        Delete the index-th node in the linked list, if the index is valid.
        :type index: int
        :rtype: void
        """
        if index < 0 or index >= self.size:
            return

        curr = self.head
        if index == 0:
            self.head = curr.next
        else:
            for i in range(index - 1):
                curr = curr.next
            curr.next = curr.next.next

        self.size -= 1
```

```python
class Node:
    def __init__(self, val=None, pre=None, post=None):
        self.val, self.pre, self.post = val, pre, post

class MyLinkedList:

    def __init__(self):
        self.head = self.tail = Node()
        self.head.post, self.tail.pre = self.tail, self.head
        self.size = 0
        
    def add_after(self, pre, val):
        new_node = Node(val)
        new_node.pre, new_node.post = pre, pre.post
        pre.post = new_node.post.pre = new_node
        self.size += 1
    
    def remove(self, node):
        node.pre.post, node.post.pre = node.post, node.pre
        self.size -= 1
        
    def forward(self, start, end, ptr):
        for _ in range(start, end): ptr = ptr.post
        return ptr
    
    def backward(self, start, end, ptr):
        for _ in range(start, end, -1): ptr = ptr.pre
        return ptr

    def get(self, index):
        if 0 <= index <= self.size // 2: return self.forward(-1, index, self.head).val
        elif self.size // 2 < index < self.size: return self.backward(self.size, index, self.tail).val
        return -1
        
    def addAtHead(self, val):
        self.add_after(self.head, val)
        
    def addAtTail(self, val):
        self.add_after(self.tail.pre, val)
    
    def addAtIndex(self, index, val):
        if 0 <= index <= self.size // 2: self.add_after(self.forward(0, index, self.head), val)
        elif self.size // 2 < index <= self.size: self.add_after(self.backward(self.size, index-1, self.tail), val)

    def deleteAtIndex(self, index: int):
        if 0 <= index <= self.size // 2: self.remove(self.forward(-1, index, self.head))
        elif self.size // 2 < index < self.size: self.remove(self.backward(self.size, index, self.tail))
```

### Doubly linked list

```python
from typing import Union

class Node:
    def __init__(self, val):
        self.val = val
        self.prev = None
        self.next = None
        
class MyLinkedList:

    def __init__(self):
        self.head = Node(-1)
        self.tail = Node(-1)
        self.head.next = self.tail
        self.tail.prev = self.head
        self.size = 0
    
    def getNode(self, index: int) -> Union['Node', None]:
        if index == self.size:
            return self.tail
        if index < self.size:
            cur = self.head.next
            for _ in range(index):
                cur = cur.next
            return cur

    def get(self, index: int) -> int:
        return node.val if (node := self.getNode(index)) else -1
        

    def addAtHead(self, val: int) -> None:
        self.addAtIndex(0, val)
        

    def addAtTail(self, val: int) -> None:
        self.addAtIndex(self.size, val)
        

    def addAtIndex(self, index: int, val: int) -> None:
        if (node := self.getNode(index)):
            newNode = Node(val)
            newNode.next = node
            newNode.prev = node.prev
            node.prev.next = newNode
            node.prev = newNode
            self.size += 1
        

    def deleteAtIndex(self, index: int) -> None:
        if (node := self.getNode(index)) and node != self.tail:
            node.prev.next = node.next
            node.next.prev = node.prev
            self.size -= 1
```



## 5 Python Nested Dictionary

##### In this article, you’ll learn about nested dictionary in Python. More specifically, you’ll learn to create nested dictionary, access elements, modify them and so on with the help of examples.

In Python, a dictionary is an unordered collection of items. For example:

```python
dictionary = {'key' : 'value',
'key_2': 'value_2'}
```

Here, dictionary has a `key:value` pair enclosed within curly brackets `{}`.

To learn more about dictionary, please visit [Python Dictionary](https://www.programiz.com/python-programming/dictionary).

------

#### What is Nested Dictionary in Python?

In Python, a nested dictionary is a dictionary inside a dictionary. It's a collection of dictionaries into one single dictionary.

```python
nested_dict = { 'dictA': {'key_1': 'value_1'},
                'dictB': {'key_2': 'value_2'}}
```

Here, the nested_dict is a nested dictionary with the dictionary `dictA` and `dictB`. They are two dictionary each having own key and value.

------

#### Create a Nested Dictionary

We're going to create dictionary of people within a dictionary.

##### Example 1: How to create a nested dictionary

```python
people = {1: {'name': 'John', 'age': '27', 'sex': 'Male'},
          2: {'name': 'Marie', 'age': '22', 'sex': 'Female'}}

print(people)
```

When we run above program, it will output:

```python
{1: {'name': 'John', 'age': '27', 'sex': 'Male'}, 2: {'name': 'Marie', 'age': '22', 'sex': 'Female'}}
```

In the above program, people is a nested dictionary. The internal dictionary `1` and `2` is assigned to people. Here, both the dictionary have key `name`, `age` , `sex` with different values. Now, we print the result of people.

------

#### Access elements of a Nested Dictionary

To access element of a nested dictionary, we use indexing `[]` syntax in Python.

##### Example 2: Access the elements using the [] syntax

```python
people = {1: {'name': 'John', 'age': '27', 'sex': 'Male'},
          2: {'name': 'Marie', 'age': '22', 'sex': 'Female'}}

print(people[1]['name'])
print(people[1]['age'])
print(people[1]['sex'])
```

When we run above program, it will output:

```python
John
27
Male
```

In the above program, we print the value of key `name` using i.e. `people[1]['name']` from internal dictionary `1`. Similarly, we print the value of `age` and `sex` one by one.

------

#### Add element to a Nested Dictionary

##### Example 3: How to change or add elements in a nested dictionary?

```python
people = {1: {'name': 'John', 'age': '27', 'sex': 'Male'},
          2: {'name': 'Marie', 'age': '22', 'sex': 'Female'}}

people[3] = {}

people[3]['name'] = 'Luna'
people[3]['age'] = '24'
people[3]['sex'] = 'Female'
people[3]['married'] = 'No'

print(people[3])
```

When we run above program, it will output:

```python
{'name': 'Luna', 'age': '24', 'sex': 'Female', 'married': 'No'}
```

In the above program, we create an empty dictionary `3` inside the dictionary people.



Then, we add the `key:value` pair i.e `people[3]['Name'] = 'Luna'` inside the dictionary `3`. Similarly, we do this for key `age`, `sex` and `married` one by one. When we print the `people[3]`, we get `key:value` pairs of dictionary `3`.

##### Example 4: Add another dictionary to the nested dictionary

```python
people = {1: {'name': 'John', 'age': '27', 'sex': 'Male'},
          2: {'name': 'Marie', 'age': '22', 'sex': 'Female'},
          3: {'name': 'Luna', 'age': '24', 'sex': 'Female', 'married': 'No'}}

people[4] = {'name': 'Peter', 'age': '29', 'sex': 'Male', 'married': 'Yes'}
print(people[4])
```

When we run above program, it will output:

```python
{'name': 'Peter', 'age': '29', 'sex': 'Male', 'married': 'Yes'}
```

In the above program, we assign a dictionary literal to `people[4]`. The literal have keys `name`, `age` and `sex` with respective values. Then we print the `people[4]`, to see that the dictionary `4` is added in nested dictionary people.

------

#### Delete elements from a Nested Dictionary

In Python, we use “ del “ statement to delete elements from nested dictionary.

##### Example 5: How to delete elements from a nested dictionary?

```python
people = {1: {'name': 'John', 'age': '27', 'sex': 'Male'},
          2: {'name': 'Marie', 'age': '22', 'sex': 'Female'},
          3: {'name': 'Luna', 'age': '24', 'sex': 'Female', 'married': 'No'},
          4: {'name': 'Peter', 'age': '29', 'sex': 'Male', 'married': 'Yes'}}

del people[3]['married']
del people[4]['married']

print(people[3])
print(people[4])
```

When we run above program, it will output:

```python
{'name': 'Luna', 'age': '24', 'sex': 'Female'}
{'name': 'Peter', 'age': '29', 'sex': 'Male'}
```

In the above program, we delete the `key:value` pairs of `married` from internal dictionary `3` and `4`. Then, we print the `people[3]` and `people[4]` to confirm changes.

##### Example 6: How to delete dictionary from a nested dictionary?

```python
people = {1: {'name': 'John', 'age': '27', 'sex': 'Male'},
          2: {'name': 'Marie', 'age': '22', 'sex': 'Female'},
          3: {'name': 'Luna', 'age': '24', 'sex': 'Female'},
          4: {'name': 'Peter', 'age': '29', 'sex': 'Male'}}

del people[3], people[4]
print(people)
```

When we run above program, it will output:

```python
{1: {'name': 'John', 'age': '27', 'sex': 'Male'}, 2: {'name': 'Marie', 'age': '22', 'sex': 'Female'}}
```

In the above program, we delete both the internal dictionary `3` and `4` using `del` from the nested dictionary people. Then, we print the nested dictionary people to confirm changes.

------

#### Iterating Through a Nested Dictionary

Using the for loops, we can iterate through each elements in a nested dictionary.

##### Example 7: How to iterate through a Nested dictionary?

```python
people = {1: {'Name': 'John', 'Age': '27', 'Sex': 'Male'},
          2: {'Name': 'Marie', 'Age': '22', 'Sex': 'Female'}}

for p_id, p_info in people.items():
    print("\nPerson ID:", p_id)
    
    for key in p_info:
        print(key + ':', p_info[key])
```

When we run above program, it will output:

```python
Person ID: 1
Name: John
Age: 27
Sex: Male

Person ID: 2
Name: Marie
Age: 22
Sex: Female
```

In the above program, the first loop returns all the keys in the nested dictionary people. It consist of the IDs p_id of each person. We use these IDs to unpack the information p_info of each person.

The second loop goes through the information of each person. Then, it returns all of the keys `name`, `age`, `sex` of each person's dictionary.

Now, we print the key of the person’s information and the value for that key.

------

## Key Points to Remember:

1. Nested dictionary is an unordered collection of dictionary
2. Slicing Nested Dictionary is not possible.
3. We can shrink or grow nested dictionary as need.
4. Like Dictionary, it also has key and value.
5. Dictionary are accessed using key.

## 6 Remove duplicate words from an array

```python
test_list = ['gfg, best, gfg', 'I, am, I', 'two, two, three' ] 
  
# printing original list 
print("The original list is : " + str(test_list)) 
  
# Remove duplicate words from Strings<code></code> in List 
# using loop + set() + split() 
res = [] 
for strs in test_list: 
    res.append(set(strs.split(", "))) 
```

## 7 Valid Palindrome

 considering only alphanumeric characters and ignoring cases.

#### Approach 1: Compare with Reverse

```c++
class Solution {
 public:
  bool isPalindrome(string s) {
    string filtered_string, reversed_string;

    for (auto ch : s) {
      if (isalnum(ch))
        filtered_string += tolower(ch);
    }

    reversed_string.resize(filtered_string.size());
    reverse_copy(filtered_string.begin(), filtered_string.end(),
                 reversed_string.begin());

    return filtered_string == reversed_string;
  }
};
```

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:

        filtered_chars = filter(lambda ch: ch.isalnum(), s)
        lowercase_filtered_chars = map(lambda ch: ch.lower(), filtered_chars)

        filtered_chars_list = list(lowercase_filtered_chars)
        reversed_chars_list = filtered_chars_list[::-1]

        return filtered_chars_list == reversed_chars_list

```

**Complexity Analysis**

- Time complexity : *O*(*n*), in length *n* of the string.

  We need to iterate thrice through the string:

  1. When we filter out non-alphanumeric characters, and convert the remaining characters to lower-case.
  2. When we reverse the string.
  3. When we compare the original and the reversed strings.

  Each iteration runs linear in time (since each character operation completes in constant time). Thus, the effective run-time complexity is linear.

- Space complexity : *O*(*n*), in length *n* of the string. We need O*(*n) additional space to stored the filtered string and the reversed string.

#### Approach 2: Two Pointers

```c++
class Solution {
 public:
  bool isPalindrome(string s) {
    for (int i = 0, j = s.size() - 1; i < j; i++, j--) {
      while (i < j && !isalnum(s[i]))
        i++;
      while (i < j && !isalnum(s[j]))
        j--;

      if (i < j && tolower(s[i]) != tolower(s[j]))
        return false;
    }

    return true;
  }
};
```

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:

        i, j = 0, len(s) - 1

        while i < j:
            while i < j and not s[i].isalnum():
                i += 1
            while i < j and not s[j].isalnum():
                j -= 1

            if i < j and s[i].lower() != s[j].lower():
                return False

            i += 1
            j -= 1

        return True

```

**Complexity Analysis**

- Time complexity : *O*(*n*), in length *n* of the string. We traverse over each character at-most once, until the two pointers meet in the middle, or when we break and return early.
- Space complexity : O(1). No extra space required, at all.



## 7 You have a Go board full of white pieces, black pieces, and empty spots. Given the representation as an array, come up with an program to find all connected clusters of white pieces (two pieces are said to be connected when they are touching vertically or horizontally) 

#### [Depth First Search for percolation to find clusters in Go game](https://codereview.stackexchange.com/questions/101230/depth-first-search-for-percolation-to-find-clusters-in-go-game)

I have some questions about Depth First Search and whether I implemented it correctly. Below is a more thorough discussion. The graph in question is a randomly colored square grid (I use 3 colors). Adjacent vertices are connected if they have the same color. The task was to compute the clusters. DFS seems perfect for the job, but I had some trouble with the implementation. The code below is working now, but I imagine there is still room for improvement.

**One challenge is that in order to compute the clusters, I run a DFS for each new vertex I visit.** If the vertex has not been visited before, I assume it is not part of the previous clusters. Keeping track of all these DFS is giving me some difficult, making the code a bit disorganized.

For a [Go](https://gogameguru.com/) game app, I need to compute the connected clusters of pieces of the same color. This is a straightfoward depth first search, but I am still missing some parts.

Initialize the board. Here I used numpy to generate a random 10 × 10 grid with possible values 0, 1, 2 (representing empty, black and white states). We are looking for connected clusters of 0s, 1s and 2s. This is called [**percolation**](http://introcs.cs.princeton.edu/java/lectures/24percolation-2x2.pdf).

> ```python
> import numpy as np
> 
> n = 10
> board = (3*np.random.random((n,n))).astype(int)
> print board
> 
> [[1 1 0 2 2 0 0 2 2 0]
>  [1 2 2 0 1 1 1 2 2 0]
>  [0 2 0 0 1 0 0 1 0 1]
>  [2 2 2 0 2 2 1 2 2 0]
>  [2 1 1 0 0 2 2 1 2 1]
>  [0 1 2 0 0 0 2 2 2 2]
>  [1 0 0 0 0 2 2 0 0 2]
>  [1 1 2 2 1 0 1 1 2 1]
>  [1 2 2 1 2 2 1 0 0 0]
>  [1 1 0 1 0 0 2 1 1 2]]
> ```

##### DFS

Using a [depth-first search](http://eddmann.com/posts/depth-first-search-and-breadth-first-search-in-python/) I found on the internet I proceed to find the clusters.

> ```python
> def dfs(graph, start):
>     visited, stack = set(), [start]
>     while stack:
>         vertex = stack.pop()
>         if vertex not in visited:
>             visited.add(vertex)
>             stack.extend(graph[vertex] - visited)
>     return visited
> 
> dfs(graph, 'A') # {'E', 'D', 'F', 'A', 'C', 'B'}
> ```

Here I adapt the implementation to my problem. My graph is the 10 × 10 grid and two vertices are connected **if they have the same color**. I have to change the algorithm a bit since I don't really define the graph vertices and edges explicitly.

```python
# http://stackoverflow.com/questions/509211/explain-pythons-slice-notation

visited =  np.zeros((n,n)).astype(int)

cluster = []

for (a,b) in [(a,b) for a in range(n) for b in range(n)][:]:

    if visited[a,b] == 0:

        queue = [[a,b]]
        color = board[a,b]    

        cluster += [[]]

        count = 0

        while queue:

            q = queue.pop()      
            cluster[-1] += [q]

            if visited[q[0],q[1]] == 0:
                visited[q[0],q[1]] = len(cluster)

                for x in [[1,0],[0,1], [-1,0], [0,-1]]:
                    if q[0] + x[0] > -1 and q[-1] + x[-1] > -1 and q[0] + x[0] < n and q[1] + x[1] < n:
                        if visited[q[0] + x[0], q[1] + x[1]] == 0 and board[q[0] + x[0], q[1] + x[1]] == color:
                            queue += [[q[0] + x[0], q[1] + x[1]]]       
            count += 1

            if count > 20:
                break

            print count,

visited
```

Unfortunately I have to add a **break** statement, but at least the program finds the cluster containing `[0, 0]`. Here are all the clusters containing points in the first two:

> ```bash
> [[[0, 0], [1, 0], [2, 0], [2, 1], [2, 2]],
>   [[0, 1], [1, 1], [1, 2]],
>   [[0, 2], [0, 3]],
>   [[0, 3]],
>   [[0, 4]],
>   [[0, 5], [1, 5], [2, 5], [1, 4], [2, 4], [1, 3]],
>   [[0, 6], [1, 6], [1, 7]],
>   [[0, 7]],
>   [[0, 8]],...]
> ```

## Debugging

I am trying to get rid of the break statement. It is very bad style. Restricting only to the first vertex I noticed the `queue` variable does not become empty despite the repeated `q = queue.pop()` statement. Why does why does that variable stay populated?

Definintely open to other implementations of the graph and DFS. Thank you so much for your help.

------

## Progress

Here's simplified version of the error I found:

> ```py
> a      = [5] 
> b      = [a]
> b[-1] += [5]
> ...
> a      = [5,5]
> ```

Even though we changed the variable `b`, the variable `a` has also changed.

Defining `cluster += [[]]` instead of `cluster += [queue]` has worked.

Another toy example `[1, 2, 3].pop() = 3` so we pop off the last item - I thought it was the first.

With those changes the program works to my satisfaction and now it is ready for Code Review.

---------------------------

# [Google Interview Question] Captive Go Stones

[easy](https://interviewperfect.github.io/categories/easy) [graph](https://interviewperfect.github.io/categories/graph) [google](https://interviewperfect.github.io/categories/google)

Apr 10, 2016

## Problem

Here’s a very, very simplified explanation for the game of Go. There are two players, one player plays with white stones, the other with black stones. The objective of the game is have surrounded a larger total area of the board with one’s stones than the opponent. Our questions deals with a simple aspect of the game, that of capturing and removing another player’s stones.

A stone is captured if it, or its group, is surrounded by opposing stones. Opposing stones surround the group of stones by occupying all orthogonally-adjacent points of the group.

Examples:

```bash
------ ------ ------ -------
| x  | |    | | x  | |xooox|
|xox | |    | |xox | |xo ox|
|xo x| | xx | |xox | |xooox|
| xx | |xoox| | x  | | xxx |
------ ------ ------ -------
```

In the first board, the o stones are not surrounded as there is still one spot the x stones need to fill. In the second board, the o stones are surrounded as their north, west, and east edges are occupied by x stones and their south edge is the end of the board. In the third board, the o stones are surrounded. In the last board, even though all the external edges are occupied by x stones, the o stones are not surrounded as there is a center spot that is empty and must be either filled by an o stone or an x stone for the rest of the o stones to be considered surrounded.

We want to write a program that, passed with a coordinate of a white stone and a Go board, we need to determine if the white stone is considered surrounded or not.

## Ideas and solution

We know that a group of stones is surrounded if the group has all edges occupied either by opposing stones or the edge of the board. This means that if there’s a stone in the group that has an edge that is empty, the group of stones is not surrounded. As such, the problem comes down to searching if one such stone in the group exists.

The fact that this problem involves a grid hints at a graph-related solution. We also know that this problem involves some kind of searching. Let’s try the most basic search algorithm, depth first search (DFS).

We would start the DFS from the coordinates of the white stone passed to us. The “children” of the stone (AKA grid positions we will search next) will be its north, south, east, and west neighbouring grid positions. At each new grid position we encounter, we check whether the grid is empty or not. If it’s empty, we know that the whole group of white stones we just processed is not surrounded, and thus the original stone we started searching from is not surrounded. If we encounter a black stone, there’s no point in searching further at this point as we encountered an “edge” of the group. The same applies if we encounter an edge of the board. Finally, one last thing we need to make sure is to keep track of all the grid positions we’ve seen so we do not re-search already searched stones.

As such, we see that DFS solves our problem nice and easily.

## Coded solution (Python)

```python
# white stones are denoted by 2, black stones by 1, empty grid positions by 0

def is_captive(x, y, board):
    # check that board exists
    if len(board) == 0 or len(board[0]) == 0:
        return False
    # check that coordinates passed in is within bounds of board
    if x < 0 or x >= len(board) or y < 0 or y >= len(board[0]):
        return False
    # check that grid passed in is a white stone
    if board[x][y] != 2:
        raise Exception()

    # keep track of already searched grip positions
    seen_set = set()
    seen_set.add((x,y))

    # if any of the white stones right beside us is not a captive, we are not a captive
    return __is_captive(x+1, y, board, seen_set) and \
            __is_captive(x-1, y, board, seen_set) and \
            __is_captive(x, y+1, board, seen_set) and \
            __is_captive(x, y-1, board, seen_set)

def __is_captive(x, y, board, seen_set):
    # check if already seen
    if (x,y) in seen_set:
        return True

    seen_set.add((x,y))

    # check if board edge
    if x < 0 or x >= len(board) or y < 0 or y >= len(board[0]):
        return True

    # check if empty block
    if board[x][y] == 0:
        return False

    # check if black piece
    if board[x][y] == 1:
        return True

    # must be white piece, search all neighbours
    return __is_captive(x+1, y, board, seen_set) and \
            __is_captive(x-1, y, board, seen_set) and \
            __is_captive(x, y+1, board, seen_set) and \
            __is_captive(x, y-1, board, seen_set)

print is_captive(2, 1, [
    [ 0, 1, 0, 0, 0],
    [ 1, 2, 1, 0, 0],
    [ 1, 2, 2, 1, 0],
    [ 1, 2, 1, 0, 0],
    [ 0, 1, 0, 0, 0]
])
```

---------------

#### 130. Surrounded Regions

```python
class Solution(object):
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        if not board or not board[0]:
            return

        self.ROWS = len(board)
        self.COLS = len(board[0])

        # Step 1). retrieve all border cells
        from itertools import product
        borders = list(product(range(self.ROWS), [0, self.COLS-1])) \
                + list(product([0, self.ROWS-1], range(self.COLS)))

        # Step 2). mark the "escaped" cells, with any placeholder, e.g. 'E'
        for row, col in borders:
            self.DFS(board, row, col)

        # Step 3). flip the captured cells ('O'->'X') and the escaped one ('E'->'O')
        for r in range(self.ROWS):
            for c in range(self.COLS):
                if board[r][c] == 'O':   board[r][c] = 'X'  # captured
                elif board[r][c] == 'E': board[r][c] = 'O'  # escaped


    def DFS(self, board, row, col):
        if board[row][col] != 'O':
            return
        board[row][col] = 'E'
        if col < self.COLS-1: self.DFS(board, row, col+1)
        if row < self.ROWS-1: self.DFS(board, row+1, col)
        if col > 0: self.DFS(board, row, col-1)
        if row > 0: self.DFS(board, row-1, col)
```

## 8 How would you make a tic tac toe game with a “smart” computer?

### 794. Valid Tic-Tac-Toe State

### 1275. Find Winner on a Tic Tac Toe Game

### 348. Design Tic-Tac-Toe

### Design Tic-Tac-Toe

Design a Tic-tac-toe game that is played between two players on a n x n grid.

You may assume the following rules:

A move is guaranteed to be valid and is placed on an empty block.

Once a winning condition is reached, no more moves is allowed.

A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.

Example:

```
Given n = 3, assume that player 1 is "X" and player 2 is "O" in the board.

TicTacToe toe = new TicTacToe(3);

toe.move(0, 0, 1); -> Returns 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

toe.move(0, 2, 2); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

toe.move(2, 2, 1); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

toe.move(1, 1, 2); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

toe.move(2, 0, 1); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

toe.move(1, 0, 2); -> Returns 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

toe.move(2, 1, 1); -> Returns 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
```

Follow up: Could you do better than O(n2) per move() operation?

Hint:

Could you trade extra space such that move() operation can be done in O(1)? You need two arrays: int rows[n], int cols[n], plus two variables: diagonal, anti_diagonal.

**Tips:**

**O(1)** Time, **O(n)** extra space. 

The key observation is that in order to win Tic-Tac-Toe you must have the entire row or column. Thus, we don't need to keep track of an entire n^2 board. We only need to keep a count for each row and column. If at any time a row or column matches the size of the board then that player has won.

To keep track of which player, I add one for Player1 and -1 for Player2. There are two additional variables to keep track of the count of the diagonals. Each time a player places a piece we just need to check the count of that row, column, diagonal and anti-diagonal.

**Code:**

```c++
public class TicTacToe {
    private int[] rows;
    private int[] cols;
    private int diagonal;
    private int antiDiagonal;
    /** Initialize your data structure here. */
    public TicTacToe(int n) {
        rows = new int[n];
        cols = new int[n];
    }

    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    public int move(int row, int col, int player) {
        int toAdd = player == 1 ? 1 : -1;
        rows[row] += toAdd;
        cols[col] += toAdd;
        if (row == col) {
            diagonal += toAdd;
        }
        if (row + col == rows.length + 1) {
            antiDiagonal += toAdd;
        }
        int size = rows.length;
        if (Math.abs(rows[row]) == size || 
            Math.abs(cols[col]) == size ||
            Math.abs(diagonal) == size ||
            Math.abs(antiDiagonal) == size) {
                return player;
            }
        return 0;
    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```

----------------

Record the number of moves for each rows, columns, and two diagonals.
For each move, we -1 for each player 1's move and +1 for player 2's move.
Then we just need to check whether any of the recored numbers equal to n or -n.



```python
class TicTacToe(object):

    def __init__(self, n):
        self.row, self.col, self.diag, self.anti_diag, self.n = [0] * n, [0] * n, 0, 0, n
        
    def move(self, row, col, player):
        offset = player * 2 - 3
        self.row[row] += offset
        self.col[col] += offset
        if row == col:
            self.diag += offset
        if row + col == self.n - 1:
            self.anti_diag += offset
        if self.n in [self.row[row], self.col[col], self.diag, self.anti_diag]:
            return 2
        if -self.n in [self.row[row], self.col[col], self.diag, self.anti_diag]:
            return 1
        return 0
```

A game of Tic-Tac-Toe is won by whichever player first places their marks to fill a whole row, column or a diagonal.
Thus, for us to determine whether or not a player has won after a move it's enough to keep track of the number of marks that player has placed before for each row/column/diagonal.

------------------------



Since there are only 2 diagonals in the classic game that contain enough cells for a possible winning situation, we only need 2 variables to keep track of those.
But other people have already provided solutions like that. I was more interested in the general case, though - what if we set the rules so that the winning condition requires *less* than N marks placed, regardless whether they are all adjacent or not? How would we solve the problem optimally then? Storing just 2 variables for diagonals wouldn't do anymore - the number of possibly winning diagonals could be arbitrarily large.



To solve the general case we'd have to memorize the number of marks placed by each player for each diagonal, just like we do with rows and columns.
That brings us to the next issue, though - how do we identify which diaginal/antidiagonal the current cell belongs to? With rows and columns it's obvious - there's just 1 coordinate to keep in mind, but with diagonals it's not immediately obvious.



Let's draw a picture of a 3x3 matrix, and explore the indices of each diagonal and antidiagonal:
![image](https://assets.leetcode.com/users/images/2e38e920-4a78-488a-abeb-6c6f275fc81d_1595061135.2734926.png)



Cells belonging to diagonals have the following coordinates (each line contains coordinates of cells belonging to the same diagonal):



```
(0, 2)
(0, 1) (1, 2)
(0, 0) (1, 1) (2, 2)
(1, 0) (2, 1)
(2, 0)
```



Cells belonging to antidiagonals have the following coordinates:



```
(0, 0)
(0, 1) (1, 0)
(0, 2) (1, 1) (2, 0)
(1, 2) (2, 1)
(2, 2)
```



Let's explore the relationship between coordinates lying on the same diagonal. We can observe that coordinates lying on the same diagonal happen to have exactly the same `row - column` difference:



```
(0, 2) -> (0-2) -> -2
(0, 1) (1, 2) -> (0-1), (1-2) -> -1, -1
(0, 0) (1, 1) (2, 2) -> (0-0), (1-1), (2-2) -> 0, 0, 0
(1, 0) (2, 1) -> (1-0), (2-1) -> 1, 1
(2, 0) -> (2-0) -> 2
```



So we can uniquely identify the diagonal to which a cell belongs by subtracting the column number from the row number of the cell: `row - col`.



What about the antidiagonals though? Using the difference doesn't work in this case, but it turns out that using the *sum* does:



```
(0, 0) -> (0+0) -> 0
(0, 1) (1, 0) -> (0+1), (1+0) -> 1, 1
(0, 2) (1, 1) (2, 0) -> (0+2), (1+1), (2+0) -> 2, 2, 2
(1, 2) (2, 1) -> (1+2), (2+1) -> 3, 3
(2, 2) -> (2+2) -> 4
```



As we can see, each cell belonging to the same antidiagonal now also corresponds to exactly the same value - `row + col`.



Now that we can map cells to diagonals/antidiagonals, we can treat them in the same way we treat rows/columns - each player has 4 arrays, to store the number of marks placed by the plaer for each row/column/diagonal/antidiagonal.



Here's my Python solution using that idea:



```python
from collections import defaultdict

class TicTacToe:

    def __init__(self, n: int):
        """
        Initialize your data structure here.
        """
        self.n = n
        self.placed = [[defaultdict(int) for _ in range(4)] for i in range(3)]        

    def move(self, row: int, col: int, player: int) -> int:
        """
        Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins.
        """
        arr = self.placed[player]
        # Rows
        if arr[0][row] + 1 == self.n:
            return player
        else:
            arr[0][row] += 1
        # Cols
        if arr[1][col] + 1 == self.n:
            return player
        else:
            arr[1][col] += 1
        # Diagonal (uniquely identified by "row - col")
        if arr[2][row - col] + 1 == self.n:
            return player
        else:
            arr[2][row - col] += 1
        # Back diagonal (uniquely identified by "row + col")
        if arr[3][row + col] + 1 == self.n:
            return player
        else:
            arr[3][row + col] += 1
            
        return 0
```



## 9 Design a search engine

### How do search engines work?

Search engines have three primary functions:

1. **Crawl:** Scour the Internet for content, looking over the code/content for each URL they find.
2. **Index:** Store and organize the content found during the crawling process. Once a page is in the index, it’s in the running to be displayed as a result to relevant queries.
3. **Rank:** Provide the pieces of content that will best answer a searcher's query, which means that results are ordered by most relevant to least relevant.

### What is search engine crawling?

[Crawling](https://support.google.com/webmasters/answer/6065802) is the discovery process in which search engines send out a team of robots (known as crawlers or spiders) to find new and updated content. Content can vary — it could be a webpage, an image, a video, a PDF, etc. — but regardless of the format, content is discovered by links.



### What's that word mean?

Having trouble with any of the definitions in this section? Our SEO glossary has chapter-specific definitions to help you stay up-to-speed.

[See Chapter 2 definitions ](https://moz.com/beginners-guide-to-seo/seo-glossary#chapter2)

![Search engine robots, also called spiders, crawl from page to page to find new and updated content.](https://moz.imgix.net/learn/guides/Moz-BGSEO-Ch2-04-1.svg?auto=format&ch=Width&fit=max&q=50&s=76c3c0d3708c9899c1b739a83fc89fa6)

Googlebot starts out by fetching a few web pages, and then follows the links on those webpages to find new URLs. By hopping along this path of links, the crawler is able to find new content and add it to their index called [Caffeine](https://googleblog.blogspot.com/2010/06/our-new-search-index-caffeine.html) — a massive database of discovered URLs — to later be retrieved when a searcher is seeking information that the content on that URL is a good match for.

### What is a search engine index?

Search engines process and store information they find in an index, a huge database of all the content they’ve discovered and deem good enough to serve up to searchers.

#### Search engine ranking

When someone performs a search, search engines scour their index for highly relevant content and then orders that content in the hopes of solving the searcher's query. This ordering of search results by relevance is known as ranking. In general, you can assume that the higher a website is ranked, the more relevant the search engine believes that site is to the query.

It’s possible to block search engine crawlers from part or all of your site, or instruct search engines to avoid storing certain pages in their index. While there can be reasons for doing this, if you want your content found by searchers, you have to first make sure it’s accessible to crawlers and is indexable. Otherwise, it’s as good as invisible.

By the end of this chapter, you’ll have the context you need to work with the search engine, rather than against it!

-----------------------

**Phase 1: Indexing Infrastructure (spend 5 minutes explaining)**

The first phase of implementing Google (or any search engine) is to build an indexer. This is the piece of software that crawls the corpus of data and produces the results in a data structure that is more efficient for doing reads.

To implement this, consider two parts: a crawler and indexer.

The web crawler's job is to spider web page links and dump them into a set. The most important step here is to avoid getting caught in infinite loop or on infinitely generated content. Place each of these links in one massive text file (for now).

Second, the indexer will run as part of a Map/Reduce job. (Map a function to every item in the input, and then Reduce the results into a single 'thing'.) The indexer will take a single web link, retrieve the website, and convert it into an index file. (Discussed next.) The reduction step will simply be aggregating all of these index files into a single unit. (Rather than millions of loose files.) Since the indexing steps can be done in parallel, you can farm this Map/Reduce job across an arbitrarily-large data center.

**Phase 2: Specifics of Indexing Algorithms (spend 10 minutes explaining)**

Once you have stated how you will process web pages, the next part is explaining how you can compute meaningful results. The short answer here is 'a lot more Map/Reduces', but consider the sorts of things you can do:

- For each web site, count the number of incoming links. (More heavily linked-to pages should be 'better'.)
- For each web site, look at how the link was presented. (Links in an < h1 > or < b > should be more important than those buried in an < h3 >.)
- For each web site, look at the number of outbound links. (Nobody likes spammers.)
- For each web site, look at the types of words used. For example, 'hash' and 'table' probably means the web site is related to Computer Science. 'hash' and 'brownies' on the other hand would imply the site was about something far different.

Unfortunately I don't know enough about the sorts of ways to analyze and process the data to be super helpful. But the general idea is *scalable ways to analyze your data*.

**Phase 3: Serving Results (spend 10 minutes explaining)**

The final phase is actually serving the results. Hopefully you've shared some interesting insights in how to analyze web page data, but the question is how do you actually query it? Anecdotally 10% of Google search queries each day have never been seen before. This means you cannot cache previous results.

You cannot have a single 'lookup' from your web indexes, so which would you try? How would you look across different indexes? (Perhaps combining results -- perhaps keyword 'stackoverflow' came up highly in multiple indexes.)

Also, how would you look it up anyways? What sorts of approaches can you use for reading data from *massive* amounts of information quickly? (Feel free to namedrop your favorite NoSQL database here and/or look into what Google's BigTable is all about.) Even if you have an awesome index that is highly accurate, you need a way to find data in it quickly. (E.g., find the rank number for 'stackoverflow.com' inside of a 200GB file.)

**Random Issues (time remaining)**

Once you have covered the 'bones' of your search engine, feel free to rat hole on any individual topic you are especially knowledgeable about.

- Performance of the website frontend
- Managing the data center for your Map/Reduce jobs
- A/B testing search engine improvements
- Integrating previous search volume / trends into indexing. (E.g., expecting frontend server loads to spike 9-5 and die off in the early AM.)



## 10 Garbage collection 

### 10A What is C++ Garbage Collection?

Garbage collection is a memory management technique. It is a separate automatic memory management method which is [used in programming languages](https://www.educba.com/what-is-a-programming-language/) where manual memory management is not preferred or done. In the manual memory management method, the user is required to mention the memory which is in use and which can be deallocated, whereas the garbage collector collects the memory which is occupied by variables or objects which are no more in use in the program. Only memory will be managed by garbage collectors, other resources such as destructors, user interaction window or files will not be handled by the garbage collector.

Few languages need garbage collectors as part of the language for good efficiency. These languages are called as garbage-collected languages. For example, Java, C# and most of the scripting languages needs garbage collection as part of their functioning. Whereas languages such as [C and C++ support](https://www.educba.com/c-vs-c-plus-plus/) manual memory management which works similar to the garbage collector. There are few languages that support both garbage collection and manually managed memory allocation/deallocation and in such cases, a separate heap of memory will be allocated to the garbage collector and manual memory.

Some of the bugs can be prevented when the garbage collection method is used. Such as:

- **dangling pointer problem** in which the memory pointed is already deallocated whereas the pointer still remains and points to different reassigned data or already deleted memory
- the problem which occurs when we try to delete or deallocate a memory second time which has already been deleted or reallocated to some other object
- removes problems or bugs associated with data structures and does the memory and data handling efficiently
- **memory leaks** or memory exhaustion problem can be avoided

Let’s see a detailed understanding of manual memory management vs garbage collection, advantages, disadvantages and how it is implemented in C++.

#### Manual Memory Management

Dynamically allocated memory during run time from the heap needs to be released once we stop using that memory. Dynamically allocated memory takes memory from the heap, which is a free store of memory.

In C++ this memory allocation and deallocation are done manually using commands like **new, delete**. Using ‘new’ memory is allocated from the heap. After its usage, this memory must be cleared using the ‘delete’ command.

Every memory allocation with ‘new’ must be finished with a ‘delete’ command. If not we will go out of memory.

**To show it clearly with an example:**

n = new sample_object;
******* usage is implemented here*******
delete n;

As shown, every new should end or incline with a delete command. Here n pointer is allocated memory using ‘new’ command and is referenced or pointed to an object called ‘sample_object’. Once the usage and functioning of the pointer are completed, we should release or free the memory using the ‘delete’ command as done above.

But in case of garbage collection, the memory is allocated using ‘new’ command but it need not be manually released using ‘delete’. In such cases, the garbage collector runs periodically checking for free memory. When a piece of memory is not pointed by any object it clears or releases the memory creating more free heap space.

#### Advantages and Disadvantages of Manual Memory Management

Advantages of manual memory management are that the user would have *complete control* over both allocating and deallocating operations and also know when a new memory is allocated and when it is deallocated or released. But in the case of garbage collection, at the exact same instance after the usage the memory will not be released it will get released when it encounters it during the periodic operation.

Also in the case of manual memory management, the destructor will be called at the same moment when we call the ‘delete’ command. But in case of garbage collector that is not implemented.

There are a few issues associated with using manual memory management. Sometimes we might tend to double delete the memory occupied. When we delete the already deleted pointer or memory, there are chances that the pointer might be referencing some other data and can be in use.

Another issue which we have in manual memory management is, if we get an exception during the execution or usage of the new memory allocated pointer, it will go out of the sequence of ‘new’ and ‘delete’ and the release operation will not be performed. Also, there can be issues of memory leak.

#### Advantages and Disadvantages of Garbage Collector

One major disadvantage of garbage collection is the time involved or CPU cycles involved to find the unused memory and deleting it, even if the user knows which pointer memory can be released and not in use. Another disadvantage is, we will not know the time when it is deleted nor when the destructor is called.

#### Garbage Collection Algorithm

There are many garbage collection algorithms such as *reference counting, mark, and sweep, copying,* etc. Let’s see one algorithm in detail for better understanding. For example, when we see the reference counting algorithm, each dynamic memory will have a reference count. When a reference is created the reference count will increase and whenever a reference is deleted the reference count is decremented. Once the reference count becomes zero it shows that the memory is unused and can be released.

This algorithm can be implemented in C++ using a specific pointer type. A specific pointer type should be declared and this can be used for purposes such as keeping track of all the references created, keeping track of the reference count when reference is created and deleted. A C++ program can contain both manual memory management and garbage collection happening in the same program. According to the need, either the normal pointer or the specific garbage collector pointer can be used.

Thus, to sum up, garbage collection is a method opposite to manual memory management. In a garbage collector, the memory is released automatically based on a periodic time basis or based on specific criteria which tells if it is no more in use. Both methods have their own advantages and disadvantages. This can be implemented and used according to the complexity of the function, depending on the language used and its scope.

--------------

### **10B R**esource **A**cquisition **I**s **I**nitialization

RAII stand for **R**esource **A**cquisition **I**s **I**nitialization. Probably, the most important idiom in C++ says that a resource should be acquired in the constructor of the object and released in the destructor of the object. The key is that the destructor will be automatically called if the object goes out of scope. If this is not totally deterministic? In Java or Python (__del__) you have a destructor but not the guarantee. Therefore, it can end disastrous, if you use the destructor to release a critical resource like a lock. That's a classical anti pattern for a deadlock. But in C++, we are on the safe side.

The example shows the totally deterministic behavior of RAII in C++.

```c++
// raii.cpp

#include <iostream>
#include <new>
#include <string>

class ResourceGuard{
  private:
    const std::string resource;
  public:
    ResourceGuard(const std::string& res):resource(res){
      std::cout << "Acquire the " << resource << "." <<  std::endl;
    }
    ~ResourceGuard(){
      std::cout << "Release the "<< resource << "." << std::endl;
    }
};

int main(){

  std::cout << std::endl;

  ResourceGuard resGuard1{"memoryBlock1"};

  std::cout << "\nBefore local scope" << std::endl;
  {
    ResourceGuard resGuard2{"memoryBlock2"};
  }
  std::cout << "After local scope" << std::endl;
  
  std::cout << std::endl;

  
  std::cout << "\nBefore try-catch block" << std::endl;
  try{
      ResourceGuard resGuard3{"memoryBlock3"};
      throw std::bad_alloc();
  }   
  catch (std::bad_alloc& e){
      std::cout << e.what();
  }
  std::cout << "\nAfter try-catch block" << std::endl;
  
  std::cout << std::endl;

}
```

 

ResourceGuard is a guard that managed its resource. In this case, the resource is a simple string. ResourceGuard creates in it constructor (line 11 - 13) the resource and release the resource in its destructor (line 14 - 16). It does it job very reliable.

The destructor of resGuard1 (line 23) will be exactly called at the end of the main function (line 46). The lifetime of resGuard2 (line 27) already ends in line 28. Therefore, the destructor will automatically be executed. Even the throwing of an exception does not alter the reliability of resGuard3 (line 36). Its destructor will be called at the end of the try block (line 35 - 38).

The screenshot displays the lifetime of the objects.

![raii](https://www.modernescpp.com/images/blog/EmbeddedProgrammierung/RAII/raii.png)

I will refer once more to the program raii.cpp. What is the key idea of the RAII idiom? **The lifetime of a resource will be bound to the lifetime of a local variable. C++ automatically manages the lifetime of locals.**

The idea is quite simple but has far-reaching consequences. Critical resources are bound to local objects. The remaining job is done by the C++ runtime.

### RAII everywhere

RAII everywhere holds for locks [std::lock_guard, std::unique_lock, ](https://www.modernescpp.com/index.php/tag/lock)and [std::shared_lock](https://www.modernescpp.com/index.php/tag/lock) that manage their resource mutex. RAII everywhere holds for the smart pointers [std::unique_ptr](https://www.modernescpp.com/index.php/std-unique-ptr), [std::shared_ptr](https://www.modernescpp.com/index.php/std-shared-ptr), and [std::weak_ptr ](https://www.modernescpp.com/index.php/std-weak-ptr)that manage their resource memory.

Thanks to RAII we have in C++ a kind of garbage collection.

But there is a subtle difference to a general garbage collection.

1. You have to explicitly use smart pointers in C++
2. The memory management with std::unique_ptr has by design no overhead in performance or memory compared to a raw pointer (see [Memory and Performance Overhead of Smart Pointers](https://www.modernescpp.com/index.php/memory-and-performance-overhead-of-smart-pointer)).

Therefore, C++ adhere to its key principle in a twofold way: Don't pay for something you don't use.

## Special resources

Thanks to RAII, the destructor of the local object and therefore the cleaning up of the resource will be done totally deterministic. So far, so good. If the destructor can throw an exception, the destructor of the object modelling RAII will trigger the exception. This will be the case, if the resource is a file. The close function can trigger an exception. Therefore, you have to answer the question for yourself, if it's ok, that the destructor can throw an exception. If not, you should not use RAII.

### Dealing with throwing destructors (**Udo Steinbach**)

Udo Steinbach wrote an e-mail to me about the issue with potentially throwing destructors. Here is the e-mail.

RAII is a nice thing − as long as no error can occur while destroying the object. The latter is forgotten when talking about RAII. Why a destructor should not throw, you can read in many places: [https://www.qwant.com/?q=should%20destructors%20throw](https://www.qwant.com/?q=should destructors throw). As a result RAII has to be supplemented manually in many cases and so it seems to be done twice.

 

```c++
class MyFileHandle
{  public:
      MyFileHandle(...)
         :handle(::OpenFile(...))
      {  if (handle == nullptr)
            throw ...;
      }
      ~MyFileHandle() noexcept
      {  ::CloseFile(handle);
      }
   private:
      MySystemHandle handle;
};


{
   MyFileHandle file(...);
   ...
}
```

Does CloseFile() not close, the call looks correct but the handle is lost, the user has to restart the program and search and delete the file himself, or other embarrassing symptoms as they are familiar from programs.

So the class must have a throwing

```c++
void Close();
```

and the destructor has to check:

```c++
{ 
  MyFileHandle file(...)
  ... 
  file.Close();
}
```

This doesn't look like pure RAII. For symmetry we need a manual Open():

```c++
{
  MyFileHandle file;
  file.Open(...); 
  ...
  file.Close();
}
```

 

− RAII is lost. For the lover remains the consolation, that the object is now reusable and runs correct in both non error and error case.

Under the condition of a proper error handling from the perspective of the program user I renounce RAII in many of my classes. Automatic tests according to an idea from [boost](http://www.boost.org/community/exception_safety.html) show

- [exception safety ](http://en.wikipedia.org/wiki/Exception_safety)as minimum,
- with best known error handling, which must be dispensed with at RAII, e.g. automatically delete incomplete files and rethrow the exception,
- and messages presentable to the user,

an always best possible behaviour of the program: Make users and support happy by replacing crash and data garbage by meaningful messages. An automatism, here destructor or garbage collector, can handle errors only automatically: on minimalistic scale or ignore it completely.

In application programs this should not be acceptable nor should it be.

## 10C Python garbage collection

#### Automatic memory management and garbage collection

With automatic memory management, programmers no longer needed to manage memory themselves. Rather, the runtime handled this for them.

There are a few different methods for automatic memory management, but one of the more popular ones uses reference counting. With reference counting, the runtime keeps track of all of the references to an object. When an object has zero references to it, it’s unusable by the program code and thus able to be deleted.

For programmers, automatic memory management adds a number of benefits. It’s faster to develop programs without thinking about low-level memory details. Further, it can help avoid costly memory leaks or dangerous dangling pointers.

However, automatic memory management comes at a cost. Your program will need to use additional memory and computation to track all of its references. What’s more, many programming languages with automatic memory management use a “stop-the-world” process for garbage collection where all execution stops while the garbage collector looks for and deletes objects to be collected.

With the advances in computer processing from [Moore’s law](https://en.wikipedia.org/wiki/Moore's_law) and the larger amounts of RAM in newer computers, the benefits of automatic memory management usually outweigh the downsides. Thus, most modern programming languages like Java, Python, and Golang use automatic memory management.

For long-running applications where [performance](https://stackify.com/retrace-tracked-functions-monitor-performance/) is critical, some languages still have manual memory management. The classic example of this is C++. We also see manual memory management in [Objective-C](https://en.wikipedia.org/wiki/Objective-C), the language used for macOS and iOS. For newer languages, [Rust](https://www.rust-lang.org/) uses manual memory management.

Now that we know about memory management and garbage collection in general, let’s get more specific about how garbage collection works in Python.

### How Python implements garbage collection

In this section, we’ll cover how garbage collection works in Python.

This section assumes you’re using the [CPython implementation of Python](https://en.wikipedia.org/wiki/CPython). CPython is the most widely used implementation. However, there are other implementations of Python, such as [PyPy](https://pypy.org/), [Jython](http://www.jython.org/) (Java-based), or [IronPython](https://ironpython.net/) (C#-based).

To see which Python you’re using, run the following command in your terminal:

```python
python -c 'import platform; print(platform.python_implementation())'
```

There are two aspects to memory management and garbage collection in CPython:

- Reference counting
- Generational garbage collection

Let’s explore each of these below. 

#### Reference counting in CPython

The main garbage collection mechanism in CPython is through [reference counts](https://docs.python.org/3.6/c-api/intro.html#reference-counts). Whenever you create an object in Python, the underlying C object has both a Python type (such as list, dict, or function) and a reference count.

At a very basic level, a Python object’s reference count is incremented whenever the object is referenced, and it’s decremented when an object is dereferenced. If an object’s reference count is 0, the memory for the object is deallocated.

Your program’s code can’t disable Python’s reference counting. This is in contrast to the generational garbage collector discussed below.

Some people claim [reference counting is a poor man’s garbage collector](https://www.quora.com/Why-doesnt-Apple-Swift-adopt-the-memory-management-method-of-garbage-collection-like-Java-uses#__w2_wTLUJRu99_answer). It does have some downsides, including an inability to detect cyclic references as discussed below. However, reference counting is nice because [you can immediately remove an object when it has no references](https://rushter.com/blog/python-garbage-collector/).

##### Viewing reference counts in Python

You can use the [sys](https://docs.python.org/3/library/sys.html) module from the Python standard library to check reference counts for a particular object. There are a few ways to increase the reference count for an object, such as 

- Assigning an object to a variable.
- Adding an object to a data structure, such as appending to a list or adding as a property on a class instance.
- Passing the object as an argument to a function.

Let’s use a Python REPL and the sys module to see how reference counts are handled.

First, in your terminal, type **python** to enter into a Python REPL.

Second, import the sys module into your REPL. Then, create a variable and check its reference count:

```python
>>> import sys
>>> a = 'my-string'
>>> sys.getrefcount(a)
2
```

Notice that there are two references to our variable **a**. One is from creating the variable. The second is when we pass the variable **a** to the **sys.getrefcount()** function.

If you add the variable to a data structure, such as a list or a dictionary, you’ll see the reference count increase:

```python
>>> import sys
>>> a = 'my-string'
>>> b = [a] # Make a list with a as an element.
>>> c = { 'key': a } # Create a dictionary with a as one of the values.
>>> sys.getrefcount(a)
4
```

As shown above, the reference count of **a** increases when added to a list or a dictionary.

In the next section, we’ll learn about the generational garbage collector, which is the second tool Python uses for memory management.

#### Generational garbage collection

In addition to the reference counting strategy for memory management, Python also uses a method called a generational garbage collector.

The easiest way to understand why we need a generational garbage collector is by way of example.

In the previous section, we saw that adding an object to an array or object increased its reference count. But what happens if you add an object to itself?

```python
>>> class MyClass(object):
...     pass
...
>>> a = MyClass()
>>> a.obj = a
>>> del a
```

In the example above, we defined a new class. We then created an instance of the class and assigned the instance to be a property on itself. Finally, we deleted the instance.

By deleting the instance, it’s no longer accessible in our Python program. However, Python didn’t destroy the instance from memory. The instance doesn’t have a reference count of zero because it has a reference to itself.

We call this type of problem a reference cycle, and you can’t solve it by reference counting. This is the point of the generational garbage collector, which is accessible by the [gc module](https://docs.python.org/3/library/gc.html) in the standard library.

##### Generational garbage collector terminology

There are two key concepts to understand with the generational garbage collector. The first concept is that of a generation.

The garbage collector is keeping track of all objects in memory. A new object starts its life in the first generation of the garbage collector. If Python executes a garbage collection process on a generation and an object survives, it moves up into a second, older generation. The Python garbage collector has three generations in total, and an object moves into an older generation whenever it survives a garbage collection process on its current generation.

The second key concept is the threshold. For each generation, the garbage collector module has a threshold number of objects. If the number of objects exceeds that threshold, the garbage collector will trigger a collection process. For any objects that survive that process, they’re moved into an older generation.

Unlike the reference counting mechanism, you may change the behavior of the generational garbage collector in your Python program. This includes changing the thresholds for triggering a garbage collection process in your code, manually triggering a garbage collection process, or disabling the garbage collection process altogether.

Let’s see how you can use the gc module to check garbage collection statistics or change the behavior of the garbage collector.

##### Using the GC module

In your terminal, enter **python** to drop into a Python REPL.

Import the **gc** module into your session. You can then check the configured thresholds of your garbage collector with the **get_threshold()** method:

```python
>>> import gc
>>> gc.get_threshold()
(700, 10, 10)
```

By default, Python has a threshold of 700 for the youngest generation and 10 for each of the two older generations.

You can check the number of objects in each of your generations with the **get_count()** method:

```python
>>> import gc
>>> gc.get_count()
(596, 2, 1)
```

In this example, we have 596 objects in our youngest generation, two objects in the next generation, and one object in the oldest generation.

As you can see, Python creates a number of objects by default before you even start executing your program. You can trigger a manual garbage collection process by using the **gc.collect()** method:

```python
>>> gc.get_count()
(595, 2, 1)
>>> gc.collect()
57
>>> gc.get_count()
(18, 0, 0)
```

Running a garbage collection process cleans up a huge amount of objects—577 in the first generation and three more in the older generations.

You can alter the thresholds for triggering garbage collection by using the **set_threshold()** method in the gc module:

```python
>>> import gc
>>> gc.get_threshold()
(700, 10, 10)
>>> gc.set_threshold(1000, 15, 15)
>>> gc.get_threshold()
(1000, 15, 15)
```

In the example above, we increase each of our thresholds from their defaults. Increasing the threshold will reduce the frequency at which the garbage collector runs. This will be less computationally expensive in your program at the expense of keeping dead objects around longer.

Now that you know how both reference counting and the garbage collector module work, let’s discuss how you should use this when writing Python applications.

## 10D [Reference Counting in C++ OO-Style](https://stackoverflow.com/questions/11788187/reference-counting-in-c-oo-style)

No, I don't think it's a good idea to reinvent reference counting especially since we have std::shared_ptr now in C++11. You can easily implement your possibly polymorphic reference-counted Pimpl idiom class in terms of std::shared_ptr. Notice how we don't have to implement copy ctor, assignment, dtor anymore and mutation gets simpler w.r.t. the reference counter and cloning:

```cpp
// to be placed into a header file ...

#include <memory>
#include <utility>
#include <string>

class Fred
{
public:
    static Fred create1(std::string const& s, int i);
    static Fred create2(float x, float y);

    void sampleInspectorMethod() const;   // No changes to this object
    void sampleMutatorMethod();           // Change this object

private:
    class Data;
    std::shared_ptr<Data> data_;

    explicit Fred(std::shared_ptr<Data> d) : data_(std::move(d)) {}
};
```

...and the implementation...

```cpp
// to be placed in the corresponding CPP file ...

#include <cassert>
#include "Fred.hpp"

using std::shared_ptr;

class Fred::Data
{
public:
    virtual ~Data() {}                               // A virtual destructor
    virtual shared_ptr<Data> clone() const = 0;      // A virtual constructor
    virtual void sampleInspectorMethod() const = 0;  // A pure virtual function
    virtual void sampleMutatorMethod() = 0;
};

namespace {

class Der1 : public Fred::Data
{
public:
    Der1(std::string const& s, int i);
    virtual void sampleInspectorMethod() const;
    virtual void sampleMutatorMethod();
    virtual shared_ptr<Data> clone() const;
    ...
};

// insert Der1 function definitions here

class Der2 : public Data
{
public:
    Der2(float x, float y);
    virtual void sampleInspectorMethod() const;
    virtual void sampleMutatorMethod();
    virtual shared_ptr<Data> clone() const;
    ...
};

// insert Der2 function definitions here

} // unnamed namespace

Fred Fred::create1(std::string const& s, int i)
{
    return Fred(std::make_shared<Der1>(s,i));
}

Fred Fred::create2(float x, float y)
{
    return Fred(std::make_shared<Der2>(x,y));
}

void Fred::sampleInspectorMethod() const
{
    // This method promises ("const") not to change anything in *data_
    // Therefore we simply "pass the method through" to *data_:
    data_->sampleInspectorMethod();
}

void Fred::sampleMutatorMethod()
{
    // This method might need to change things in *data_
    // Thus it first checks if this is the only pointer to *data_
    if (!data_.unique()) data_ = data_->clone();
    assert(data_.unique());

    // Now we "pass the method through" to *data_:
    data_->sampleMutatorMethod();
}
```

(untested)

> If it is suitable, why do you suppose it's not used more often?

I think reference counting, if you implement it yourself, is easier to get wrong. It also has the reputation of being slow in multithreaded environments because the reference counters have to be incremented and decremented atomically. But I guess due to C++11 which offers shared_ptr and move semantics, this copy-on-write pattern might get a bit more popular again. If you enable move semantics for the Fred class you can avoid some of the costs of atomically incrementing reference counters. So moving a Fred object from one location to another should be even faster than copying it.

## 11 itoa() in c++

```c++
/* A C++ program to implement itoa() */
#include <iostream> 
using namespace std; 
  
/* A utility function to reverse a string  */
void reverse(char str[], int length) 
{ 
    int start = 0; 
    int end = length -1; 
    while (start < end) 
    { 
        swap(*(str+start), *(str+end)); 
        start++; 
        end--; 
    } 
} 
  
// Implementation of itoa() 
char* itoa(int num, char* str, int base) 
{ 
    int i = 0; 
    bool isNegative = false; 
  
    /* Handle 0 explicitely, otherwise empty string is printed for 0 */
    if (num == 0) 
    { 
        str[i++] = '0'; 
        str[i] = '\0'; 
        return str; 
    } 
  
    // In standard itoa(), negative numbers are handled only with  
    // base 10. Otherwise numbers are considered unsigned. 
    if (num < 0 && base == 10) 
    { 
        isNegative = true; 
        num = -num; 
    } 
  
    // Process individual digits 
    while (num != 0) 
    { 
        int rem = num % base; 
        str[i++] = (rem > 9)? (rem-10) + 'a' : rem + '0'; 
        num = num/base; 
    } 
  
    // If number is negative, append '-' 
    if (isNegative) 
        str[i++] = '-'; 
  
    str[i] = '\0'; // Append string terminator 
  
    // Reverse the string 
    reverse(str, i); 
  
    return str; 
} 
  
// Driver program to test implementation of itoa() 
int main() 
{ 
    char str[100]; 
    cout << "Base:10 " << itoa(1567, str, 10) << endl; 
    cout << "Base:10 " << itoa(-1567, str, 10) << endl; 
    cout << "Base:2 " << itoa(1567, str, 2) << endl; 
    cout << "Base:8 " << itoa(1567, str, 8) << endl; 
    cout << "Base:16 " << itoa(1567, str, 16) << endl; 
    return 0; 
} 
```

## 11A atoi 

```c++
class Solution {
public:
    int myAtoi(string str) {
        int i = 0;
        int sign = 1;
        int result = 0;
        if (str.length() == 0) return 0;

        //Discard whitespaces in the beginning
        while (i < str.length() && str[i] == ' ')
            i++;

        // Check if optional sign if it exists
        if (i < str.length() && (str[i] == '+' || str[i] == '-'))
            sign = (str[i++] == '-') ? -1 : 1;

        // Build the result and check for overflow/underflow condition
        while (i < str.length() && str[i] >= '0' && str[i] <= '9') {
            if (result > INT_MAX / 10 ||
                    (result == INT_MAX / 10 && str[i] - '0' > INT_MAX % 10)) {
                return (sign == 1) ? INT_MAX : INT_MIN;
            }
            result = result * 10 + (str[i++] - '0');
        }
        return result * sign;
```

```python
class Solution(object):
    def myAtoi(self, s):
        """
        :type str: str
        :rtype: int
        """
        ###better to do strip before sanity check (although 8ms slower):
        #ls = list(s.strip())
        #if len(ls) == 0 : return 0
        if len(s) == 0 : return 0
        ls = list(s.strip())
        
        sign = -1 if ls[0] == '-' else 1
        if ls[0] in ['-','+'] : del ls[0]
        ret, i = 0, 0
        while i < len(ls) and ls[i].isdigit() :
            ret = ret*10 + ord(ls[i]) - ord('0')
            i += 1
        return max(-2**31, min(sign * ret,2**31-1))
```



## 12 Quick Sort vs Merge Sort



**Prerequisite** :[Merge Sort](https://www.geeksforgeeks.org/merge-sort/) and [Quick Sort](https://www.geeksforgeeks.org/quick-sort/)

**[Quick sort](https://www.geeksforgeeks.org/quick-sort/)** is an internal algorithm which is based on divide and conquer strategy. In this:

- The array of elements is divided into parts repeatedly until it is not possible to divide it further.
- It is also known as **“partition exchange sort”**.
- It uses a key element (pivot) for partitioning the elements.
- One left partition contains all those elements that are smaller than the pivot and one right partition contains all those elements which are greater than the key element.

![quicksort](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2014/01/QuickSort2.png)

**[Merge sort](https://www.geeksforgeeks.org/merge-sort/)** is an external algorithm and based on divide and conquer strategy. In this:

- The elements are split into two sub-arrays (n/2) again and again until only one element is left.
- Merge sort uses additional storage for sorting the auxiliary array.
- Merge sort uses three arrays where two are used for storing each half, and the third external one is used to store the final sorted list by merging other two and each array is then sorted recursively.
- At last, the all sub arrays are merged to make it ‘n’ element size of the array.

![Merge-Sort-Tutorial](https://media.geeksforgeeks.org/wp-content/cdn-uploads/Merge-Sort-Tutorial.png)

### Quick Sort vs Merge Sort

1. **Partition of elements in the array** :
   In the merge sort, the array is parted into just 2 halves (i.e. n/2).
   whereas
   In case of quick sort, the array is parted into any ratio. There is no compulsion of dividing the array of elements into equal parts in quick sort.
2. **Worst case complexity** :
   The worst case complexity of quick sort is O(n2) as there is need of lot of comparisons in the worst condition.
   whereas
   In merge sort, worst case and average case has same complexities O(n log n).
3. **Usage with datasets** :
   Merge sort can work well on any type of data sets irrespective of its size (either large or small).
   whereas
   The quick sort cannot work well with large datasets.
4. **Additional storage space requirement** :
   Merge sort is not in place because it requires additional memory space to store the auxiliary arrays.
   whereas
   The quick sort is in place as it doesn’t require any additional storage.
5. **Efficiency** :
   Merge sort is more efficient and works faster than quick sort in case of larger array size or datasets.
   whereas
   Quick sort is more efficient and works faster than merge sort in case of smaller array size or datasets.
6. **Sorting method** :
   The quick sort is internal sorting method where the data is sorted in main memory.
   whereas
   The merge sort is external sorting method in which the data that is to be sorted cannot be accommodated in the memory and needed auxiliary memory for sorting.
7. **Stability** :
   Merge sort is stable as two elements with equal value appear in the same order in sorted output as they were in the input unsorted array.
   whereas
   Quick sort is unstable in this scenario. But it can be made stable using some changes in code.
8. **Preferred for** :
   Quick sort is preferred for arrays.
   whereas
   Merge sort is preferred for linked lists.
9. **Locality of reference** :
   Quicksort exhibits good cache locality and this makes quicksort faster than merge sort (in many cases like in virtual memory environment).

|          BASIS FOR COMPARISON          |                          QUICK SORT                          |                          MERGE SORT                          |
| :------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| The partition of elements in the array | The splitting of a array of elements is in any ratio, not necessarily divided into half. | The splitting of a array of elements is in any ratio, not necessarily divided into half. |
|         Worst case complexity          |                            O(n^2)                            |                           O(nlogn)                           |
|             Works well on              |                It works well on smaller array                |            It operates fine on any size of array             |
|           Speed of execution           | It work faster than other sorting algorithms for small data set like Selection sort etc |        It has a consistent speed on any size of data         |
|  Additional storage space requirement  |                        Less(In-place)                        |                      More(not In-place)                      |
|               Efficiency               |                Inefficient for larger arrays                 |                        More efficient                        |
|             Sorting method             |                           Internal                           |                           External                           |
|               Stability                |                          Not Stable                          |                            Stable                            |
|             Preferred for              |                          for Arrays                          |                       for Linked Lists                       |
|         Locality of reference          |                             good                             |                             poor                             |



## 13 [Merging k sorted streams of infinite length](https://imeandalgorithms.blogspot.com/2012/11/merging-k-sorted-streams-of-infinite.html)

***Variant of this problem from CLRS***
Give an *O(n* lg *k)*-time algorithm to merge *k* sorted lists into one sorted list, where *n* is the total number of elements in all the input lists. (*Hint:* Use a minheap for *k*-way merging.)

**Solution**
Build a heap out the first element from all k streams.
Remove the min and insert the next element into the heap from the stream to which min belongs. Keep repeating this till we run out of all streams.

**Complexity analysis**
Extracting the min element from a k-element heap, takes lgn => for n elements, O(nlgk)

### 21. Merge Two Sorted Lists

### 23. Merge k Sorted Lists



## 14 Square all the numbers in a sorted array and maintain sortedness

### 977. Squares of a Sorted Array

```python
class Solution(object):
    def sortedSquares(self, A):
        N = len(A)
        # i, j: negative, positive parts
        j = 0
        while j < N and A[j] < 0:
            j += 1
        i = j - 1

        ans = []
        while 0 <= i and j < N:
            if A[i]**2 < A[j]**2:
                ans.append(A[i]**2)
                i -= 1
            else:
                ans.append(A[j]**2)
                j += 1

        while i >= 0:
            ans.append(A[i]**2)
            i -= 1
        while j < N:
            ans.append(A[j]**2)
            j += 1

        return ans	

```

**Complexity Analysis**

- Time Complexity: O*(*N*), where N* is the length of `A`.
- Space Complexity: *O*(*N*).



## 15 find the closest number in array

### 658. Find K Closest Elements

### Find closest number in array

```
Input : arr[] = {1, 2, 4, 5, 6, 6, 8, 9}
             Target number = 11
Output : 9
9 is closest to 11 in given array

Input :arr[] = {2, 5, 6, 7, 8, 8, 9}; 
       Target number = 4
Output : 5
```

```c++
/ CPP program to find element 
// closest to given target. 
#include <bits/stdc++.h> 
using namespace std; 
  
int getClosest(int, int, int); 
  
// Returns element closest to target in arr[] 
int findClosest(int arr[], int n, int target) 
{ 
    // Corner cases 
    if (target <= arr[0]) 
        return arr[0]; 
    if (target >= arr[n - 1]) 
        return arr[n - 1]; 
  
    // Doing binary search 
    int i = 0, j = n, mid = 0; 
    while (i < j) { 
        mid = (i + j) / 2; 
  
        if (arr[mid] == target) 
            return arr[mid]; 
  
        /* If target is less than array element, 
            then search in left */
        if (target < arr[mid]) { 
  
            // If target is greater than previous 
            // to mid, return closest of two 
            if (mid > 0 && target > arr[mid - 1]) 
                return getClosest(arr[mid - 1], 
                                  arr[mid], target); 
  
            /* Repeat for left half */
            j = mid; 
        } 
  
        // If target is greater than mid 
        else { 
            if (mid < n - 1 && target < arr[mid + 1]) 
                return getClosest(arr[mid], 
                                  arr[mid + 1], target); 
            // update i 
            i = mid + 1;  
        } 
    } 
  
    // Only single element left after search 
    return arr[mid]; 
} 
  
// Method to compare which one is the more close. 
// We find the closest by taking the difference 
// between the target and both values. It assumes 
// that val2 is greater than val1 and target lies 
// between these two. 
int getClosest(int val1, int val2, 
               int target) 
{ 
    if (target - val1 >= val2 - target) 
        return val2; 
    else
        return val1; 
} 
  
// Driver code 
int main() 
{ 
    int arr[] = { 1, 2, 4, 5, 6, 6, 8, 9 }; 
    int n = sizeof(arr) / sizeof(arr[0]); 
    int target = 11; 
    cout << (findClosest(arr, n, target)); 
} 
```

```python
# Python3 program to find element 
# closet to given target. 
  
# Returns element closest to target in arr[] 
def findClosest(arr, n, target): 
  
    # Corner cases 
    if (target <= arr[0]): 
        return arr[0] 
    if (target >= arr[n - 1]): 
        return arr[n - 1] 
  
    # Doing binary search 
    i = 0; j = n; mid = 0
    while (i < j):  
        mid = (i + j) / 2
  
        if (arr[mid] == target): 
            return arr[mid] 
  
        # If target is less than array  
        # element, then search in left 
        if (target < arr[mid]) : 
  
            # If target is greater than previous 
            # to mid, return closest of two 
            if (mid > 0 and target > arr[mid - 1]): 
                return getClosest(arr[mid - 1], arr[mid], target) 
  
            # Repeat for left half  
            j = mid 
          
        # If target is greater than mid 
        else : 
            if (mid < n - 1 and target < arr[mid + 1]): 
                return getClosest(arr[mid], arr[mid + 1], target) 
                  
            # update i 
            i = mid + 1
          
    # Only single element left after search 
    return arr[mid] 
  
  
# Method to compare which one is the more close. 
# We find the closest by taking the difference 
# between the target and both values. It assumes 
# that val2 is greater than val1 and target lies 
# between these two. 
def getClosest(val1, val2, target): 
  
    if (target - val1 >= val2 - target): 
        return val2 
    else: 
        return val1 
  
# Driver code 
arr = [1, 2, 4, 5, 6, 6, 8, 9]  
n = len(arr) 
target = 11
print(findClosest(arr, n, target)) 
```

https://github.com/awangdev/LintCode/blob/master/Java/Closest%20Number%20in%20Sorted%20Array.java



## 16 Find the kth largest element in an array

### 215. Kth Largest Element in an Array