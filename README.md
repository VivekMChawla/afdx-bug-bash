# AFDX Pro-Code Bug Bash

TODO: Add content
## SETUP INSTRUCTIONS
### Environment Setup
#### 1. Authorize your CLI to the Bug Bash Production org.
Use the credentials provided to you by the Bug Bash host.
```
sf auth web login --alias AFDX:BugBashProd
```
#### 2. Set the Bug Bash Production org as the default Dev Hub for this project.
```
sf config set target-dev-hub AFDX:BugBashProd
```
#### 3. Create a scratch org.
```
sf org create scratch -d -a AFDX:BugBashScratch -f config/org-shape-scratch-def.json
```
#### 4. Deploy baseline metadata.
```
sf project deploy start --metadata-dir baseline/metadata-scratch
```
#### 5. Manually enable Agentforce
Open the org to the Agents setup page, then manually enable Agentforce.
```
sf org open --path lightning/setup/EinsteinCopilot/home
```
#### 6. Deploy source.


## Useful Commands
### Delete a Scratch Org
```
sf org delete scratch -o AFDX:BugBashScratch
```
### Deploy Project Metadata
```
sf project deploy start --source-dir force-app
```
### Retrieve Project Metadata
```
sf project retrieve start --source-dir force-app
```
### Deploy ONLY Agent Metadata (All Agents)
```
sf project deploy start --manifest manifests/AllAgents.package.xml
```
### Retrieve ONLY Agent Metadata (All Agents)
```
sf project retrieve start --manifest manifests/AllAgents.package.xml
```
