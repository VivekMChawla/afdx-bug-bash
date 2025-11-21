# AFDX+NGA Pro-Code Bug Bash

## Bug Bash Flow

### Environment Setup

#### 1. Authorize your project to the Bug Bash Test org.
Use the credentials provided to you by the Bug Bash host.
```
sf org login web -s -a AFDX:BugBash -r https://afdx-sdb39-01.test1.my.pc-rnd.salesforce.com
```
#### 2. Update the Salesforce CLI
Ensure you're running the latest version of the Salesforce CLI. Run the update command repeatedly until you see a message "Updating CLI... already on version <current version>"
```
sf update
```
#### 3. Install the latest version of the `agent` plugin.
Ensure you're using the pre-release version of the `agent` plugin.
```
sf plugins install agent@nga
```
#### 4. Install VS Code extensions
Follow the instructions in [AFDX: V3 Beta Bug Bash](https://docs.google.com/spreadsheets/d/18JM9WoYFad5x85g9-F-qYwgbUkYEe6DuXpqzxiAfFDY/edit?usp=sharing)

#### 5. Retrieve all `Local_Info_Agent_NGA` authoring bundles.
This gives you access to all versions of the NGA agent used during the bug bash.
```
sf project retrieve start -m "AiAuthoringBundle:Local_Info_Agent_NGA_*"
```

### Preview from Authoring Bundles

#### 1. Preview `Local_Info_Agent_NGA_3` in "simulation mode" using the CLI
Run the following command and select `Local_Info_Agent_NGA_3 (Agent Script)`, then ask about today's weather.  
```
sf agent preview 
```
#### 2. Preview `Local_Info_Agent_NGA_3` in "live action mode" using the CLI
Run the following command and select `Local_Info_Agent_NGA_3 (Agent Script)`, then ask about today's weather.  
```
sf agent preview --use-live-actions
```
#### 3. Preview `Local_Info_Agent_NGA` in "simulation mode" using the CLI
Open `Local_Info_Agent_NGA.agent` from the `aiAuthoringBundles` source directory inside `force-app/main/default`.

Right-click inside the Agent Script file and select **AFDX: Preview this Agent**.

Click the "Start Simulation" button.  If the button says "Start Live Test", use the button's drop-down to select the correct mode.

Ask about today's weather.  Click the **Stop Simulation** button when done.

#### 4. Preview `Local_Info_Agent_NGA` in "live action mode" using the CLI
Use the "Start Simulation" button's drop-down to select "Live Mode".

Click the "Start Live Test" button.

Ask about today's weather.  Click the **Stop Live Test** button when done.


### Modify the Agent and Publish the Authoring Bundle

#### 1. Create an invalid Agent Script
Find the `check_local_weather` topic inside `Local_Info_Agent_NGA.agent`.

Change the keyword `topic` to `topics`.

Right-click inside the Agent Script file and select **AFDX: Validate this Agent**. 

Return the keyword to `topic` and run the validation command again.

#### 2. Make the agent give you a whimsical weather report
Modify the agent script so the agent reports weather in the whimsical manner of your choice.  Look at the `.agent` file in the version 3 bundle to see how it was made to give weather like a pirate.

#### 3. Preview the agent to see your changes.
Run a live or simulated preview to view changes in the agent's behavior.

#### 4. Publish the agent.
Right-click inside the Agent Script file and select **AFDX: Publish this Agent**.

You can also try to publish using the following command:
```
sf agent publish authoring-bundle --api-name Local_Info_Agent_NGA
```

### Create new Agent Scripts

See René Winkelmeyer's [Agent Script Recipes](https://github.com/salesforce-developers-emu/agent-script-recipes) repo for examples of other Agent Scripts you can try out.

Run the following command to get a simple Authoring Bundle that you can overwrite with your custom script.
```
sf agent generate authoring-bundle --spec specs/Local_Info_Agent-agentSpec.yaml
``` 
