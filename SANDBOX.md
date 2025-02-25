# AFDX Bug Bash: Sandbox Setup
## SETUP INSTRUCTIONS
### Environment Setup
#### 1-A. Create a sandbox org
```
TODO: Add CLI commnd for creating a sandbox
```
#### 1-B. Authenticate the CLI to the sandbox org
```
TODO: Add CLI commnd for creating a sandbox
```
#### 1-C. Configure the sandbox org as the project default
```
TODO: Add CLI commnd for creating a sandbox
```

#### 2-A. Load dev/test data.
```
sf data import tree --plan baseline/data/plan.json
```
#### 2-B. Load Knowledge articles.
```
sf data import tree --files baseline/data/Knowledge__kav.json
```
#### 2-C. Publish the Knowledge articles.
1. Navigate to the Knowledge app.
2. View draft articles, or create a new list view for All Articles
3. Select all articles, then click the "Publish" button.

#### 3-A. Test and Activate the `Guest_Experience_Agent`
1. Open the org to the Agents setup page.
2. Prompt the Guest Experience Agent with this question:
```
I'm sofiarodriguez@example.com and my member number is 10008155. What beach activities are happening today?
```
#### 3-B. Test and Activate the `Local_Info_Agent`
1. Open the org to the Agents setup page.
2. Prompt the Local Info Agent with this question:
```
Can give me a weather forecast for Coral Cloud resort today?
```



## Useful Commands
### Delete a Scratch Org
```
sf org delete scratch -o AFDX_SCRATCH_SDB39:??
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
