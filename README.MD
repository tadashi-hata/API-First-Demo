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

**Notes:**
The Mock server is running on local host. If you want to setup your own mock-server follow these steps:

Start docker:

    $ sudo dockerd

Pull MockServer Image:

    $ docker pull mockserver/mockserver

Run MockServer Container:

    $ docker run -d --rm -p 1080:1080 mockserver/mockserver

------------------------------

## using mock server:
Creating mock based on api document:

    $ curl -v -X PUT "http://BRPC013768:1080/mockserver/openapi" -d '{
            "specUrlOrPayload": "https://raw.githubusercontent.com/tadashi-hata/API-First-Demo/main/customers.yaml"
        }'

    # On powershell
    $  $Body = "{'specUrlOrPayload': 'https://raw.githubusercontent.com/tadashi-hata/API-First-Demo/main/customers.yaml'}"
        Invoke-WebRequest -UseBasicParsing -URI http://BRPC013768:1080/mockserver/openapi -Body $Body -ContentType "application/json" -Method 'PUT'


## Set up local github runner
Ref:
https://github.com/actions/runner/releases

The following snipped needs to be run on powershell:

### Create a folder under the drive root

    $ mkdir \actions-runner ; cd \actions-runner

### Download the latest runner package

    $ Invoke-WebRequest -Uri https://github.com/actions/runner/releases/download/v2.289.2/actions-runner-win-x64-2.289.2.zip -OutFile actions-runner-win-x64-2.289.2.zip
### Extract the installer

    $ Add-Type -AssemblyName System.IO.Compression.FileSystem ;
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$PWD\actions-runner-win-x64-2.289.2.zip", "$PWD")

### Configure
#### Create the runner and start the configuration experience
    $ ./config.cmd --url https://github.com/tadashi-hata/API-First-Demo --token ABZ4KGCZWBMO3PHVLMRT2FTCKQWFM

    # Configurations
    # Enter the name of the runner group to add this runner to: [press Enter for Default]
    # Enter the name of runner: [press Enter for BRPC013768] tiha-runner

    # This runner will have the following labels: 'self-hosted', 'Windows', 'X64'

#### Run it!
$ ./run.cmd

For windows based os its required to set up additional steps:
Create a file named start-runner.ps1 and write the following command into it:
    
    $ C:\actions-runner\run.cmd

Them execute:

    $ powershell -ExecutionPolicy Bypass -File start-runner.ps1

