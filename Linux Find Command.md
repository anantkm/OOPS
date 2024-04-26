# Simplified Python Implementation of the Linux Find Command

This document describes the design and implementation of a simplified version of the Linux `find` command in Python. This script provides functionality to search for files based on specific filters such as minimum size and file extension.

## Classes and Functions

### File Class

Represents a file or directory in the system.

```python
class File:
    def __init__(self, name, size):
        self.name = name
        self.size = size
        self.children = []
        self.is_directory = False if '.' in name else True
        self.extension = name.split(".")[1] if '.' in name else ""

    def __repr__(self):
        return "{" + self.name + "}"

```
### Abstract Filter Class
Defines an interface for filters.

``` python
from abc import ABC, abstractmethod

class Filter(ABC):
    @abstractmethod
    def apply(self, file):
        pass

```
### MinSizeFilter Class
Filters files that are larger than a specified size.

``` python
class MinSizeFilter(Filter):
    def __init__(self, size):
        self.size = size

    def apply(self, file):
        return file.size > self.size
```
### ExtensionFilter Class
```python
class ExtensionFilter(Filter):
    def __init__(self, extension):
        self.extension = extension

    def apply(self, file):
        return file.extension == self.extension

```
### LinuxFind Class
Implements the core functionality of the Linux `find` command.

```python
from collections import deque
from typing import List

class LinuxFind():
    def __init__(self):
        self.filters: List[Filter] = []

    def add_filter(self, given_filter):
        self.filters.append(given_filter)

    def apply_OR_filtering(self, root):
        found_files = []
        queue = deque([root])
        while queue:
            current = queue.popleft()
            if current.is_directory:
                queue.extend(current.children)
            else:
                if any(f.apply(current) for f in self.filters):
                    found_files.append(current)
        return found_files

    def apply_AND_filtering(self, root):
        found_files = []
        queue = deque([root])
        while queue:
            current = queue.popleft()
            if current.is_directory:
                queue.extend(current.children)
            else:
                if all(f.apply(current) for f in self.filters):
                    found_files.append(current)
        return found_files
```

### Example Usage
``` python
def main():

    f1 = File("root_300", 300)
    f2 = File("fiction_100", 100)
    f3 = File("action_100", 100)
    f4 = File("comedy_100", 100)
    f1.children = [f2, f3, f4]

    f5 = File("StarTrek_4.txt", 4)
    f6 = File("StarWars_10.xml", 10)
    f7 = File("JusticeLeague_15.txt", 15)
    f8 = File("Spock_1.jpg", 1)
    f2.children = [f5, f6, f7, f8]

    f9 = File("IronMan_9.txt", 9)
    f10 = File("MissionImpossible_10.rar", 10)
    f11 = File("TheLordOfRings_3.zip", 3)
    f3.children = [f9, f10, f11]

    f11 = File("BigBangTheory_4.txt", 4)
    f12 = File("AmericanPie_6.mp3", 6)
    f4.children = [f11, f12]


    greater5_filter = MinSizeFilter(5)
    txt_filter = ExtensionFilter("txt")

    my_linux_find = LinuxFind()
    my_linux_find.add_filter(greater5_filter)
    my_linux_find.add_filter(txt_filter)

    print(my_linux_find.apply_OR_filtering(f1))
    print(my_linux_find.apply_AND_filtering(f1))

if __name__ == "__main__":
    main()
```