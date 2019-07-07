# Create multiple CloudFormation Stacks using Onica Runway

## Steps 

### Quick setup
https://docs.onica.com/projects/runway/en/latest/quickstart.html

```bash
pipenv install runway
pipenv run runway gen-sample cfn
```
### Customize
- Modify stacks.yaml to change all the CUSTOMERNAME etc
- Rename the dev-us-east-1.env to proper name reflecting env and region
- Modify the env file above to proper settings
- copy stacks json files from the result of https://github.com/TanAlex/aws-stacker-troposphere
- in our case, I copied from AWS CloudFormation Template tab for the result stack json file. There are 3 stacks (vpc.json, webserver.json and bastion.json)

- Modify stacks.yaml to change  
`OfficeNetwork: 162.156.1.187/32`  
to your own PC's public IP so only you can access the bastion server
- Make sure you have a ssh-key-pair named 'default' otherwise modify stacks.yaml to rename `SshKeyName: default`.

```bash
# Run test and deploy
pipenv run runway test
pipenv run runway plan
pipenv run runway deploy
```

Go to us-west-1 N.California, you should see 3 instances, 2 for webserver and 1 for bastion

You can try to access the bastion by
```bash
ssh -i your_private_key centos@bastion_public_ip
```

to try the Nginx web page, get the ELB DNS name from the Webserver Stack Output or simply click the ELB in Console and copy its DNS Name

Use browser to check the name, my sample one is like:   
http://onica-dev-webserve-g4lutwjiq76f-202124472.us-west-1.elb.amazonaws.com/  

It should show the hostname like `hello ip-10-128-11-251.internal`

Finally, run this to destroy all the stacks and resources
```bash
pipenv run runway destroy
```