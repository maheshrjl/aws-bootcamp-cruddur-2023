# Week 0 — Billing and Architecture

## Installing & Configure AWS CLI

### AWS CLI Setup

#### Local Setup

- Install AWS CLI in local machine (WSL) & enable autocomplete

- For Autocomplete: Add line to ~/.bashrc
```
complete -C '/usr/local/bin/aws_completer' aws
```

#### Remote Setup (Github Codespaces)

- Install the AWS CLI in github codespaces

Create a `.devcontainer/devcontainer.json` in the repo with below contents

```
{
	"image": "mcr.microsoft.com/devcontainers/universal:2",
	"features": {
		"ghcr.io/devcontainers/features/aws-cli:1": {}
	}
}
```

- Create a Access key for github codespaces & configure aws credentials with `aws configure`

- Enable autocomplete for aws commands

```
echo "complete -C '/usr/local/bin/aws_completer' aws" >> ~/.bashrc
```

- Reload bashrc configuration for autocomplete to work with `source ~/.bashrc`

- Verify aws access by running`aws sts get-caller-identity`

## Billing and Alerts

### Set GitHub Codespace variables for AWS Account ID

1. Navigate to `Settings -> Codespaces -> Codespaces secrets` in GitHub

2. Create a secret `AWS_ACCOUNT_ID` for the cruddur repository

### Enable Billing

- From the root account in the [billing page](https://us-east-1.console.aws.amazon.com/billing/home?region=us-east-1#/preferences)

- Enable `Receive Billing alerts` & `receive free usage alerts`

### Billing Alarm

#### Create SNS topic

- To enable billing alarm we need a sns (Simple Notification Servicee) topic.

```
aws sns create-topic --name billing-alarm-topic
```

#### Subscribe to the SNS topic

- Subscribe to the SNS topic with our Email & the topic ARN (Amazon Resource Name) to receive Email alerts

```
aws sns subscribe --topic-arn topicARN --protocol email --notification-endpoint mail@example.com
```

- Confirm the Email before we start receiving alerts

#### Create a cloudwatch alarm

```
aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm_config.json
```

#### Create a budget

```
aws budgets create-budget --account-id $AWS_ACCOUNT_ID --budget file://aws/json/budget.json --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json
```

## Architecture

[Conceptual Diagram](https://lucid.app/lucidchart/45111b94-da21-4009-b2b5-f374b60ae602/edit?invitationId=inv_3f63a28b-ab19-4507-8bfb-52fedf50a663&page=0_0#)

![image](https://user-images.githubusercontent.com/62394512/218376017-7c8330a1-ce92-4fe5-a2ba-0535cc9b1dcd.png)


[Architecture Diagram](https://lucid.app/documents/view/460b3c8d-a049-40c0-ac05-aee04d264962)

![image](https://user-images.githubusercontent.com/62394512/218374776-855aea03-9fff-46d7-a02e-8c5472ba28c6.png)



