## Debugging in VS Code
- Use the auto attach feature: Toggle Auto Attach command from the Command Palette (Ctrl+Shift+P)
    - Restart your terminal, and next time you run a node script with the `--inspect` option you'll be able to debug. 
- In VS Code, there are two core debugging modes, Launch and Attach
    - Think of a launch configuration as a recipe for how to start your app in debug mode before VS Code attaches to it, while an attach configuration is a recipe for how to connect VS Code's debugger to an app or process that's already running.
	    - You can create the launch configuration by going into the dubugger sidebar tab and creating a `launch.json`. The file will appear in a `.vscode` folder. 
- Here is the `launch.json` generated for NodeJS debugging:
``` json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}\\app.js"
    }
  ]
}
```


## Debugging in Chrome DevTools
- Run your node app with the `--inspect` flag. This basically allows third-party debuggers to attach to it. 
- Go to `chrome://inspect` . Make sure the **Discover network targets** checkbox is checked. Click the **Configure…** button and add your IP address and port.
- Then click **Open dedicated DevTools for Node**. In the **sources** tab you add breakpoints // make edits (edits won't be saved). 
- Once this window is open and you've some breakpoints, rerun the app in terminal and the DevTools should work. 