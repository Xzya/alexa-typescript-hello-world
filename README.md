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

## License

Open sourced under the [MIT license](./LICENSE.md).