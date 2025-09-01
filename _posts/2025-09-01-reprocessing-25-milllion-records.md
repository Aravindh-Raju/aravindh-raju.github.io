---
title: "Reprocessing 25M Records Without Losing My Mind"
categories:
  - Blog
tags:
  - DynamoDB
  - AWS
  - Data Engineering
  - EventBridge
---

Sometimes the “simple” tasks are the ones that stretch your creativity the most.  
I recently had to **reprocess historical data from 2023** — about **25 million records** — and push it all back into an S3 bucket. Sounds straightforward, right? Well, here’s how it actually played out.

---

## Step 1: From Excel to CSV (aka Herding 25M IDs)

The data came to me in multiple Excel sheets — not exactly the most processing-friendly format. I wrote a quick **JavaScript script** to merge all those IDs into a single CSV file.

While at it, I added a new column called **`processed`**, defaulting to `false`. This would act like a tracker, so I’d know which records were already handled and which ones were still pending.

---

## Step 2: Importing Into DynamoDB

With the CSV ready, I used DynamoDB’s **“Import from S3” feature** to bulk-load the data. This is a lifesaver when you’re dealing with millions of rows — no need to write custom loaders or deal with batching yourself.

The table’s primary key was already set as `id`, so that part was straightforward.  
But there was a challenge: how do I efficiently pull out only the **unprocessed rows** without scanning all 25M records every time?

---

## Step 3: The GSI Trick

Scanning through millions of items on the `processed` flag would have been painfully slow and costly. The fix?  
I created a **Global Secondary Index (GSI)** with the `processed` column as the partition key.

Now I could query only the rows where `processed = false` — fast, efficient, and cost-friendly.

---

## Step 4: EventBridge for Automation

Next came the processing loop. I set up an **EventBridge rule** on a **30-minute schedule**:

1. Query the GSI to fetch items where `processed = false`.  
2. Process them (max **5,000 items per run** so Lambda doesn’t hit its 15-minute timeout).  
3. Push results into the S3 bucket.  
4. Update those rows in DynamoDB to mark `processed = true`.  

If a run fails, the rows simply remain `false`. That’s fine — reprocessing isn’t a problem since **S3 will just overwrite the file**, not create duplicates.

---

## Final Thoughts

It wasn’t glamorous, but it worked:  
- Historical data reprocessed ✅  
- Ingested into S3 ✅  
- DynamoDB cleaned and tracked ✅  
- Fully automated ✅  
- Safe batching with 5k per Lambda execution ✅  

---

## Over to You

This setup worked for me, but I’m curious:  
Do you know a **better way to handle 25M+ record reprocessing** without the DynamoDB + GSI + Eventbridge + Lambda setup? 

If yes, I’d love to hear it. Share your ideas with me!