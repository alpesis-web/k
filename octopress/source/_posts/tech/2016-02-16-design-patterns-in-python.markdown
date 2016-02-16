---
layout: post_tech
title: "Design Patterns in Python"
date: 2016-02-16 00:16:49 +0800
comments: true
categories: [tech]
tags: [python, programming, design patterns]
toc: true
---

Study notes on [Python in Practice: Create Better Programs Using Concurrency, Libraries, and Patterns](http://www.amazon.com/Python-Practice-Concurrency-Libraries-Developers/dp/0321905636)


- Creational design patterns
- Structural design patterns
- Behavioral design patterns

Comparisons:

- Creational design patterns concern on how objects are created;
- Structural design patterns concern on how objects are composed together to form new and larger objects;
- Behavioral design patterns concern on how individual objects or groups of objects can get things done.


## 1. Creational Design Patterns

Creational design patterns are concerned with how objects are created. When we concern
on how objects are created, the creational design patterns are useful.

### 1.1. Abstract factory

The abstract facroty pattern is designed for situations where we want to create complex
objects that are composed of other objects and where the composed objects are all of one
particular "family".

```python DiagramFactory
class DiagramFactory:

    @classmethod
    def make_diagram(self, width, height):
        pass

    @classmethod
    def make_rectangle(self, x, y, width, height, fill="white"):
        pass

    @classmethod
    def make_text(self, x, y, text, fontsize=12):
        pass


class SvgDiagramFactory(DiagramFactory):

    class Text:
        def __init__(self):
            pass

    def make_diagram(self, width, height):
        return SvgDiagram(width, height)

class SvgDiagram:

    def add(self, component):
        pass


def create_diagram(factory):
    ...
    return diagram


if __name__ == '__main__':
    txtDiagram = create_diagram(DiagramFactory())
    svgDiagram = create_diagram(SvgDiagramFactory())
```

### 1.2. Builder

The builder pattern is similar to the abstract factory pattern in that both patterns
are designed for creating complex objects that are composed of other objects.
But builder pattern also holds the representation of the entire complex object itself.

The differences between abstract factory and builder are
- factory is the subclass: Car -> Honda/Ford
- builder implements the whole parts: Card -> Honda()/Ford() with custom updates

```python

class AbstractFormBuilder(metaclass=abc.ABCMeta):

    @abc.abstractmethod
    def add_title(self, title):
        pass

    @abc.abstractmethod
    def form(self):
        pass

    @abc.abstractmethod
    def add_label(self, text, row, column, **kwargs):
        pass

class HtmlFormBuilder(AbstractFormBuilder):

    def __init__(self):
        pass

    def add_title(self, title):
        pass

    def add_label(self, text, row, column, **kwargs):
        pass

    def add_entry(self, variable, row, column, **kwargs):
        pass

    def form(self):
        pass

class TkFormBuilder(AbstractFormBuilder):

    def __init__(self):
        pass

    def add_title(self, title):
        pass

    def add_label(self, text, row, column, **kwargs):
        pass

    def form(self):
        pass

def create_login_form(builder):
    ...
    return builder


if __name__ == '__main__':
    htmlForm = create_login_form(HtmlFormBuilder())
    tkForm = create_logiin_form(TkFormBuilder())
```

### 1.3. Factory method

The factory method pattern is intended to be used when we want subclasses to choose
which classes they should instantiate when an object is requested. This is useful in its
own right, but can be taken further and used in cases where we cannot know the class in
advance (e.g. the class to use is based on what we read from a file or depends on user
input).

```python

BLACK, WHITE = ("BLACK", "WHITE")

class AbstractBoard:

    def __init__(self, rows, columns):
        self.board = [[None for _ in range(columns) for _ in range(rows)]]
        self.populate_board()

    def populate_board(self):
        raise NotImplementedError()

    def __str__(self):
        squares = []
        for y, row in enumerate(self.board):
            for x, piece in enumerate(row):
                square = console(piece, BLACK if (y+x) %s else WHITE)
            squares.append("\n")
        return "".join(squares)

class CheckersBoard(AbstractBoard):

    def __init__(self):
        super().__init__(10,10)

    def populate_board(self):
        for x in range(0, 9, 2):
            for row in range(4):
                column = x + ((row + 1) % 2)
                self.board[row][column] = BlackDraught()
                self.board[row+6][column] = WhiteDraught()

class ChessBoard(AbstractBoard):

    def __init__(self):
        super().__init__(8, 8)

    def populate_board(self):
        self.board[0][0] = BlackChessRook()
        self.board[0][1] = BlackChessKnight()
        ...
        self.board[7][7] = WhiteChessRook()
        for column in range(8):
            self.board[1][column] = BlackChessPawn()
            self.board[6][column] = WhiteChessPawn()

class Piece(str):
    
    __slots__ = ()

ckass BlackDraught(Piece):

    __slots__ = ()
    
    def __new__(Class):
        return super().__new__(Class, "\N(black draughts man)")

class WhiteChessKing(Piece):

    __slots__ = ()

    def __new__(Class):
        return super().__new__(Class, "\N(white chess king)")

if __name__ == '__main__':

    checkers = CheckersBoard()
    chess = ChessBoard()
```

### 1.4. Prototype

The prototype pattern is used to create new objects by cloning an original object,
and then modifying the clone.

```python
class Point:

    __slots__ = ("x", "y")

    def __init__(self, x, y):
        self.x = x
        self.y = y

def make_object(Class, *args, **kwargs):
    return Class(*args, **kwargs)


if __name__ == '__main__':

    point1 = Point(1, 2)
    point2 = eval("{}({}, {}))".format("Point", 2, 4))
    point3 = getattr(sys.modules[__name__], "Point")(3,6)
    point4 = globals()["Point"](4, 8)
    point5 = make_object(Point, 5, 10)
    point6 = copy.deepcopy(point5)
    point6.x = 6
    point6.y = 12
    point7 = point1.__class__(7, 14)
```

### 1.5. Singleton

The singleton pattern is used when we need a class that has only a single instance
that is the one and only instance accessed throughtout the program.



## 2. Structural Design Patterns

The primary concern of structural design patterns is how objects are composed together to
form new, larger objects.

Three themes stand out in structural design patterns:

- adapting interfaces
- adding functionality
- handling collections of objects

Patterns:

- the adapter and facade patterns make it straightforward to reuse classes in new contexts
- the bridge pattern makes it possible to embed the sophisticated functionality of one class inside another
- the composite pattern makes it easy to create hierarchies of objects
- the flyweight pattern is to use the object reference



### 2.1. Adapter

The adapter pattern is a technique for adapting an interface so that one class
can make use of another -- that has an incompatible interface -- without changing either
of the classes being used.

```python
"""
Page
 --> renderer <---- Renderer Interface
                         |---> TextRenderer
                         |---> HtmlRenderer <---- HtmlWriter (adapter)
"""

class Page:

    def __init__(self, title, renderer):
        pass

    def add_paragraph(self, paragraph):
        pass

    def render(self):
        pass

class Renderer(metaclass=abc.ABCMeta):

    @classmethod
    def __subclasshook__(Class, Subclass):
        pass

class TextRenderer:

    def __init__(self, width=80, file=sys.stdout):
        pass

    def header(self, title):
        pass

    def paragraph(self, text):
        pass

    def footer(self):
        pass

class HtmlRenderer:

    def __init__(self, htmlWriter):
        self.htmlWriter = htmlWriter

    def header(self, title):
        self.htmlWriter.header()
        self.htmlWriter.title(title)
        self.htmlWriter.start_body()

    def paragraph(self, text):
        self.htmlWriter.body(text)

    def footer(self):
        self.htmlWriter.end_body()
        self.htmlWriter.footer()

class HtmlWriter:

    def __init__(self, file=sys.stdout):
        pass

    def header(self):
        pass

    def title(self, title):
        pass

    def start_body(self, text):
        pass

    def body(self, text):
        pass

    def end_body(self):
        pass
 
    def footer(self):
        pass


if __name__ == '__main__':

    textPage = Page(title, TextRenderer(22))
    textPage.add_paragraph(paragraph1)
    textPage.add_paragraph(paragraph2)
    textPage.render()

    htmlPage = Page(title, HtmlRenderer(HtmlWriter(file)))
    htmlPage.add_paragraph(paragraph1)
    htmlPage.add_paragraph(paragraph2)
    htmlPage.render()
```

### 2.2. Bridge

The bridge pattern is used in situations where we want to separate an abstraction
(e.g., an interface or an algorithm from how it is implemented.

The conventional approach without using the bridge pattern would be to create one
or more abstract base classes and then provide two or more concrete implementations
of each of the base classes.

But with the bridge pattern the approach is to create two independent class hierarchies:

- the "abstract" one defining the operations (e.g., the interface and high-level algorithms)
- the concrete one providing the implementations that the abstract operations will ultimately call.

The "abstract" class aggregates an instance of one of the concrete implementation classes -
and this instance serves as a bridge between the abstract interface and the concrete operations.

Bridge pattern is to pass serveral separated classes into a base class (bridge).

```python
"""
BarCharter
  |----> renderer  <---- Bar Charter Interface
                               |----> TextBarRenderer
                               |----> ImageBarRenderer
"""

class BarCharter:

    def __init__(self, renderer):
        pass

    def render(self, caption, pairs):
        pass

@Qtrac.has_methods("initialize", "draw_caption", "draw_bar", "finalize")
class BarRenderer(metaclass-abc.ABCMeta): pass

class TextBarRenderer:

    def __init__(self, scaleFactor=40):
        pass

    def initialize(self, bars, maximum):
        pass

    def draw_caption(self, caption):
        pass
  
    def draw_bar(self, name, value):
        pass

    def finalize(self):
        pass

class ImageBarRenderer:

    COLORS = [Image.color_for_name(name) for name in ("red", "green", "blue")]

    def __init__(self, stepHeight=10, barWidth=30, barGap=2):
        pass

    def initialize(self, bars, maximum):
        pass

    def draw_caption(self, caption):
        pass

    def draw_bar(self, name, value):
        pass

    def finalize(self):
        pass


if __name__ == '__main__':

    pairs = (("Mon", 16), ("Tue", 17), ("Wed", 19))

    textBarCharter = BarCharter(TextBarRenderer())
    imageBarCharter = BarCharter(ImageBarRenderer())
```

### 2.3. Composite

The composite pattern is designed to support the uniform treatment of objects
in a hierarchy, whether they contain other objects (as part of the hierarchy) or not.
Such objects are called composite.

In the classic approach, composite objects have the same base class for both individual
objects and for collections of objects. Both composite and noncomposite objects normally
have the same core methods, with composite objects also having additional methods to
support adding, removing, and iterating their child objects.

This pattern is often used in drawing programs, such as Inkscape, to support grouping and
ungrouping. The pattern in such cases because when the user selects components to group
or ungroup, some of the components might be single items (e.g., a rectangle), while
others might be composite (e.g., a face made up of many different shapes).


```python
"""
Boxed Pencil Set
  |----> Box
  |----> PencilSet ----> Pencil, Ruler, Eraser
  |----> Pencil

SimpleItem (concrete)
  |----> AbstractItem (abstract)

CompositeItem (concreate)
  |----> AbstractCompositeItem (abstract)
                 |----> AbstractItem (abstract)

"""

class AbstractItem(metaclass=abc.ABCMeta):

    @abc.abstractproperty
    def composite(self):
        pass

    def __iter__(self):
        return iter([])

class SimpleItem(AbstractItem):

    def __init__(self, name, price=0.00):
        pass

    @property
    def composite(self):
        return False

    def print(self, indent="", file=sys.stdout):
        pass

class AbstractCompositeItem(AbstractItem):

    def __init__(self, *items):
        self.children = []
        if items:
            self.add(*items)

    def add(self, first, *items):
        self.children.append(first)
        if items:
            self.children.extend(items)

    def remove(self, item):
        self.children.remove(item)

    def __iter__(self):
        return iter(self.children)

class CompositeItem(AbstractCompositeItem):

    def __init__(self, name, *items):
        super().__init__(*items)
        self.name = name

    @property
    def composite(self):
        return True

    @property
    def price(self):
        return sum(item.price for item in self)

    def print(self, indent="", file=sys.stdout):
        pass


if __name__ == '__main__':

    pencil = SimpleItem("Pencil", 0.40)
    ruler = SimpleItem("Ruler", 1.60)
    eraser = SimpleItem("Eraser", 0.20)
    pencilSet = CompositeItem("Pencil Set", pencil, ruler, eraser)

    box = SimpleItem("Box", 1.00)
    boxedPencilSet = CompositeItem("Boxed Pencil Set", box, pencilSet)
    boxedPencilSet.add(pencil)
```

### 2.4. Decorator

A decorator is a function that takes a function as its sole argument and returns a new
function with the same name as the original function but with enhanced functionality.
Decorators are often used by frameworks (e.g., web frameworks) to make it easy to integrate
our own functions within the framework.

```python
"""
@decorator #3
  @decorator #2
    @decorator #1
      function, method, or class
"""

@float_args_and_return
def mean(first, second, *rest):
   numbers = (first, second) + rest
   return sum(numbers) / len(numbers)

def float_args_and_return(function):
    @functools.wraps(function)
    def wrapper(*args, **kwargs):
        args = [float(arg) for arg in args]
        return float(function(*args, **kwargs))
    return wrapper



@statically_typed(str, str, return_type=str)
def make_tagged(text, tag):
    return "{0}{1}{0}".format(tag, escape(text))

@statically_typed(str, int, str)
def repeat(what, count, separator):
    return ((what + separator) * count)[:-len(separator)]

def statically_typed(*types, return_type=None):
    def decorator(function):
        @functools.wraps(function)
        def wrapper(*args, **kwargs):
            ...
            return result
        return wrapper
    return decorator

#--------------------------------------------------#

@ensure("title", is_non_empty_str)
@ensure("isbn", is_valid_isbn)
@ensure("price", is_in_range(1, 100000))
@ensure("quantity", is_in_range(0, 10000))
class Book:

    def __init__(self, title, isbn, price, quantity):
        pass

    @property
    def value(self):
        return self.price * self.quantity


ensure("title", is_non_empty_str)(
    ensure("isbn", is_valid_isbn)(
      ensure("price", is_in_range(1, 100000))(
          ensure("quantity", is_in_range(0, 10000))(class Book: ...))))

def ensure(name, validate, doc=None):
    def decorator(Class):
        privateName = "__" + name
        def getter(self):
            return getattr(self, privateName)
        def setter(self, value):
            validate(name, value)
            setattr(self, privateName, value)
        setattr(Class, name, property(getter, setter, doc=doc))
        return Class
    return decorator

def is_non_empty_str(name, value):
    pass

def is_in_range(minimum=None, maximum=None):
    pass

#--------------------------------------------------#

@do_ensure
class Book:

    title = Ensure(is_non_empty_str)
    isbn = Ensure(is_valid_isbn)
    price = Ensure(is_in_range(1, 10000))
    quantity = Ensure(is_in_range(0, 10000))

    def __init__(self, title, isbn, price, quantity):
        self.title = title
        self.isbn = isbn
        self.price = price
        self.quantity = quantity

    @property
    def value(self):
        return self.price * self.quantity

class Ensure:

    def __init__(self, validate, doc=None):
        self.validate = validate
        self.doc = doc

def do_ensure(Class):

    def make_property(name, attribute):
        privateName = "__" + name
        def getter(self):
            return getattr(self, privateName)
        def setter(self, value):
            attribute.validate(name, value)
            setattr(self, privatName, value)
        return property(getter, setter, doc=attribute.doc)
    for name, attribute in Class.__dict__.items():
        if isinstance(attribute, Ensure):
            setattr(Class, name, make_property(name, attribute)
    return Class
```

### 2.5. Facade 

The facade pattern is used to present a simplified and uniform interface to a
subsystem whose interface is too complex or too low-level for convenient use.


```python

"""
Archive
  |----> filename
  |----> names()
  |----> unpack()  ----> gzip
                   ----> tarfile.TarFile --> getnames()/extractall()
                   ----> zipfile.ZipFile --> namelist()/extractall()
"""

class Archive:

    def __init__(self, filename):
        self._names = None
        self._unpack = None
        self._file = None
        self._filename = filename

    @property
    def filename(self):
        return self.__filename`

    @filename.setter
    def filename(self, name):
        self.close()
        self.__filename = name

    def close(self):
        pass

    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        self.close()

    def names(self):
        if self._file is None:
            self._prepare()
        return self._names()

    def unpack(self):
        if self._file is None:
            self._prepare()
        return self._unpack()

    def _prepare(self):
        if self.filename.endswith((".tar.gz", ".tar.gz2", "tar.xz", ".zip")):
            self._prepare_tarball_or_zip()
        elif self.filename.endswith(".gz"):
            self._prepare_gzip()
        else:
            raise ValueError("unreadable: {}".format(self.filename))

    def _prepare_tarball_r_zip(self):
        pass

    def _prepare_gzip(self):
        pass
```

### 2.6. Flyweight 

The flyweight pattern is designed for handling large numbers of relatively small objects,
where many of the small objects are duplicates of each other. The pattern is implemented by
representing each unique object only once, and by sharing this unique instance wherever it
is needed.

```python
red, gree, blud = "red", "green", "blue"
x = (red, green, blue, red, green, blue, red, green)
y = ("red", "green", "blue", "red", "green", "blue", "red", "green")

class Point:

    #__slots__ = ("x", "y", "z", "color")
    __slots__ = ()
    __dbm = shelve.open(os.path.join(tempfile.gettempdir(), "point.db"))

    def __init__(self, x=0, y=0, z=0, color=None):
        self.x = x
        self.y = y
        self.z = z
        self.color = color

    def __key(self, name):
        return "{:X}:{}".format(id(self), name)

    def __getattr__(self, name):
        return Point.__dbm(self.__key(name))

    def __setattr__(self, name, value):
        Point.__dbm[self.__key(name)] = value
```

### 2.7. Proxy 

The proxy pattern is used when we want one object to stand in for another.

Four use cases:

- a remote proxy where a local object proxies a remote object
- a virtual proxy that allows us to create lightweight objects instead of heavyweight objects
- a protection proxy that provides different levels of access depending on a client's access rights
- a smart reference that "performs additional actions where an object accessed"

Proxy pattern is also be used in unit testing.

```python

class ImageProxy:

    def __init__(self, ImageClass, width=None, height=None, filename=None):
        ...
        self.Image = ImageClass

    def load(self, filename):
        self.commands = [(self.Image, None, None, filename)]

class Image:

    def set_pixel(self, x, y, color):
        pass

    def line(self, x0, y0, x1, y1, color):
        pass

    def rectangle(self, x0, y0, x1, y1, outline=None, file=None):
        pass

    def ellipse(self, x0, y0, x1, y1, outline=None, fill=None):
        pass

    def save(self, filename=None):
        pass

if __name__ == '__main__':

    YELLOW, CYAN, BLUE, RED, BLACK = (Image.color_for_name(color)
        for color in ("yellow", "cyan", "blue", "red", "black"))

    image = ImageProxy(Image.Image, 300, 60)
    image.rectangle(0, 0, 299, 59, fill=YELLOW)
    image.ellipse(0, 0, 299, 59,, fill=CYAN)
    image.ellipse(60, 20, 120, 40, BLUE, RED)
    image.line(181, 32, 239, 32, BLUE)
    image.save(filename)
```

## 3. Behavioral Design Patterns

### 3.1. Chain of responsibility

### 3.2. Command

### 3.3. Interpreter

### 3.4. Iterator

### 3.5. Mediator 

### 3.6. Memento 

### 3.7. Observer 

### 3.8. State 

### 3.9. Strategy 

### 3.10. Template method 

### 3.11. Visitor 

