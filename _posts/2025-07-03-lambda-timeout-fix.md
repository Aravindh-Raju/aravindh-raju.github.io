---
title: "AWS Lambda Timeout Got You? Beat It with Recursion (and a Little JS Magic)"
categories:
  - Blog
tags:
  - AWS
  - Lambda
  - JavaScript
  - Serverless
  - Cloud
  - Tech
  - Software Development
---

We’ve all been there. You're building something cool with AWS Lambda — serverless, fast, simple. Everything's great... until you hit this dreaded limit:

> “Lambda function timeout is limited to 900 seconds.”

Yep. 15 minutes. That’s all you get.

Now, if you're just resizing images or sending emails, no worries. But if you’re processing a big list, running long API chains, or doing heavy I/O, you’re out of luck — unless you get creative.

So, here’s the trick: **Let Lambda call itself.**

---

## 💡 The Idea: Break It Down and Recurse

You can’t make Lambda run longer than 15 minutes. That’s non-negotiable.

But what you *can* do is break the task into smaller pieces. Let each function call process a chunk, and then — before it times out — have it invoke itself again to continue.

Each execution passes its progress (like `startIndex`) to the next one. And the loop continues until the job’s done.

It’s simple. It’s clean. And it works like magic.

---

## 🧪 A Real Example: Processing 1000 Items in Batches

Let’s say you have 1000 items to process, and you want to handle 100 at a time.

Here’s how you’d do it in a Node.js Lambda:

```js
const AWS = require('aws-sdk');
const lambda = new AWS.Lambda();

const TOTAL_ITEMS = 1000;
const BATCH_SIZE = 100;

exports.handler = async (event) => {
  const startIndex = event?.startIndex || 0;
  const endIndex = Math.min(startIndex + BATCH_SIZE, TOTAL_ITEMS);

  console.log(`Processing items ${startIndex} to ${endIndex - 1}`);

  for (let i = startIndex; i < endIndex; i++) {
    // Pretend we’re doing something useful here
    console.log(`Processed item ${i}`);
  }

  if (endIndex < TOTAL_ITEMS) {
    console.log(`Still more to go. Calling myself again from index ${endIndex}`);

    const params = {
      FunctionName: process.env.AWS_LAMBDA_FUNCTION_NAME,
      InvocationType: 'Event', // async call
      Payload: JSON.stringify({ startIndex: endIndex }),
    };

    await lambda.invoke(params).promise();
  } else {
    console.log("✅ All items processed. Done and dusted!");
  }

  return {
    statusCode: 200,
    body: `Processed items ${startIndex} to ${endIndex - 1}`,
  };
};
```

---

## 🧐 “Wait... doesn’t `await lambda.invoke()` wait for the next function to finish?”

This was my first question too. And it’s an important one.

**Short answer:** No, it doesn’t — *if* you use `'Event'` as the `InvocationType`.

Let’s break it down:

| Invocation Type     | What it does                            | Does `await` wait? |
|---------------------|------------------------------------------|--------------------|
| `'RequestResponse'` | Synchronous. Waits for the result.       | ✅ Yes              |
| `'Event'`           | Asynchronous. Fire-and-forget.           | ❌ No               |
| `'DryRun'`          | Just validates permissions.              | ✅ N/A              |

With `'Event'`, the Lambda just **queues the new invocation and returns immediately**. It doesn’t wait for the next one to finish — which is exactly what we want in a recursive setup.

You’re not waiting on yourself. You’re handing off the baton and exiting fast 🏃‍♂️💨

---

## 🛠 Pro Tips

- **Leave a buffer** — don’t cut it too close to the 900-second timeout.
- Use **CloudWatch Logs** to monitor and debug each batch/run.
- Add retry and failure handling using **DLQ (Dead Letter Queue)**, or level up with **Step Functions**.
- If you want to run tasks in parallel, consider **SQS fan-out** instead of recursion.

---

## ✅ TL;DR

- Lambda has a **15-minute hard timeout**.  
- **Break the task into smaller chunks**.  
- **Recursively call your own Lambda** with updated progress.  
- Use `InvocationType: 'Event'` so the current function **doesn’t wait**.  
- Keep your function lightweight, and let the chain continue.

Boom — infinite job execution, still fully serverless. 🚀

---

## Wanna Go Further?

You can take this same logic and plug it into **Step Functions** if you want visual workflows, retries, or branching. Or use the **Serverless Framework** to deploy everything without the boilerplate headache.

This pattern is super flexible — whether you're processing huge datasets, crawling pages, or running async jobs — it just works.

So go ahead: slice it, queue it, recurse it.
s
Happy hacking! 🔁💻🚀
