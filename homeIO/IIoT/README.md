Instructions to run the project for the first time (Windows)

Requirements
- Windows 10 or later
- The computer language must be set to English

## 1. Install HomeIO
Launch the installer `homeio-org-installer.exe` and follow the instructions.
Bu sure to install all the optional components. 

## 2. Open HomeIO
Launch the application `HomeIO.exe`, start a new simulation and save a game. It will create a folder `Home IO` in your Documents folder:
- `C:\Users\<YourUserName>\Documents\Home IO`

Put the provided config file `OT_2024_6_24_10_44_30.xml` in `$HOME/Documents/Home IO/Saves`

Run Home IO and load the config file.

## 3. Launch Gateway
On the same computer than HomeIO (while HomeIO is running), launch the Modbus Gateway (Windows x64):
```bash
.\ModbusGateway-Win-x64.exe
```

## 4. Launch Controller

For Wago (`192.168.1.2`) on the IIoT Rack via SSH
- User: `root`
- Password: `wago`

```bash
.\Controller_sol-Wago
```

Or locally to your computer
```bash
.\Controller_sol-Win-x64 -ip "127.0.0.1"
```

## 5.a Check the REST API
Go to the website: [REST API](https://homeio.sdi.hevs.ch/homeio/swagger/index.html)


## 5.b Launch REST API (only for the teacher)
Go to the folder where the REST API is located: `<...>\rest-api-container`
```bash
docker-compose up
```

Once the REST API is running, you can access the Swagger UI at:

```bash
http://localhost:8080/swagger/index.html
```

To test the API, you can use the usual credentials:
username: `sdi##` (the username is sdi followed by the student number)
password: the password is the MD5 hash of the SDI username (the username is sdi followed by the id)

For the prof the id number is 42. For the hash, check [here](https://sdi.hevs.ch/)


# 7. Test the API on python


```python
import requests

# URL
url = # Replace with the actual URL of your REST API (on the local machine: 'http://localhost:8080/homeio')

# Parameters from the URL query string
params = {'room': 'Garage', 'device': 'Door'}

# Headers from the curl request
headers = {
    'accept': 'application/json',
    'Content-Type': 'application/json'
}

# Data (request body) from the curl request
data = {
    "command": "UP"
}

# Replace with your actual username and password
username = # replace with your username
password = # replace with your password

try:
    response = requests.post(
        url,
        params=params,
        headers=headers,
        json=data,  # Use 'json' parameter to automatically encode the data as JSON
        auth=(username, password)  # For basic authentication
    )

    # Print the response status code
    print(f"Status Code: {response.status_code}")

    # Print the response content (if any)
    try:
        print("Response JSON:", response.json())
    except requests.exceptions.JSONDecodeError:
        print("Response Content:", response.text)

    # You can add more error handling or processing of the response here

except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```


