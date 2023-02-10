# AWS Lambda Telemetry Failure

This repo is meant to recreate an issue with the AWS Lambda Telemetry API that we are experiencing

## Usage

### Deployment

In order to deploy the example, you need to run the following command:

```
$ yarn deploy
```

### Invocation

After successful deployment, you can invoke the deployed function by using the following command:

```bash
yarn sls invoke --function testFunction
```

Which should print out the logs from the last invocation.

Any log line that begins with `⚡️ TELEMETRY: ` is data that this telemetry extension received.

Currently this is all the data the telemetry server will receive 👇

```
⚡️ TELEMETRY: [{"time":"2023-02-10T20:39:01.267Z","type":"platform.initStart","record":{"initializationType":"on-demand","phase":"init","runtimeVersion":"nodejs:18.v4","runtimeVersionArn":"arn:aws:lambda:us-east-1::runtime:f2af14107aa47577b6391cef2e58e6032a5520042f7d27a21ab33c9ee5a18adb"}},{"time":"2023-02-10T20:39:01.405Z","type":"platform.telemetrySubscription","record":{"name":"index.js","state":"Subscribed","types":["platform","function"]}}]
⚡️ TELEMETRY: [{"time":"2023-02-10T20:39:01.751Z","type":"platform.initStart","record":{"initializationType":"on-demand","phase":"invoke","runtimeVersion":"nodejs:18.v4","runtimeVersionArn":"arn:aws:lambda:us-east-1::runtime:f2af14107aa47577b6391cef2e58e6032a5520042f7d27a21ab33c9ee5a18adb"}}]
⚡️ TELEMETRY: [{"time":"2023-02-10T20:39:01.865Z","type":"platform.telemetrySubscription","record":{"name":"index.js","state":"Already subscribed","types":["platform","function"]}}]
⚡️ TELEMETRY: [{"time":"2023-02-10T20:39:02.172Z","type":"function","record":"2023-02-10T20:39:02.172Z\tundefined\tERROR\tUncaught Exception \t{\"errorType\":\"Runtime.ImportModuleError\",\"errorMessage\":\"Error: Cannot find module 'does-not-exist'\\nRequire stack:\\n- /var/task/src/index.js\\n- /var/runtime/index.mjs\",\"stack\":[\"Runtime.ImportModuleError: Error: Cannot find module 'does-not-exist'\",\"Require stack:\",\"- /var/task/src/index.js\",\"- /var/runtime/index.mjs\",\"    at _loadUserApp (file:///var/runtime/index.mjs:1000:17)\",\"    at async UserFunction.js.module.exports.load (file:///var/runtime/index.mjs:1035:21)\",\"    at async start (file:///var/runtime/index.mjs:1200:23)\",\"    at async file:///var/runtime/index.mjs:1206:1\"]}\n"}]
```

That last `function` log is also only sent intermittently which is also frustrating.
