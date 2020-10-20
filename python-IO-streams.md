# Python IO streams in examples

[
  ](https://medium.com/@rysartem?source=post_page-----97d2c4367207--------------------------------)------------------)

We’ll be using [Python IO streams](https://docs.python.org/3/library/io.html): **BytesIO** and **StringIO** to execute some tasks from the real world: sending a photo to [Telegram Bot](https://core.telegram.org/bots) and get configuration from [Netflix Config Server](https://github.com/spring-cloud/spring-cloud-config) (now it’s a part of Spring Cloud).

We are going to cover 2 different stream classes:

- [**BytesIO**](https://docs.python.org/3/library/io.html#binary-i-o) — expects *binary-like* objects and produces *bytes* objects
- [**StringIO**](https://docs.python.org/3/library/io.html#text-i-o) — expects and produces *str* objects

# BytesIO

For example, you are writing a **Telegram bot** that sends your user photos from **Unsplash** website. The **intuitive** way to code this task is to **save** the photo to the disk and then **read** from that file and **send** the photo to Telegram, at least, I thought so. But, there is a **better way** to do it using **Python streams**.

Intuitive way:

```python
import requests
import os

r = requests.get("https://images.unsplash.com/photo-1452857576997-f0f12cd77844?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2850&q=80")
with open("photo.jpg", "wb") as f:
    f.write(r.content)

requests.post(
    "https://api.telegram.org/bot<TOKEN>/sendPhoto?chat_id=<chat_id>",
    files={"photo": open("photo.jpg", "rb")}
)
os.remove("photo.jpg")
```



<iframe src="https://medium.com/media/fd5d3a20b5c895839dd74e5bbdb5176d" allowfullscreen="" frameborder="0" height="315" width="680" title="Save file from Unsplash and send it to Telegram bot" class="t u v hr ak" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 315px;"></iframe>

Python stream way:

```python
import requests
import io

r = requests.get("https://images.unsplash.com/photo-1452857576997-f0f12cd77844?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2850&q=80")
with io.BytesIO() as buf:
    buf.write(r.content)
    buf.seek(0)

    requests.post(
        "https://api.telegram.org/bot<TOKEN>/sendPhoto?chat_id=<chat_id>",
        files={"photo": buf}
    )
```



<iframe src="https://medium.com/media/9151efc8a6f0686eccbe4643535ed4d5" allowfullscreen="" frameborder="0" height="315" width="680" title="IO file from Unsplash and send it to Telegram bot" class="t u v hr ak" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 315px;"></iframe>

Using **with** statement so we don’t need to close buffer manually

Let’s discuss the difference between these 2 approaches. Obviously, the biggest one is that **you don’t need to save a file to the disk**, this leads to better performance. Using **streams is better** cause of the **speed of using RAM** to process bytes-like object, rather than **using HDD/SSD** to save it on disk and then read it from disk.

# StringIO

Another example is to parse and save the configuration from **Java Netflix Config Server**. Yeah, this is a *very unusual case*, but still :) Of course, it could be any text-like data file.

I had such a task at my current job. Our project has a big part written in Java and uses **Netflix stack**, including **Netflix Config Server**. All **configurations** for microservices and different environments are **stored there**. *It provides an ability to get any configuration by requesting a specific link.* IMHO, it is a great way to handle different configurations and I am looking for a configuration server written in Python, maybe sometimes I’ll write it by myself.

You can use the function `get_properties_from_config_server` giving it an link that your configuration contains and it returns`dict` object with all configuration values.

```python
import io
import jprops

def get_properties_from_config_server(link):
    r = requests.get(link)
    f = io.StringIO(r.text)
    props = jprops.load_properties(f)
    return props
```



<iframe src="https://medium.com/media/fa9ce4d2b389e1e0e8bff9f43d6ec9c7" allowfullscreen="" frameborder="0" height="212" width="680" title="IO file from Config Server and parse it" class="t u v hr ak" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 212px;"></iframe>

**StringIO** **approach** handling configuration file is better cause you don’t need to choose the **storage directory**. In my example, I am working on MacBook and my microservice is being deployed using Docker. And I used`/tmp` folder to store this configuration file, then read from it and deleted this file. **But!** Another my colleague is using Windows. His OS doesn’t have `/tmp` folder. And, in my opinion, it is a bad way to adjust your code to be using folder that suits all the OS, **you can just use StringIO to store a config file in RAM**.

# Finally

We have discussed **BytesIO** and **StringIO** classes from Python [io](https://docs.python.org/3/library/io.html) standard library, have written some code that covers sending a photo to Telegram bot and read a configuration file from Netflix Config Server. Of course, you can use Python streams for your own tasks.