# Alexa Skill using AWS Lambda in Typescript

This is a simple starter project for Alexa skills using Typescript. 

## Pre-requisites

- Node.js
- Register for an [AWS Account](https://aws.amazon.com/)
- Register for an [Amazon Developer Account](https://developer.amazon.com/)
- Install and Setup [ASK CLI](https://developer.amazon.com/docs/smapi/quick-start-alexa-skills-kit-command-line-interface.html)

## Installation

- Install the dependencies

```bash
$ npm install
```

## Deployment

**ASK CLI** will create the skill and the Lambda function for you. The Lambda function will be created in `us-east-1 (Northern Virginia)` by default.

1. Navigate to the project's root directory. you should see a file named 'skill.json' there.

2. Deploy the skill and the Lambda function in one step by running the following command:

```bash
$ ask deploy
```

## Local development

During development, it's a pain to always have to deploy your Lambda function to see your changes.

There is a better way: you can connect Alexa to your local environment instead of Lambda, without having to make any modifications to your Lambda functions. This will save you the time of always making deployment, and your changes will be applied instantly.

1. First, we will need an HTTPS endpoint. You can use [ngrok](https://ngrok.com/) for this (it's free), or any other similar tools.

```bash
$ ngrok http 3980 
```

This will give you an HTTPS endpoint which will proxy all requests to your local server (something like `https://84f7599f.ngrok.io`).

2. Open `.ask/config` and replace the following line

```json
"uri": "https://YOUR_URL.ngrok.io",
```

with the HTTPS endpoint from `ngrok`, e.g.:

```json
"uri": "https://84f7599f.ngrok.io",
```

3. Now we need to update the skill endpoint to our HTTPS endpoint

```bash
$ ask deploy --target skill --profile local
```

You can also do this manually if you want:

- Go to [your skill's dashboard](https://developer.amazon.com/alexa/console)
- Select `Endpoint`
- Select `HTTPS`
- Enter your HTTPS url
- For the `SSL certificate type` make sure you select `My development endpoint is a sub-domain of a domain that has a wildcard certificate from a certificate authority`

4. Now we need to start the local HTTP server

```bash
$ npm start
```

This will start a server using `express` (check `lambda/local/index.ts`) and `nodemon`, which will restart the process when you make any code changes so that you don't have to.

## Developer tasks

| Command | Description |
| --- | --- |
| `clean` | Deletes the `dist` folder |
| `build` | Builds the lambda function and exports it to the `dist` folder |
| `deploy` | Builds the lambda function and deploys EVERYTHING (skill, model, lambda) |
| `deploy:lambda` | Builds the lambda function and deploys it (just the lambda function) |
| `deploy:local` | Deploys the skill details for the local profile, which will update the HTTPS endpoint |
| `start` | Starts the local `express` server using `nodemon` for local development |

To see the actual commands, check `package.json`.

Also check the [ASK CLI Command Reference](https://developer.amazon.com/docs/smapi/ask-cli-command-reference.html) for more details on using the `ASK CLI`.

## Testing

Taken from [the official hello world project](https://github.com/alexa/skill-sample-nodejs-hello-world/blob/master/instructions/7-cli.md#testing).

1. To test, you need to login to Alexa Developer Console, and **enable the "Test" switch on your skill from the "Test" Tab**.

2. Simulate verbal interaction with your skill through the command line (this might take a few moments) using the following example:

	```bash
	 $ ask simulate -l en-US -t "open greeter"

	 ✓ Simulation created for simulation id: 4a7a9ed8-94b2-40c0-b3bd-fb63d9887fa7
	◡ Waiting for simulation response{
	  "status": "SUCCESSFUL",
	  ...
	 ```

3. Once the "Test" switch is enabled, your skill can be tested on devices associated with the developer account as well. Speak to Alexa from any enabled device, from your browser at [echosim.io](https://echosim.io/welcome), or through your Amazon Mobile App and say :

	```text
	Alexa, start hello world
	```

## Customization

Taken from [the official hello world project](https://github.com/alexa/skill-sample-nodejs-hello-world/blob/master/instructions/7-cli.md#customization).

1. ```./skill.json```

   Change the skill name, example phrase, icons, testing instructions etc ...

   Remember than many information are locale-specific and must be changed for each locale (e.g. en-US, en-GB, de-DE, etc.)

   See the Skill [Manifest Documentation](https://developer.amazon.com/docs/smapi/skill-manifest.html) for more information.

2. ```./lambda/custom/index.ts```

   Modify messages, and data from the source code to customize the skill.

3. ```./models/*.json```

	Change the model definition to replace the invocation name and the sample phrase for each intent.  Repeat the operation for each locale you are planning to support.

4. Remember to re-deploy your skill and Lambda function for your changes to take effect.

	```bash
	$ ask deploy
	```

## License

Open sourced under the [MIT license](./LICENSE.md).