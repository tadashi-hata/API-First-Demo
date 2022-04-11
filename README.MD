# API First Demo
## What is API First?
API First is a consumer-driven approach to design your API focused on developer experience. 

Simply put, it is the action of create the API documentation based on standard specifications (e.g: Open API Spec) before start coding.

Even Though it's a simple concept, its a powerful practice that can improve the led time of new features and products.

The key benefits are:
- Always up to date specification;
- Better developer experience;
- Enforcement of API Governance;
- Reduce duplicated functionalities;
- Cohesive documentation and standards;
- Leverage DevOps to speed-up development;
- Decreased development effort by generation code based on  documentation;
- Enables multiple teams to work in parallel;

----------------------------

## Dependencies:
- [Spectral](https://github.com/stoplightio/spectral)
- [Mock server](https://www.mock-server.com/#what-is-mockserver)
- [Redoc](https://github.com/Redocly/redoc)
- [Open API Generator](https://openapi-generator.tech/)

----------------------------

## Related repositories:
- [Developer Portal](https://github.com/tadashi-hata/API-First-Developer-Portal)


The diagram bellow show how actions are executed and how the automation process works.
![Actions workflow](./images/API%20First%20example-Demo%20workflow.png)

------------------------------

## Setting up and using mock server:
The Mock server is running on local host. If you want to setup your own mock-server follow these steps:

Start docker:

    $ sudo dockerd

Pull MockServer Image:

    $ docker pull mockserver/mockserver

Run MockServer Container:

    $ docker run -d --rm -p 1080:1080 mockserver/mockserver

Now your mock server is up in running on localhost:1080
To create mock based on api documentation just execute the code bellow:
    # For linux based runner
    $ curl -v -X PUT "http://localhost:1080/mockserver/openapi" -d '{
            "specUrlOrPayload": "https://raw.githubusercontent.com/tadashi-hata/API-First-Demo/main/customers.yaml"
        }'

    # On powershell for windows based runner
    $  $Body = "{'specUrlOrPayload': 'https://raw.githubusercontent.com/tadashi-hata/API-First-Demo/main/customers.yaml'}"
        Invoke-WebRequest -UseBasicParsing -URI http://localhost:1080/mockserver/openapi -Body $Body -ContentType "application/json" -Method 'PUT'


### Windows based mock-server
In my case, I'm running the mock server on WSL ubuntu on windows machine.
Since I have restrictions on firewall and network, I did a workaround to make github actions able to execute the request to create the mock services.

My workaround is setting up a windows based runner to execute the **Invoke-WebRequest** locally in order to create the mock.

Steps to set up local github runner
**Create a folder under the drive root**

    $ mkdir \actions-runner ; cd \actions-runner

**Download the latest runner package**

    $ Invoke-WebRequest -Uri https://github.com/actions/runner/releases/download/v2.289.2/actions-runner-win-x64-2.289.2.zip -OutFile actions-runner-win-x64-2.289.2.zip

**Extract the installer**

    $ Add-Type -AssemblyName System.IO.Compression.FileSystem ;
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$PWD\actions-runner-win-x64-2.289.2.zip", "$PWD")

**Configure**
Create the runner and start the configuration experience

    $ ./config.cmd --url https://github.com/tadashi-hata/API-First-Demo --token {token}

    # This runner will have the following labels: 'self-hosted', 'Windows', 'X64'

**Bootstrap the runner**
Create a file named start-runner.ps1 and write the following command into it:
    
    $ C:\actions-runner\run.cmd

Them execute:

    $ powershell -ExecutionPolicy Bypass -File start-runner.ps1

