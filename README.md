Some Ruby scripts, used for introspecting which packages are installed in 
EC2 AMIs.

========================================================================

Filter script ist responsible to filter AMIs from AWS EC2 with following filter mechanisms 
- Region
- Owner ID
- Free
- Unknown

Introspection script takes the AMI list delivered by Filter, and introspect die AMIs by doing following tasks
- launch AMIs
- get infos about package manager, os and installed packages on the system --> [package_manager_info file]
- try to install Ohai (now only support Ubuntu > 8.04)
- if Ohai is installed successfully --> [ohai_info file]
- download the two files above
- save in corresponding region and owner

========================================================================

PREREQUISITES
- Ruby Interpreter
- EC2 API Tools
- Gem logger
- Gem aws-sdk
- Gem yaml
- Gem json
- Gem net-ssh

DISTRO SUPPORT
- Package Manager Info are gathered only by DPKG and RPM.
- Ohai Info only by Ubuntu > 8.04

INPUT 
[input/configuration.yml]
- Create a input/configuration.yml by using the configuration.yml.tmpl template
- AWS Credentials (for using AWS SDK Ruby to perform requests to EC2)
- X509 Certificates (for using EC2 API Tools to perform request to EC2). Set up in .bashrc. See https://help.ubuntu.com/community/EC2StartersGuide
- Region (e.g. us-east-1, us-west-1, etc..)
- Owner ID
- Login user (prioritised!!, first user will be checked first to establisch a ssh connection)
- Key pair
- Security Group

USE FILTER
- ruby Run_Filter.rb 
- -> [output/intermediate/region_owner_free_unknown_amis.txt]: the list, which contains AMIs after filter

USE INTROSPECTION
- ruby Run_Introspection.rb 
- -> [output/regions/....]: the JSON files are located in the corresponding folder

