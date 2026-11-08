# Python Requests API 

This package (library) is how in python we send and HTTP request and handle the response from the target server

# How to install the `Requests` API

To use this package you need to install it onto your system using `pip` 

```python
pip install requests
#or
pip3 install requests
```

then we need to in a python file Import the module so we can use it

```python
import requests
```

# how to make a request

The basic format for a request is as follows 

```python
response = requests.<request_method>("target url/host", <requestbody>)
```
we can use the requests package for all of the main HTTP request types

```python
#GET
response = requests.get('http://api.open-notify.org/iss-now.json')
#POST
response = requests.post('https://httpbin.org/post', data={'key': 'value'})
# PUT
response = requests.put('https://httpbin.org/put', data={'key': 'value'})
# PATCH
response = requests.patch('https://httpbin.org/patch', data={'key':'value'})
# DELETE
response = requests.delete('https://httpbin.org/delete')
```
the HTTP response package will be accessable through the `response` variable

## `GET` request

as we saw above we can handle a standard `GET` request simply by using the `requests.get()` method

Example:
```python
response = requests.get("http://api.open-notify.org/iss-now.json")
```
### Passing URL Peramaters

You can send the url perameters as a `dict` for the get method to translate to an URL string

```python
parameters = {"lat": 48.8566, "lon": 2.3522} 
response = requests.get("http://api.open-notify.org/iss-now.json", params=parameters)
```
this is the same as passing the following URL through a web browser

```
 http://api.open-notify.org/iss-now.json?lat=48.8566&lon=2.3522
```

you can check the url programaticly by accessing the `url` attribute on the request object

```python
print(request.url)
# http://api.open-notify.org/iss-now.json?lat=48.8566&lon=2.3522
```
:::warning
Note that any dictionary key whose value is `None` will not be added to the URL’s query string.
:::
## `Post` Requests

the basic syntax of a POST request is
```python
requests.post(url, data={key: value}, json={key: value}, headers={key:value}, args) #(data, json, headers parameters are optional.)
```
### sending JSON Data
When you use the `json= parameter`, requests automatically converts your dictionary into a JSON string and sets the **Content-Type** header to `application/json` for you.
```python
# 1. Define the API endpoint URL
url = 'https://httpbin.org/post'

# 2. Define your custom headers
headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Accept': 'application/json'
}

# 3. Define the request body (payload)
payload = {
    'name': 'John Doe',
    'email': 'john@example.com'
}

# 4. Make the POST request
response = requests.post(url, headers=headers, json=payload)

# 5. Check and view the response
print(f"Status Code: {response.status_code}")
print(response.json())
```
### Sending Form-Encoded Data
If the receiving server expects form data, use the `data= parameter` instead. This automatically sets the **Content-Type** header to `application/x-www-form-urlencoded`

```python
url = 'https://httpbin.org/post'

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN'
}

# Payload for form-encoded data
payload = {
    'username': 'johndoe123',
    'action': 'login'
}

# Use data= instead of json=
response = requests.post(url, headers=headers, data=payload)

print(f"Status Code: {response.status_code}")
print(response.text)
```

## passing custom headers to the request
If you’d like to add HTTP headers to a request, simply pass in a `dict` to the `headers` parameter.

for example:
```python
url = 'https://api.github.com/some/endpoint'
headers = {'user-agent': 'my-app/0.0.1'}
response = requests.get(url, headers=headers)
```
most often this is used for sending *Authentication*


# The Response object

The `response` object contains the entire response package (status codes, headers, response body)

```python
import requests

response = requests.get("http://api.open-notify.org/iss-now.json")

print(response)
# <Response [200]> returns a response object with the status code
```

## Accessing the status code
use the `status_code` attribute
```python
response = requests.get("http://api.open-notify.org/iss-now.json")
# Print the status code of the response.
print(response.status_code)
# 200 
```
## content (response body)
you can access the response body as a text
```python
response = requests.get("http://api.open-notify.org/iss-now.json")
# Print the content of the response.
print(response.text)
# {"message": "success", "iss_position": {"longitude": "-32.3037", "latitude": "-12.7147"}, "timestamp": 1588684375} 
```
which returns a text string in the json format (in this case) 
we can convert it to a python `dict` with the `json.loads()` function
or we can access the request as a JSON
```python
response = requests.get("http://api.open-notify.org/iss-now.json")
# Print the content of the response in Python
print(response.json())
# {'message': 'success', 'iss_position': {'longitude': '-29.3427', 'latitude': '-16.5135'}, 'timestamp': 1588684451}  
```
which converts it dirctly to a python dict.

:::Info
you can also get binary data which can convert to an image or other non-text responses. 
:::

## Response Headers
we can access the response headers by
```python
response = requests.get("http://api.open-notify.org/iss-now.json")
print(response.header)
# {'Server': 'nginx/1.10.3', 'Date': 'Tue, 05 May 2020 13:16:04 GMT', 'Content-Type': 'application/json', 'Content-Length': '114', 'Connection': 'keep-alive', 'access-control-allow-origin': '*'}
```

# Accessing the request

we can access the request that we sent to the target server by using the `request` attribut on the response object

```python
response = requests.get("http://api.open-notify.org/astros.json")
data = response.text
print(response.request.url)
print(response.request.body)
# http://api.open-notify.org/astros.json                                   
# None              
```
# Summery
- install the `requests` package using `pip install`
- The request has a standard format and can handle many different HTTP request types
- we can access the response information via the `response` obj
- we can send custom headers to the target server with the request package