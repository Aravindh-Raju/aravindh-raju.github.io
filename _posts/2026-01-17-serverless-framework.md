---
title: "Why I Use Serverless Framework for AWS Infrastructure"
categories:
  - Blog
tags:
  - AWS
  - Serverless
  - CloudFormation
  - Terraform
  - Infrastructure as Code
---

Managing AWS infrastructure manually works… until it doesn’t.

At some point, clicking around the console, trying to remember what was created where, and hoping nothing breaks during updates becomes exhausting. That’s when I started leaning heavily on the **[Serverless Framework](https://www.serverless.com/)**, backed by **CloudFormation**, to manage my AWS serverless resources.


I now use this setup to provision and maintain my infrastructure, and it’s made things a lot calmer and far more predictable.

---

## Why I Needed an IaC Approach

Before moving fully to Infrastructure as Code, a few patterns kept repeating:
- Resources were created manually and forgotten later
- Reproducing environments was painful
- Infrastructure changes weren’t tracked properly
- Rollbacks were stressful and slow

Once infrastructure started living alongside application code, most of these problems disappeared.

---

## Where Serverless Framework Fits In

The Serverless Framework lets me define AWS serverless resources in a simple configuration file. Everything lives in one place.

Behind the scenes, Serverless Framework generates **CloudFormation templates** and lets AWS handle:
- resource creation
- updates
- dependency ordering
- rollbacks

I get a clean developer experience without losing AWS-native reliability.

---

## A Simple Example

```yaml
service: user-service

provider:
  name: aws
  runtime: nodejs18.x
  region: us-east-1
  environment:
    TABLE_NAME: users-table

functions:
  getUser:
    handler: handler.getUser
    events:
      - http:
          path: users/{id}
          method: get

resources:
  Resources:
    UsersTable:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: users-table
        BillingMode: PAY_PER_REQUEST
        AttributeDefinitions:
          - AttributeName: id
            AttributeType: S
        KeySchema:
          - AttributeName: id
            KeyType: HASH
```

With this setup, Lambda, API Gateway, DynamoDB, and IAM permissions are created together and versioned in Git.

---

## How This Compares to Terraform

Both tools are solid, but they shine in different areas.

### Serverless Framework
- Optimized for serverless workloads
- Less boilerplate for Lambda and API Gateway
- Faster to get started for app-focused teams
- CloudFormation manages state and rollbacks

### Terraform
- Cloud-agnostic
- Better for large multi-cloud setups
- More control over non-serverless resources
- Requires explicit state management

For AWS-first, serverless-heavy applications, Serverless Framework feels more natural and lightweight.

---

## Final Thoughts

Using Serverless Framework with CloudFormation has made infrastructure feel predictable and safe.

Everything is written down, versioned, and repeatable. I spend less time worrying about infrastructure and more time building features.

That’s exactly how infrastructure should feel.

---

## Over to You

How are you managing your AWS infrastructure today?  
If you’ve used Serverless Framework or Terraform in production, I’d love to hear what worked for you.
