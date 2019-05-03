# requests library Examples

- Example 1 - shibe.online 
- Example 2 - github

# Example 1

dog_req.py
```py
# First thing we'll do is import the requests library
import requests

# Define a variable with the URL of the API
api_url = "http://shibe.online/api/shibes?count=1"

# Call the root of the api with GET, store the answer in a response variable
# This call will return a list of URLs that represent dog pictures
response = requests.get(api_url)

# Get the status code for the response. Should be 200 OK
# Which means everything worked as expected
print(f"Response status code is: {response.status_code}")

# Get the result as JSON
response_json = response.json()

# Print it. We should see a list with one image URL.
print(response_json)
```

```py
python3 dog_req.py  # Execute

# Output
Response status code is: 200
['https://cdn.shibe.online/shibes/28d7c372ea7defdb315ef845285d4ac3906ccea4.jpg']
```

#### Passing in Parameters

```py
# We'll store our base URL here and pass in the count parameter later
api_url = "http://shibe.online/api/shibes"

params = {
   "count": 10
}

# Pass those params in with the request.
api_response = requests.get(api_url, params=params)

print(f"Shibe API Response Status Code is: {api_response.status_code}")  # should be 200 OK

json_data = api_response.json()

print("Here is a list of URLs for dog pictures:")
for url in json_data:
    print(f"\t {url}")
```

```py
$ python3 dog_req.py # Execute

# Output

Shibe API Response Status Code is: 200
Here is a list of URLs for dog pictures:
     https://cdn.shibe.online/shibes/dfb2af0b2ac1f057750da32f0ea0e154afc160cf.jpg
     https://cdn.shibe.online/shibes/4989daad2c805ec62b0fb09a80280ba2262f1b08.jpg
     ...
```

# Example 2

[code](https://www.learnpython.dev/02-introduction-to-python/190-apis/final-exercise/)

```py
import requests

def repos_with_most_stars():
    gh_api_repo_search_url = "https://api.github.com/search/repositories"

    response = requests.get(gh_api_repo_search_url)

    print(response.text)

if __name__ == "__main__":
    repos_with_most_stars()
```    

```py
python3 day_one.py # Execute

# Output
{"message":"Validation Failed","errors":[{"resource":"Search","field":"q","code":"missing"}],"documentation_url":"https://developer.github.com/v3/search"}
```

```py
import requests

def repos_with_most_stars():
    gh_api_repo_search_url = "https://api.github.com/search/repositories"

    parameters = {"q": "stars:>50000"}
    response = requests.get(gh_api_repo_search_url, params=parameters)

    print(response.text)

if __name__ == "__main__":
    repos_with_most_stars()
```    

```py
python3 day_one.py # Execute

# Output
{"total_count":33,"incomplete_results":false,"items":[{"id":28457823,"node_id":"MDEwOlJlcG9zaXRvcnkyODQ1NzgyMw==","name":"freeCodeCamp"...
```

### Response Parsing

```py
import requests

def repos_with_most_stars():
    gh_api_repo_search_url = "https://api.github.com/search/repositories"

    parameters = {"q": "stars:>50000"}
    response = requests.get(gh_api_repo_search_url, params=parameters)
    response_json = response.json()

    return response_json["items"]


if __name__ == "__main__":
    results = repos_with_most_stars()

    for result in results:
        language = result["language"]
        stars = result["stargazers_count"]
        name = result["name"]

        print(f"-> {name} is a {language} repo with {stars} stars.")
```        

```py
python3 day_one.py # Execute

# output
-> freeCodeCamp is a JavaScript repo with 298059 stars.
-> bootstrap is a JavaScript repo with 131410 stars.
-> vue is a JavaScript repo with 130168 stars.
-> react is a JavaScript repo with 124029 stars.
-> tensorflow is a C++ repo with 122328 stars.
-> free-programming-books is a None repo with 118241 stars.
-> awesome is a None repo with 103392 stars.
-> You-Dont-Know-JS is a None repo with 97587 stars.
...
```

### Constructing Query

```py
def create_query(languages, min_stars=50000):
    # An example search query looks like:
    # stars:>50000 language:python language:javascript

    query = f"stars:>{min_stars} "

    for language in languages:
        query += f"language:{language} "

    return query
```

Improvising
> passing arguments, and using create_query function

> updating the main 

```py
def repos_with_most_stars(languages):
    gh_api_repo_search_url = "https://api.github.com/search/repositories"

    query = create_query(languages)
    sort = "stars"
    order = "desc"
    parameters = {"q": query, "sort": sort, "order": order}

    response = requests.get(gh_api_repo_search_url, params=parameters)
    response_json = response.json()

    return response_json["items"]

if __name__ == "__main__":
    languages = ["python", "javascript", "ruby"]
    results = repos_with_most_stars(languages)

    for result in results:
        language = result["language"]
        stars = result["stargazers_count"]
        name = result["name"]

        print(f"-> {name} is a {language} repo with {stars} stars.")    
```

### Cleaning Up and Handling Errors

Cleaning up
> adding default parameters
```py
def repos_with_most_stars(languages, sort="stars", order="desc"):
    gh_api_repo_search_url = "https://api.github.com/search/repositories"

    query = create_query(languages)
    parameters = {"q": query, "sort": sort, "order": order}

    response = requests.get(gh_api_repo_search_url, params=parameters)
    response_json = response.json()

    return response_json["items"]
```

Handling error

```py
def repos_with_most_stars(languages, sort="stars", order="desc"):
    gh_api_repo_search_url = "https://api.github.com/search/repositories"
    query = create_query(languages)

    # Define the parameters we want to be part of our URL
    parameters = {"q": query, "sort": sort, "order": order}

    # Pass in the query and the parameters as part of the request.
    response = requests.get("https://api.github.com/search/repositories", params=parameters)
    status_code = response.status_code

    # Check if the rate limit was hit.
    if status_code == 403:
        raise RuntimeError("Rate limit reached. Please wait a minute and try again.")
    if status_code != 200:
        raise RuntimeError(f"An error occurred. HTTP Status Code was: {status_code}.")
    else:
        response_json = response.json()
        records = response_json["items"]
        return records
```    

Final Code

```py
"""
A small Python program that uses the GitHub search API to list
the top projects by language, based on stars.

GitHub Search API documentation: https://developer.github.com/v3/search/

Additional parameters for searching repos can be found here:
https://help.github.com/en/articles/searching-for-repositories#search-by-number-of-stars

Note: The API will return results found before a timeout occurs,
so results may not be the same across requests, even with the same query.

Requests to this endpoint are rate limited to 10 requests per
minute per IP address.
"""

import requests


def create_query(languages, min_stars=50000):
    """
    Create the query string for the GitHub search API,
    based on the minimum amount of stars for a project, and
    the provided programming languages.

    An example search query looks like:
    stars:>50000 language:python language:javascript
    """
    query = f"stars:>{min_stars} "

    for language in languages:
        query += f"language:{language} "

    return query


def repos_with_most_stars(languages, sort="stars", order="desc"):
    gh_api_repo_search_url = "https://api.github.com/search/repositories"
    query = create_query(languages)

    # Define the parameters we want to be part of our URL
    parameters = {"q": query, "sort": sort, "order": order}

    # Pass in the query and the parameters as part of the request.
    response = requests.get(gh_api_repo_search_url, params=parameters)
    status_code = response.status_code

    # Check if the rate limit was hit. 
    if status_code == 403:
        raise RuntimeError("Rate limit reached. Please wait a minute and try again.")
    if status_code != 200:
        raise RuntimeError(f"An error occurred. HTTP Status Code was: {status_code}.")
    else:
        response_json = response.json()
        records = response_json["items"]
        return records


if __name__ == "__main__":
    languages = ["python", "javascript", "ruby"]
    results = repos_with_most_stars(languages)

    for result in results:
        language = result["language"]
        stars = result["stargazers_count"]
        name = result["name"]

        print(f"-> {name} is a {language} repo with {stars} stars.")
```        