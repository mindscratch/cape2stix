# CAPE2STIX
The purpose of this project is to automatically analyze malware in CAPE and then convert CAPE data into STIX 2.1 data. <br>CAPE (Config And Payload Extraction) is a malware sandbox. It was derived from Cuckoo with the goal of adding automated malware unpacking and config extraction. Automated unpacking allows classification based on Yara signatures to complement network (Suricata) and behavior (API) signatures.
There is a free community instance online which anyone can use: [CAPE Sandbox](https://capesandbox.com)

This project contains git submodules. Use the following commands to ensure all submodules are available

    git clone --recursive
    cd cape2stix/
    git submodule update --init --recursive


The pipeline is as follows:
- Automatically create a CAPE sandbox system, complete with N number of virtual machines to analyze malware samples
- Run malware samples in CAPE
- Export CAPE reports
- Convert CAPE reports to STIX



## Automated Malware Analysis Deployment
[ama_deploy/](ama_deploy/) is the directory containing all setup for building CAPE from scratch using virtual machine images. In addition to deploying CAPE, it also contains the functionality to setup the cape2stix and neo4j docker containers. See [ama_deploy documentation](ama_deploy/README.md#command-description) for further instructions <br>**The assumed target for deployment is ubuntu 22LTS or similar.**


## cape2stix
cape2stix can convert CAPE reports to STIX 2.1 and submit them to a neo4j database. Installation can be done either manually or automatically. <br>**The assumed target for deployment is ubuntu 22LTS or similar.**

### Manual Installation
Manual setup is preferred for smale-scale conversion without database submission. Users will need to install and run poetry
```bash
python3 -m pip install poetry # install poetry
poetry install                # install dependencies under poetry environment
poetry shell                  # enter the poetry environment
```
A virtual environment will be created that can now be accessed by running `poetry shell`. This should be used to run all Python scripts

### Automated Installation
[run.py](ama_deploy/run.py) can build and deploy the docker containers necessary for cape2stix and the neo4j database. For more information on how to use this tool, see the [ama_deploy documentation](ama_deploy/README.md#command-description)

### Usage
[convert.py](cape2stix/scripts/convert.py) is a script that will convert a JSON report generated by CAPEv2 to a STIX bundle
 The usage of the script can be seen below.

    usage: convert.py [-h] [--log_level {warn,debug,info}] [--disallow_custom] [--small] (-u | -f FILE)

    CAPE json conversion to STIX. Using mainly the report.json output

    optional arguments:
    -h, --help            show this help message and exit
    --log_level {warn,debug,info}
    --disallow_custom
    --small
    -u                    run tests with base file
    -f FILE, --file FILE  path to file ie: ./report.json

Example usage:

    python3 cape2stix/scripts/convert.py -f input/test.json --log_level info

The resulting STIX JSON file will be placed in [output](output/).<br>Note: this needs to run from the repository's top level directory

## Notes

- [Neo4j_README.md](cape2stix/todb/neo4j_readme/Neo4j_README.md) contains further documentation on using Neo4j
- when running `poetry install`, poetry may generate errors. Ignore these unless they cause the command to fail altogether