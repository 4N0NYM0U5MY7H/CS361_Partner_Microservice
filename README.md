<picture>
  <source
    srcset=".github/osu-ecampus-dark.png"
    media="(prefers-color-scheme: dark)"
  />
  <source
    srcset=".github/osu-ecampus-light.png"
    media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"
  />
  <img src=".github/osu-ecampus-light.png" alt="Oregon State University Ecampus Logo." />
</picture>

# CS361: Microservice for Partner — Currency Exchange Rate Microservice

## Table of Contents
- [CS361: Microservice for Partner — Currency Exchange Rate Microservice](#cs361-microservice-for-partner--currency-exchange-rate-microservice)
  - [Table of Contents](#table-of-contents)
  - [About](#about)
  - [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [References](#references)
  - [Installation](#installation)
    - [Build and prepare the Microservice](#build-and-prepare-the-microservice)
    - [Test the Microservice is working correctly](#test-the-microservice-is-working-correctly)
  - [Using the Microservice](#using-the-microservice)
    - [Sending a Request](#sending-a-request)
    - [Receiving a Response](#receiving-a-response)
    - [Visual Request/Response Demo](#visual-requestresponse-demo)
    - [Sample Client in Python](#sample-client-in-python)
  - [Sequence Diagram](#sequence-diagram)
  - [Project Integration](#project-integration)
    - [Troubleshooting](#troubleshooting)
  - [Built With](#built-with)
  - [License](#license)

## About
Microservice for my partner's Course Project for the Software Engineering 1 course (CS 361) at Oregon State University. This microservice uses the [Open Access EchangeRate-API endpoint](https://www.exchangerate-api.com/docs/free) to generate currency exchange rate information.

## Getting Started
These instructions will get you a copy of the microservice up and running on your local machine.

### Prerequisites
You need to have a machine with [Python 3.10+](https://www.python.org/downloads/release/python-3100/) installed.
```sh
$ python --version
Python 3.10
```
This microservice requires [requests 2.28+](https://pypi.org/project/requests/). This can be installed globally on your machine or alternatively using `pipenv`.

### References
* [Pipenv: A Guide to the New Python Packaging Tool](https://realpython.com/pipenv-guide/)

## Installation
All installation is handled using `pip` or `pipenv`. The following steps will install the microservice dependencies.

### Build and prepare the Microservice
```sh
# Terminal 1
$ git clone https://github.com/4N0NYM0U5MY7H/CS361_Partner_Microservice
$ cd CS361_Partner_Microservice

# Option 1: Install depencies globally
$ python -m pip install requests

# Option 2: Install dependencies using pipenv
$ pipenv install

# Run the microservice
$ cd exchange_rate
$ python main.py
```

### Test the Microservice is working correctly
```sh
# Terminal 2
$ cd CS361_Partner_Microservice
$ python example.py

# Expected output
The exchange rate from USD to EUR is 0.932527.
```
## Using the Microservice
### Sending a Request
Send a `request` to this microservice by updating `data/requests.txt` with a request string containing two ISO 4217 Three Letter Currency Codes — e.g. `USD` for US Dollars, `EUR` for Euro etc. — separated by a comma.
```python
# Example client-side request using Python
with open("data/reqeusts.txt", w) as request:
    request.write("usd,eur")
```
> **Note**: Here's the list of [supported currency codes](https://www.exchangerate-api.com/docs/supported-currencies).

### Receiving a Response
This microservice sends `response`s to `data/response.txt` with an exchange rate string — e.g. request: `USD,EUR`. response: `0.932527`.

### Visual Request/Response Demo
<img src=".github/request-response.gif" height="350px">

### Sample Client in Python
```python
# Simple example client to run with the Currency Exchange Microservice
import time
import re

# <path_to_requests_file> points to the request file in the data directory
# NOTE: This example assumes the client is at the project root
path_to_requests_file = "data/requests.txt"

# <path_to_response_file> points to the request file in the data directory
# NOTE: This example assumes the client is at the project root
path_to_response_file = "data/response.txt"

# example currencies to exchange
base_currency = "usd"
target_currency = "eur"

# Send the request to the <path_to_request_file>
with open(path_to_requests_file, "w") as out_file:
    out_file.write(f"{base_currency},{target_currency}")

# Poll the <path_to_response_file> until you receive a response
while True:
    time.sleep(1)
    # Receive the response from the <path_to_response_file>
    with open(path_to_response_file, "r") as in_file:
        exchange_rate = in_file.readline()

    # No response received
    if exchange_rate == "":
        continue

    # Valid reponse received
    if re.search("^(0|[1-9]\d*)?(\.\d+)?(?<=\d)$", exchange_rate):
        # acknowledge valid response in <path_to_response_file>
        with open(path_to_response_file, "a") as out_file:
            out_file.write("\nResponse Received")
        break

# View the results
print(f"The exchange rate from USD to EUR is {exchange_rate}.")
```
```sh
# Expected Output
The exchange rate from USD to EUR is 0.932527.
```
## Sequence Diagram
<img src=".github/sequence-diagram.png" width="700px">

## Project Integration
Move or copy the `data` and `exchange_rate` directories to the root of your project.
```sh
# Unix systems
$ cd CS361_Partner_Microservice
$ mv exchange_rate data <path-to-project-root>
```
```sh
# windows
> cd CS361_Partner_Microservice
> move exchange_rate <path-to-project-root>
> move data <path-to-project-root>
```
```sh
# Run the microservice
$ cd <path-to-project-root>/exchange_rate
$ python main.py
```
* You can now send `request`s to the microservice by sending a valid request string to `data/requests.txt`.
* You can now recieve `response`s from the microservice by reading the information in `data/response.txt`.

> **Note**: The microservice must be running **BEFORE** the client sends a request.

### Troubleshooting
* Check that you have [python 3.10+](https://www.python.org/downloads/release/python-3100/) installed on your local machine
  ```sh
  $ python --version
  Python 3.10.0
  ```
* Check that you have [requests 2.28+](https://pypi.org/project/requests/) installed on your local machine
  ```sh
  $ python -m pip install requests
  ```
* Check that the `exchange_rate` and `data` folders are in the root of your project
  ```
  project
  |   README.md
  |   project files
  |   ...
  |___data
  |   |   requests.txt
  |   |   response.txt
  |
  |___exchage_rate
  |   |   exchage_rate.py
  |   |   main.py
  |
  |___folder
      |   folder files
      |   ...
      |
      |___subfolder
          |   subfolder files
          |   ...   
  ```
  * **Run the microservice from the exchange_rate directory**.
    ```sh
    $ cd my/project/root
    $ python main.py
    ```
    * If you do NOT, then you will encounter issues related to the communication files.
        ```sh
        # DO NOT do this!
        $ cd <project-root>
        $ python exchange_rate/main.py # Will cause issues with "requests.txt" and "response.txt"
        ``` 
* Check that your program is sending requests to `data/requests.txt`
* Check that your program is receiving responses from `data/response.txt`
  
  ```Python
  project
  |   ...
  |___data
  |   |   ...
  |   |   requests.txt 
  |   |   response.txt
  
  # from project root
  path_to_requests_file = "data/requests.txt"
  path_to_response_file = "data/response.txt"
  ```
  ```Python
  project
  |   ...
  |___data
  |   |   ...
  |   |   requests.txt
  |   |   response.txt
  |
  |___folder
      |   ...  
  
  # from project folder
  path_to_requests_file = "../data/requests.txt"
  path_to_response_file = "../data/response.txt"
  ```
  ```Python
  project
  |   ...
  |___data
  |   |   ...
  |   |   requests.txt
  |   |   response.txt
  |
  |___folder
      |   ...
      |
      |___subfolder
          |   ...   

  # from project subfolder
  path_to_requests_file = "../../data/requests.txt"
  path_to_response_file = "../../data/response.txt"
  ```
  > **[Learn more about paths](https://www.redhat.com/sysadmin/linux-path-absolute-relative)**.

## Built With
* [Python 3.10.0](https://www.python.org/downloads/release/python-3100/) - Powerful, fast, and easy to learn open language that runs everywhere.
* [Requests 2.28.2](https://pypi.org/project/requests/) - Simple, yet elegant, HTTP library.
* [ExchangeRate-API](https://www.exchangerate-api.com/docs/free) - Accurate and reliable
exchange rate API.

## License
This project is licensed under the MIT License - see the [LICENSE](license) file for details.