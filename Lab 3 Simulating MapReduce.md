### Creating a simple dataset.

~~~
%%writefile weblogs.txt
# Date, Time, IP, Method, URL, Status, ResponseSize
2025-10-10,12:01:32,192.168.1.2,GET,/index.html,200,1024
2025-10-10,12:01:33,192.168.1.3,GET,/products.html,200,850
2025-10-10,12:01:35,192.168.1.4,GET,/contact.html,404,512
2025-10-10,12:01:38,192.168.1.5,POST,/checkout,500,128
2025-10-10,12:01:41,192.168.1.6,GET,/index.html,200,1024
2025-10-10,12:01:45,192.168.1.7,GET,/images/logo.png,200,256
2025-10-10,12:01:48,192.168.1.8,GET,/about.html,404,512
2025-10-10,12:01:53,192.168.1.9,POST,/login,403,64
2025-10-10,12:02:01,192.168.1.10,GET,/index.html,200,1024
2025-10-10,12:02:07,192.168.1.11,POST,/checkout,500,128
2025-10-10,12:02:12,192.168.1.12,GET,/contact.html,404,512
2025-10-10,12:02:15,192.168.1.13

--------------------------------
Writing weblogs.txt
~~~

### Implement the Mapper

~~~
# Mapper: Extract (StatusCode, 1)
def mapper(line):
    fields = line.strip().split(',')
    if len(fields) != 7 or fields[0].startswith('#'):
        return []
    # TODO: extract the status code (5th or 6th field)
    status = fields[5]
    return [(status, 1)]
    ~~~

### Shuffle Phase

~~~
from collections import defaultdict

def shuffle(mapped_data):
    grouped = defaultdict(list)
    for key, value in mapped_data:
        grouped[key].append(value)
    return grouped

~~~

Reducer Phase

~~~
from collections import defaultdict

def reducer(mapped_data):
    grouped = defaultdict(int)
    for key, value in mapped_data:
        grouped[key] += value
    return grouped
~~~

### Combine the Phases

~~~
mapped = []
with open("weblogs.txt", "r") as f:
    for line in f:
        mapped.extend(mapper(line))

reduced = reducer(mapped)

for code, count in sorted(reduced.items()):
    print(f"HTTP {code}: {count} requests")

---------------------------------
    HTTP Ellipsis: 20 requests
~~~