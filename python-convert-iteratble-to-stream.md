# [How to convert an iterable to a stream?](https://stackoverflow.com/questions/6657820/how-to-convert-an-iterable-to-a-stream)

[Ask Question](https://stackoverflow.com/questions/ask)

If I've got an iterable containing strings, is there a simple way to turn it into a stream? I want to do something like this:

```python
def make_file():
    yield "hello\n"
    yield "world\n"

output = tarfile.TarFile(…)
stream = iterable_to_stream(make_file())
output.addfile(…, stream)
```

**ANS**

Here's my streaming iterator an experimental branch of [urllib3](http://code.google.com/p/urllib3/) supporting streaming chunked request via iterables:

```python
class IterStreamer(object):
    """
    File-like streaming iterator.
    """
    def __init__(self, generator):
        self.generator = generator
        self.iterator = iter(generator)
        self.leftover = ''

    def __len__(self):
        return self.generator.__len__()

    def __iter__(self):
        return self.iterator

    def next(self):
        return self.iterator.next()

    def read(self, size):
        data = self.leftover
        count = len(self.leftover)

        if count < size:
            try:
                while count < size:
                    chunk = self.next()
                    data += chunk
                    count += len(chunk)
            except StopIteration:
                self.leftover = ''; return data

        self.leftover = data[size:]

        return data[:size]
```

------------------



Python 3 has [a new I/O stream API](http://www.python.org/dev/peps/pep-3116/) ([library docs](http://docs.python.org/dev/library/io.html)), replacing the old file-like object protocol. (The new API is also available in Python 2 in the [`io`](http://docs.python.org/2.7/library/io.html) module, and it's backwards-compatible with the file-like object protocol.)

[Here's an implementation for the new API](https://gist.github.com/mechanical-snail/7688353), in Python 2 and 3:

```python
import io

def iterable_to_stream(iterable, buffer_size=io.DEFAULT_BUFFER_SIZE):
    """
    Lets you use an iterable (e.g. a generator) that yields bytestrings as a read-only
    input stream.

    The stream implements Python 3's newer I/O API (available in Python 2's io module).
    For efficiency, the stream is buffered.
    """
    class IterStream(io.RawIOBase):
        def __init__(self):
            self.leftover = None
        def readable(self):
            return True
        def readinto(self, b):
            try:
                l = len(b)  # We're supposed to return at most this much
                chunk = self.leftover or next(iterable)
                output, self.leftover = chunk[:l], chunk[l:]
                b[:len(output)] = output
                return len(output)
            
            except StopIteration:
                return 0    # indicate EOF
            
    return io.BufferedReader(IterStream(), buffer_size=buffer_size)
```

Example usage:

```python
with iterable_to_stream(str(x**2).encode('utf8') for x in range(11)) as s:
    print(s.read())
```

--------------

Since it doesn't look like there is a "standard" way of doing it, I've banged together a simple implementation:

```python
class iter_to_stream(object):
    def __init__(self, iterable):
        self.buffered = ""
        self.iter = iter(iterable)

    def read(self, size):
        result = ""
        while size > 0:
            data = self.buffered or next(self.iter, None)
            self.buffered = ""
            if data is None:
                break
            size -= len(data)
            if size < 0:
                data, self.buffered = data[:size], data[size:]
            result += data
        return result
```