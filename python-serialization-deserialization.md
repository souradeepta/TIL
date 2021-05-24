# Serialization and Deserialization of Python Objects: Part 1

Python object serialization and deserialization is an important aspect of any non-trivial program. If in Python you save something to a file, if you read a configuration file, or if you respond to an HTTP request, you do object serialization and deserialization. 

In one sense, serialization and deserialization are the most boring things in the world. Who cares about all the formats and protocols? You just want to persist or stream some Python objects and get them back later intact. 

This is a very healthy way to look at the world at the conceptual level. But, at the pragmatic level, which serialization scheme, format or protocol you choose may determine how fast your program runs, how secure it is, how much freedom you have to maintain your state, and how well you're going to interoperate with other systems. 

The reason there are so many options is that different circumstances call for different solutions. There is no "one size fits all". In this two-part tutorial I'll go over the pros and cons of the most successful serialization and deserialization schemes, show how to use them, and provide guidelines for choosing between them when faced with a specific use case.

## Running Example

In the following sections I'll serialize and deserialize the same Python object graphs using different serializers. To avoid repetition, I'll define these object graphs here.

### Simple Object Graph

The simple object graph is a dictionary that contains a list of integers, a string, a float, a boolean, and a None.

```python
simple = dict(int_list=[1, 2, 3],
 
              text='string',
 
              number=3.44,
 
              boolean=True,
 
              none=None)
```

### Complex Object Graph

The complex object graph is also a dictionary, but it contains a `datetime` object and user-defined class instance that has a `self.simple` attribute, which is set to the simple object graph.

```python
from datetime import datetime
 
class A(object):
 
    def __init__(self, simple):
 
        self.simple = simple        
 
    def __eq__(self, other):
 
        if not hasattr(other, 'simple'):
 
            return False
 
        return self.simple == other.simple
 
    def __ne__(self, other):
 
        if not hasattr(other, 'simple'):
 
            return True
 
        return self.simple != other.simple
```

## Pickle

Pickle is a staple. It is a native Python object serialization format. The pickle interface provides four methods: dump, dumps, load, and loads. The `dump()` method serializes to an open file (file-like object). The `dumps()` method serializes to a string. The `load()` method deserializes from an open file-like object. The `loads()` method deserializes from a string.

Pickle supports by default a textual protocol, but has also a binary protocol, which is more efficient, but not human-readable (helpful when debugging).

Here is how you pickle a Python object graph to a string and to a file using both protocols.

```python
import cPickle as pickle
 
pickle.dumps(simple)
 
"(dp1\nS'text'\np2\nS'string'\np3\nsS'none'\np4\nNsS'boolean'\np5\nI01\nsS'number'\np6\nF3.4399999999999999\nsS'int_list'\np7\n(lp8\nI1\naI2\naI3\nas."
 
 
 
pickle.dumps(simple, protocol=pickle.HIGHEST_PROTOCOL)
 
'\x80\x02}q\x01(U\x04textq\x02U\x06stringq\x03U\x04noneq\x04NU\x07boolean\x88U\x06numberq\x05G@\x0b\x85\x1e\xb8Q\xeb\x85U\x08int_list]q\x06(K\x01K\x02K\x03eu.'

```

The binary representation may seem larger, but this is an illusion due to its presentation. When dumping to a file, the textual protocol is 130 bytes, while the binary protocol is only 85 bytes.

```python
pickle.dump(simple, open('simple1.pkl', 'w'))
 
pickle.dump(simple, open('simple2.pkl', 'wb'), protocol=pickle.HIGHEST_PROTOCOL)
 
 
 
ls -la sim*.*
 
-rw-r--r--  1 gigi  staff  130 Mar  9 02:42 simple1.pkl
 
-rw-r--r--  1 gigi  staff   85 Mar  9 02:43 simple2.pkl
```

Unpickling from a string is as simple as:

```python
x = pickle.loads("(dp1\nS'text'\np2\nS'string'\np3\nsS'none'\np4\nNsS'boolean'\np5\nI01\nsS'number'\np6\nF3.4399999999999999\nsS'int_list'\np7\n(lp8\nI1\naI2\naI3\nas.")
 
assert x == simple
 
 
 
x = pickle.loads('\x80\x02}q\x01(U\x04textq\x02U\x06stringq\x03U\x04noneq\x04NU\x07boolean\x88U\x06numberq\x05G@\x0b\x85\x1e\xb8Q\xeb\x85U\x08int_list]q\x06(K\x01K\x02K\x03eu.')
 
assert x == simple
```

Note that pickle can figure out the protocol automatically. There is no need to specify a protocol even for the binary one.

Unpickling from a file is just as easy. You just need to provide an open file.

```python
 x = pickle.load(open('simple1.pkl'))
 
assert x == simple
 
 
 
x = pickle.load(open('simple2.pkl'))
 
assert x == simple
 
 
 
x = pickle.load(open('simple2.pkl', 'rb'))
 
assert x == simple

```

According to the documentation, you're supposed to open binary pickles using the 'rb' mode, but as you can see it works either way.

Let's see how pickle deals with the complex object graph.

```python
pickle.dumps(complex)
 
"(dp1\nS'a'\nccopy_reg\n_reconstructor\np2\n(c__main__\nA\np3\nc__builtin__\nobject\np4\nNtRp5\n(dp6\nS'simple'\np7\n(dp8\nS'text'\np9\nS'string'\np10\nsS'none'\np11\nNsS'boolean'\np12\nI01\nsS'number'\np13\nF3.4399999999999999\nsS'int_list'\np14\n(lp15\nI1\naI2\naI3\nassbsS'when'\np16\ncdatetime\ndatetime\np17\n(S'\\x07\\xe0\\x03\\x07\\x00\\x00\\x00\\x00\\x00\\x00'\ntRp18\ns."
 
 
 
pickle.dumps(complex, protocol=pickle.HIGHEST_PROTOCOL)
 
'\x80\x02}q\x01(U\x01ac__main__\nA\nq\x02)\x81q\x03}q\x04U\x06simpleq\x05}q\x06(U\x04textq\x07U\x06stringq\x08U\x04noneq\tNU\x07boolean\x88U\x06numberq\nG@\x0b\x85\x1e\xb8Q\xeb\x85U\x08int_list]q\x0b(K\x01K\x02K\x03eusbU\x04whenq\x0ccdatetime\ndatetime\nq\rU\n\x07\xe0\x03\x07\x00\x00\x00\x00\x00\x00\x85Rq\x0eu.'
 
 
 
pickle.dump(complex, open('complex1.pkl', 'w'))
 
pickle.dump(complex, open('complex2.pkl', 'wb'), protocol=pickle.HIGHEST_PROTOCOL)
 
 
 
ls -la comp*.*
 
-rw-r--r--  1 gigi  staff  327 Mar  9 02:58 complex1.pkl
 
-rw-r--r--  1 gigi  staff  171 Mar  9 02:58 complex2.pkl
```

The efficiency of the binary protocol is even greater with complex object graphs.