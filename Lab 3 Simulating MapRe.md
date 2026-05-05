~~~python

data = [
    "2025-10-10,12:01:32,192.168.1.2,GET,/index.html,200,1024",
"2025-10-10,12:01:33,192.168.1.3,GET,/products.html,200,850",
"2025-10-10,12:01:35,192.168.1.4,GET,/contact.html,404,512",
"2025-10-10,12:01:38,192.168.1.5,POST,/checkout,500,128",
"2025-10-10,12:01:41,192.168.1.6,GET,/index.html,200,1024",
"2025-10-10,12:01:45,192.168.1.7,GET,/images/logo.png,200,256",
"2025-10-10,12:01:48,192.168.1.8,GET,/about.html,404,512",
"2025-10-10,12:01:53,192.168.1.9,POST,/login,403,64",
"2025-10-10,12:02:01,192.168.1.10,GET,/index.html,200,1024",
"2025-10-10,12:02:07,192.168.1.11,POST,/checkout,500,128",
"2025-10-10,12:02:12,192.168.1.12,GET,/contact.html,404,512",
"2025-10-10,12:02:15,192.168.1.13,GET,/index.html,200,1024",
"2025-10-10,12:02:21,192.168.1.14,GET,/products.html,200,850",
"2025-10-10,12:02:23,192.168.1.15,GET,/about.html,404,512",
"2025-10-10,12:02:29,192.168.1.16,POST,/checkout,500,128",
"2025-10-10,12:02:31,192.168.1.17,GET,/images/logo.png,200,256",
"2025-10-10,12:02:34,192.168.1.18,GET,/contact.html,404,512",
"2025-10-10,12:02:38,192.168.1.19,POST,/login,403,64",
"2025-10-10,12:02:41,192.168.1.20,GET,/index.html,200,1024",
"2025-10-10,12:02:47,192.168.1.21,GET,/products.html,200,850"

]

# MAP
mapped = []
for line in data:
    parts = line.split()
    url = parts[1]
    mapped.append((url, 1))

# REDUCE
result = {}
for url, count in mapped:
    result[url] = result.get(url, 0) + count

print(result)
~~~
~~~5

/index → 1
/login → 2
/home → 1
~~~

~~~python
mapped = []

for line in data:
    parts = line.split()
    status = parts[2]
    size = int(parts[3])
    mapped.append((status, size))

result = {}

for status, size in mapped:
    result[status] = result.get(status, 0) + size

print(result)
~~~

~~~5
200 → 1024 + 850 + 1024 + 256 + 1024 + 1024 + 850 + 256 + 1024 + 850
403 → 64   + 64
404 → 512  + 512  + 512 + 512 + 512
500 → 128  + 128  + 128
~~~

~~~python
mapped = []

for line in data:
    parts = line.split()
    status = parts[2]

    if status == "200":
        mapped.append(("success", 1))
    else:
        mapped.append(("error", 1))

result = {}

for key, value in mapped:
    result[key] = result.get(key, 0) + value

print(result)
~~~

~~~5
success → 10
error → 10
~~~
