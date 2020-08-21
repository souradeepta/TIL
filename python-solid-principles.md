# S.O.L.I.D Principles explained in Python with examples.



SOLID is a mnemonic abbreviation for a set of design principles created for software development in object-oriented languages. The principles in **SOLID** are intended to foster simpler, more robust and updatable code from software developers. Each letter in SOLID corresponds to a principle for development:

When implemented properly it makes your code more **extendable, logical and easier to read.**

To understand **SOLID** principles, you have to know the use of the interface clearly.

I’m going to try to explain **SOLID** Principles in Python in simplest way so that it’s easy for beginners to understand. It would be very easy to just take the examples provided and apply them to Python.

Let’s go through each principle one by one:



## 1.Single Responsibility Principle

The Single Responsibility Principle requires that a class should have only one job. So if a class has more than one responsibility, it becomes coupled. A change to one responsibility results to modification of the other responsibility.

![Image for post](https://miro.medium.com/max/625/1*cgIth8fwgIGVcU76YJKqcg.png)

```python
#Below is Given a class which has two responsibilities 
class  User:
    def __init__(self, name: str):
        self.name = name
    
    def get_name(self) -> str:
        pass

    def save(self, user: User):
        pass
```

We have a User class which is responsible for both the user properties and user database management. If the application changes in a way that it affect database management functions. The classes that make use of User properties will have to be touched and recompiled to compensate for the new changes. It’s like a domino effect, touch one card it affects all other cards in line.



![Image for post](https://miro.medium.com/max/625/1*5V7-R7t5h2ujtxNn6YBbXQ.png)

So we simply split the class, we create another class that will handle the one responsibility of storing an user to a database:

```python
class User:
    def __init__(self, name: str):
            self.name = name
    
    def get_name(self):
        pass


class UserDB:
    def get_user(self, id) -> User:
        pass

    def save(self, user: User):			
        pass
```



A common solution to this dilemma is to apply the [Façade pattern](https://kennison.name/files/zopestore/uploads/python/DesignPatternsInPython_ver0.1.pdf). For introduction to Façade pattern you can[ read more](https://medium.com/@andreaspoyias/design-patterns-a-quick-guide-to-facade-pattern-16e3d2f1bfb6). User class will be the Facade for user database management and user properties management.



## 2.Open-Closed Principle

Software entities(Classes, modules, functions) should be open for extension, not modification.



![Image for post](https://miro.medium.com/max/1188/1*Q8P1-xeih046vTmC-fVXIQ.jpeg)

Let’s imagine you have a store, and you give a discount of 20% to your favorite customers using this class: When you decide to offer double the 20% discount to VIP customers. You may modify the class like this:

```python
class Discount:
  def __init__(self, customer, price):
      self.customer = customer
      self.price = price
  def give_discount(self):
      if self.customer == 'fav':
          return self.price * 0.2
      if self.customer == 'vip':
          return self.price * 0.4
```

No, this fails the OCP principle. OCP forbids it. If we want to give a new percent discount maybe, to a different type of customers, you will see that a new logic will be added. To make it follow the OCP principle, we will add a new class that will extend the Discount. In this new class, we would implement its new behavior:

```python
class Discount:
    def __init__(self, customer, price):
      self.customer = customer
      self.price = price
    def get_discount(self):
      return self.price * 0.2
class VIPDiscount(Discount):
    def get_discount(self):
      return super().get_discount() * 2

```



If you decide 80% discount to super VIP customers, it should be like this:

> Extension without modification.

![Image for post](https://miro.medium.com/max/60/1*9cxknUm5GIyd1t62qy6HOA.png?q=20)

![Image for post](https://miro.medium.com/max/416/1*9cxknUm5GIyd1t62qy6HOA.png)

## 3.Liskov Substitution Principle

The main idea behind Liskov Subtitution Principle is that, for any class, a client should be able to use any of its subtypes indistinguishably, without even noticing, and therefore without compromising the expected behavior at runtime. This means that clients are completely isolated and unaware of changes in the class hierarchy.



![Image for post](https://miro.medium.com/max/1250/1*oKOS1298SaUjoW7YNpwEnw.png)

*More formally:
Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.*

In simpler terms means that a subclass, child or specialization of an object or class must be suitable by its Parent or SuperClass.

```python
class User():
  def __init__(self, color, board):
    create_pieces()
    self.color = color
    self.board = board
  def move(self, piece:Piece, position:int):
      piece.move(position)
      chessmate_check()
  board = ChessBoard()
  user_white = User("white", board)
  user_black = User("black", board)
  pieces = user_white.pieces
  horse = helper.getHorse(user_white, 1)
  user.move(horse)
```



Remarks on the LSP The LSP is fundamental to a good object-oriented software design because it emphasizes one of its core traits — polymorphism. It is about creating correct hierarchies so that classes derived from a base one are polymorphic along the parent one, with respect to the methods on their interface. It is also interesting to notice how this principle relates to the previous one — if we attempt to extend a class with a new one that is incompatible, it will fail, the contract with the client will be broken, and as a result such an extension will not be possible (or, to make it possible, we would have to break the other end of the principle and modify code in the client that should be closed for modification, which is completely undesirable and unacceptable).

Carefully thinking about new classes in the way that LSP suggests helps us to extend the hierarchy correctly. We could then say that LSP contributes to the OCP.

## 4.Interface Segregation Principle

Make fine grained interfaces that are client specific Clients should not be forced to depend upon interfaces that they do not use. This principle deals with the disadvantages of implementing big interfaces.

![Image for post](https://miro.medium.com/max/400/1*4F4faKqXtTH87DyKWZOIrQ.jpeg)

To illustrate this completely, we will go with a classic example because it is highly significant and easily understandable. The Classic Example

```python
lass IShape:
    def draw(self):
        raise NotImplementedError

class Circle(IShape):
    def draw(self):
        pass

class Square(IShape):
    def draw(self):
        pass

class Rectangle(IShape):
    def draw(self):
        pass
```



Another nice trick is that in our business logic, a single class can implement several interfaces if needed. So we can provide a single implementation for all the common methods between the interfaces. The segregated interfaces will also force us to think of our code more from the client’s point of view, which will in turn lead to loose coupling and easy testing. So, not only have we made our code better to our clients, we also made it easier for ourselves to understand, test and implement.



## 5.Dependecy Inversion Principle

Dependency should be on abstractions not concretions. High-level modules should not depend upon low-level modules. Both low and high level classes should depend on the same abstractions. Abstractions should not depend on details. Details should depend upon abstractions.

![Image for post](https://miro.medium.com/max/750/1*pLf1DdkblANJDBdR6yURkg.png)

There comes a point in software development where our app will be largely composed of modules. When this happens, we have to clear things up by using dependency injection. High-level components depending on low-level components to function. To create specific behavior you can use techniques like inheritance or interfaces.

```python
class AuthenticationForUser():
  def __init__(self, connector:Connector):
		self.connection = connector.connect()
	
	def authenticate(self, credentials):
		pass
	def is_authenticated(self):
		pass	
	def last_login(self):
		pass

class AnonymousAuth(AuthenticationForUser):
	pass

class GithubAuth(AuthenticationForUser):
	def last_login(self):
		pass

class FacebookAuth(AuthenticationForUser):
	pass

class Permissions()
	def __init__(self, auth: AuthenticationForUser)
		self.auth = auth
		
	def has_permissions():
		pass
		
class IsLoggedInPermissions (Permissions):
	def last_login():
		return auth.last_log
```





You can learn more:

[Solid Python](https://www.researchgate.net/publication/323935872_SOLID_Python_SOLID_principles_applied_to_a_dynamic_programming_language)

[Clean Code in Python](https://www.amazon.com/Clean-Code-Python-Refactor-legacy/dp/1788835832)