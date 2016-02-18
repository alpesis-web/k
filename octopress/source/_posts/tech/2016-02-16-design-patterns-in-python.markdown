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

```python FormBuilder

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

```python Factory method

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

```python Prototype
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

```python Adapter
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

```python Bridge
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


```python Composite
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

```python Decorator
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


```python Facade

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

```python Flyweight
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

```python Proxy

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

The behavioral patterns are concerned with how things get done, that is, with algorithms
and object interactions. They provide powerful ways of thinking about and organizing
computations.

### 3.1. Chain of responsibility

The chain of responsibility pattern is designed to decouple the sender of a request from
the recipient that processes the request.

The first function sends a request to a chain of receivers. The first receiver in the chain
either can handle the request to the next receiver in the chain. The second receiver has the
same choices, and so on, until the last one is reached (which could choose to throw the request
away or to raise an exception).


```python Conventional Chain
"""
Conventional Chain

A generator is a function or method that has one or more yield expressions instead of
returns. Whenever a yield is reached, the value yielded is produced, and the function
or method is suspended with all its state intact. At this point the function has
yielded the processor (to the receiver of the value it has produced), so although
suspended, the function does not block. Then, when the function or method is used again,
execution resumes from the statement following the yield. So, value are pulled from a
generator by iterating over it (e.g., using for value in generator:) or by calling
next() on it.

    <event> -> TimeHandler -> KeyHandler -> MouseHandler -> NullHandler
"""

class NullHandler:

    def __init__(self, successor=None):
        self.__successor = successor

    def handle(self, event):
        if self.__successor is not None:
            self.__successor.handle(event)

class MouseHandler(NullHandler):

    def handle(self, event):
        if event.kind == Event.MOUSE:
            print("Click: {}".format(event))
        else:
            super().handle(event)

class DebugHandler(NullHandler):

    def __init__(self, successor=None, file=sys.stdout):
        super().__init__(successor)
        self.__file = file

    def handle(self, event):
        self.__file.write("*DEBUG*: {}\n".format(event))
        super().handle(event)


if __name__ == '__main__':

    handler1 = TimerHandler(KeyHandler(MouseHandler(NullHandler())))
    handler2 = DebugHandler(handler1)
```

Coroutine-based Chain

A coroutine uses the same yield expression as a generator but has different behavior.
A coroutine executes an infinite loop and starts out suspended at its first (or only)
yield expression, waiting for a value to be sent to it. If and when a value is sent,
the coroutine receives this as the value ofo its yield expression. The coroutine can then
do any processing it wants and when it has finished, it loops and again becomes suspended
waiting for a value to arrive at its next yield expression. So, values are pushed into a
coroutine by calling the coroutine's send() and throw() methods.


```python Coroutine-based Chain

def coroutine(function):
    @functools.wraps(function)
    def wrapper(*args, **kwargs):
        generator = function(*args, **kwargs)
        next(generator)
        return generator
    return wrapper

@coroutine
def key_handler(successor=None):
    while True:
        event = (yield)
        if event.kind == Event.KEYPRESS:
            print("Press: {}".format(event))
        elif successor is not None:
            successor.send(event)

@coroutine
def debug_handler(successor, file=sys.stdout):
    while True:
        event = (yield)
        file.write("*DEBUG*: {}\n".format(event))
        successor.send(event)

if __name__ == '__main__':

    pipeline = key_handler(mouse_handler(timer_handler()))
    pipeline - debug_handler(pipeline)
```

### 3.2. Command

The command pattern is used to encapsulate commands as objects. This makes it possible,
for example, to build up a sequence of commands for deferred execution or to create undoable
commands.


```python Command
class Grid:

    def __init__(self, width, height):
        pass

    def cell(self, x, y, color=None):
        pass

    @property
    def rows(self):
        pass

    @property
    def columns(self):
        pass


class UndoableGrid(Grid):

    def create_cell_command(self, x, y, color):
        def undo(self):
            pass

        def do(self):
            pass

        def create_rectangle_macro(self, x0, x0, x1, y1, color):
            pass

class Command:

    def __init__(self, do, undo, description=""):
        pass

    def __call__(self):
        self.do()

class Macro:

    def __init__(self, description=''):
        pass

    def add(self, command):
        pass

    def __call__(self):
        pass

    do = __call__

    def undo(self):
        pass

if __name__ == '__main__':

    grid = UndoableGrid(8, 3)
    redLeft = grid.create_cell_command(2, 1, "red")
    redRight = grid.create_cell_command(5, 0, "red")
    redLeft()
    redRight.do()
```

### 3.3. Interpreter

The interpreter pattern formalize two common requirements:

- providing some means by which users can enter nonstring values into applications
- allowing users to program applications

```python Interpreter: Expression Evaluataion with eval()/exec()/subprocess
import math

def global_context():
    globaContext = globals().copy()
    for name in dir(math):
        if not name.startswith("_"):
            globalContext[name] = getattr(math, name)
    return globalContext

def calculate(expression, globalContext, localContext, current):
    pass

def update(localContext, result, current):
    pass


if __name__ == '__main__':

    quit = "Ctrl+Z,Enter" if sys.platform.startswith("win") else "Ctrl+D"
    prompt = "Enter an expression ({} to quit): ".format(quit)
    current = types.SimpleNamespace(letter='A')
    globalContext = global_context()
    localContext = collections.OrderedDict()
    while True:
        try:
            expression = input(prompt)
            if expression:
                calculate(expression, globalContext, localContext, current)
        except EOFError:
            print()
            break

#--------------------------------------------------#

def execute(code, context):

    try:
        exec(code.code, globals(), context)
        result = context.gete("result)"
        error = context.get("error")
        handle_result(code, result, error)
    except Exception as err:
        print(err)

if __name__ == '__main__':

    context = dict(genome=genome, target='', replace='')
    execute(code, context)
```

### 3.4. Iterator

The iterator pattern provides a way of sequentially accessing the items inside a
collection or an aggregate object without exposing any of the internals of the
collection or aggregate's implementation.

```python SequenceProtocolIterators

class AtoZ:

    def __getitem__(self, index):
        if 0 <= index < 26:
            return chr(index + ord("A"))
        raise IndexError()

if __name__ == '__main__':

    for letter in AtoZ():
        print(letter, end='')

```

```python Two-Argument iter() Function Iterators

class Presidents:

    __names = ("George Washington", "John Adams", "Thomas Jefferson", ...)

    def __init__(self, first=None):
        self.index = (-1 if first is None else Presidents.__names.index(first)-1)

    def __call__(self):
        self.index += 1
        if self.index < len(Presidents.__names):
            return Presidents.__names(self.index)
        raise StopIteration()

if __name__ == '__main__':

    for president in iter(Presidents("George Bush"), None):
        print(president, end=" * ")
    print()
```

```python IteratorProtocolIterators
class Bag:

    def __init__(self, items=None):
        self.__bag = {}
        if items is not None:
            for item in items:
                self.add(item)

    def add(self, item):
        self.__bag[item] = self.__bag.get(item, 0) + 1

    def __delitem__(self, item):
        if self.__bag.get(item) is not None:
            self.__bag[item] -= 1
            if self.__bag[item] <= 0:
                del self.__bag[item]

        else:
            raise KeyError(str(item))

    def count(self, item):
        return self.__bag.get(item, 0)

    def __len__(self):
        return sum(count for count in self.__bag.values())

    def __contains__(self, item):
        return item in self.__bag

    def __iter__(self):
        items = []
        for item, count in self.__bag.items():
            for _ in range(count):
                items.append(item)
        return iter(items)
```


### 3.5. Mediator 

The mediator pattern provides a means of creating an object - the mediator - that can
encapsulate the interactions between other objects. This makes it possible to achieve
iteractions between objects that have no direct knowledge of each other.


```python Mediator
"""
Text   #1
Text   #2
             --> Mediator --> updateui()/clicked()
Button #1
Button #2
"""

class Form:

    def __init__(self):
        self.create_widgets()
        self.create_mediator()

    def create_widgets(self):
        self.nameText = Text()
        self.emailText = Text()
        self.okButton = Button("OK")
        self.cancelButton = Button("Cancel")

    def create_mediator(self):
        self.mediator = Mediator(((self.nameText, self.update_ui),
                                  (self.emailText, self.update_ui),
                                  (self.okButton, self.clicked),
                                  (self.cancelButton, self.clicked)))
        self.update_ui()

    def update_ui(self, widget=None):
        self.okButton.enabled = (bool(self.nameText.text) and bool(self.emailText.text))

    def clicked(self, widget):
        if widget == self.okButton:
            print("OK")
        elif widget == self.cancelButton:
            print("Cancel")

class Mediator:

    def __init__(self, widgetCallablePairs):
        self.callablesForWidget = collections.defaultdict(list)
        for widget, caller in widgetCallablePairs:
            self.callablesForWidget[widget].append(caller)
            widget.mediator = self

    def on_change(self, widget):
        callables = self.callablesForWidget.get(widget)
        if callables is not None:
            for caller in callables:
                caller(widget)
        else:
            raise AttributeError("No on_change() method registered for {}".format(widget))

class Mediated:

    def __init__(self):
        self.mediator = None

    def on_change(self):
        if self.mediator is not None:
            self.mediator.on_change(self)

class Button(Mediated):
        
    def __init__(self, text=""):
        super().__init__()
        self.enabled = True
        self.text = text

    def click(self):
        if self.enabled:
            self.on_change()

class Text(Mediated):
   
    def __init__(self, text=""):
        super().__init__()
        self.__text = text

    @property
    def text(self):
        return self.__text

    @text.setter
    def text(self, text):
        if self.text != text:
            self.__text = text
            self.on_change()
```

```python Coroutine-Based Mediator

def create_mediator(self):
    self.mediator = self._update_ui_mediator(self._clicked_mediator())
    for widget in (self.nameText, self.emailText, self.okButton, self.cancelButton):
        widget.mediator = self.mediator
    self.mediator.send(None)

@coroutine
def _update_ui_mediator(self, successor=None):
    while True:
        widget = (yield)
        self.okButton.enabled = (bool(self.nameText.text) and bool(self.emailText.text))

        if successor is not None:
            successor.send(widget)

@coroutine
def _clicked_mediator(self, successor=None):
    while True:
        widget = (yield)
        if widget == self.okButton:
            print("OK")
        elif widget == self.cancelButton:
            print("Cancel")
        elif successor is not None:
            successor.send(widget)

class Mediated:

    def __init__(self):
        self.mediator = None

    def on_change(self):
        if self.mediator is not None:
            self.mediator.send(self)
```


### 3.6. Memento 

The memento pattern is a means of saving and restoring an object's state without
violating encapsulation.

### 3.7. Observer 

The observer pattern supports many-to-many dependency relationships between objects,
such that when one object changes state, all its related objects are notified.

Nowadays, probably the most commmon expression of this pattern and its variants is
the mode/view/controller(MVC) paradigm. In this paradigm, a model represents data,
one or more views visualize that data, and one or more controllers mediate between
input(e.g. user interaction) and the model. And any changes to the model are
automatically reflected in the associated views.


```python Observer
"""
SliderModel
  |----> HistoryView
  |----> LiveView
"""

class Observed:

    def __init__(self):
        self.__observers = set()

    def observers_add(self, observer, *observers):
        for observer in itertools.chain((observer,), observers):
            self.__observers.add(observer)
            observer.update(self)

    def observer_discard(self, observer):
        self.__observers.discard(observer)

    def observers_notify(self):
        for observer in self.__observers:
            observer.update(self)

class SliderModel(Observed):

    def __init__(self, minimum, value, maximum):
        super().__init_()
        self.__minimum = self.__value = self.__mximum = None
        self.minimum = minimum
        self.value = value
        self.maximum = maximum

    @property
    def value(self):
        return self.__value

    @value.setter
    def value(self, value):
        if self.__value != value:
            self.__value = value
            self.observers_notify()

class HistoryView:

    def __init__(self):
        self.data = []

    def update(self, model):
        self.data.append((model.value, time.time()))

class LiveView:

    def __init__(self, length=40):
        self.length = length

    def update(self, model):
        tippingPoint = round(model.value * self.length / (model.maximum - model.minimum))
        td = ""
        html = ""
        html.extend("")
        print("".join(html))


if __name__ == '__main__':

    historyView = HistoryView()
    liveView = LiveView()
    model = SliderModel(0, 0, 40)
    model.observers_add(historyView, liveView)
    for value in (7, 23, 37):
        model.value = value
    for value, timestamp in historyView.data:
        print("{:3} {}".format(value, datetime.datetime.fromtimestamp(timestamp)), file=sys.stderr)
```

### 3.8. State 

The state pattern is intended to provide objects whose behavior changes when their state
changes; that is, objects that have modes.

```python State
class Counter:

    def __init__(self, *names):
        pass

    def __call__(self, event):
        pass

class Event:

    def __init__(self, name, count=1):
        pass

class Multiplexer:

    ACTIVE, DORMANT = ("ACTIVE", "DORMANT")

    def __init__(self):
        pass

    def connect(self, eventName, callback):
        pass

    def disconnect(self, eventName, callback=None):
        pass

    def send(self, event):
        pass

    # @property
    # def state(self):
    #     pass

    @state.setter
    def state(self, state):
        pass

    def __active_connect(self, eventName, callback):
        pass


if __name__ == '__main__':

    totalCounter = Counter()
    carCounter = Counter("cars")
    commercialCounter = Counter("vans", "trucks")

    multiiplexer = Multiplexer()
    for eventName, callback in (("cars", carCounter), 
                                ("vans", commercialCounter),
                                ("trucks", commercialCounter)):
        multiplexer.connect(eventName, callback)
        multiplexer.connect(eventName, totalCounter)
```

### 3.9. Strategy 

The strategy pattern provides a means of encapsulating a set of algorithms that
can be used interchangeably, depending on the user's needs.

```python Strategy
WINNERS = ("Niko", "Mat", "Birgit", "Sawao")

class Layout:

    def __init__(self, tabulator):
        self.tabulator = tabulator

    def tabulate(self, rows, items):
        return self.tabulator(rows, items)


class Layout:

    def __init__(self, tabulator):
        self.tabulate = tabulator

    def html_tabulator(rows, items):
        pass

if __name__ == '__main__':

    htmlLayout = Layout(html_tabulator)
    for rows in range(2, 6):
        print(htmlLayout.tabulate(rows, WINNERS))
   
    textLayout = Layout(text_tabulator)
    for rows in range(2, 6):
        print(textLayout.tabulate(rows, WINNERS)
```

### 3.10. Template method 

The template method pattern allows us to define the steps of an algorithm but defer the
execution of some of those steps to subclasses.

```python TemplateMethod
def count_words(filename):
    pass

class AbstractWordCounter:

    @staticmethod
    def can_count(filename):
        raise NotImplementedError()

    @staticmethod
    def count(filename):
        raise NotImplementedError()

class AbstractWordCounter(metaclass=abc.ABCMeta):

    @staticmethod
    @abc.abcstractmethod
    def can_count(filename):
        pass

    @staticmethod
    @abc.abcstractmethod
    def count(filename):
        pass

class PlainTextWordCounter(AbstractWordCounter):

    @staticmethod
    def can_count(filename):
        pass

    @staticmethod
    def count(filename):
        pass

class HtmlWordCounter(AbstactWordCounter):

    @staticmethod
    def can_count(filename):
        pass

    @staticmethod
    def count(filename):
        pass

    class __HtmlParser(html.parser.HTMLParser):
   
        def __init__(self):
            super().__init__()
            pass

        def handle_starttag(self, tag, attrs):
            pass

        def handle_endtag(self, tag):
            pass

        def handle_data(self, text):
            pass
```

### 3.11. Visitor 

The visitor pattern is used to apply a function to every item in a collection or
aggregate object.
