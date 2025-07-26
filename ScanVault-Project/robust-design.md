### How to Make a Robust System to Handle Bulk Requests (SQS + SNS + DLQ)

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/t8c0q9wbo06bpyv02miq.png)

Let’s say you need to send order placed and payment confirmation receipts to millions of users. Challenging, right? You can't just run a loop on the server millions of times — that’s not scalable.

So, we think smart. Let’s create a **bulk email worker/server** that receives the order and payment confirmation messages, calls the **Gmail API**, and then sends notifications to users.

Cool? Perfect! That’s how it works…  

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ww3o1sflew6immj7qf6s.png)

**But here's the challenge**:  
This is a **synchronous process**, meaning it depends on the Gmail API. If the Gmail API is down or slow, the worker will **wait for the acknowledgement** and won’t be able to process other payment events. That’s not scalable.


### Enter AWS SQS — A Robust Solution

To solve this, we can use **Amazon SQS (Simple Queue Service)**, which is **very robust** and decouples the system.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mvp4kwuoamsvlsh2m7uf.png)

#### Here’s how it works:

- We create a **queue of orders and payment confirmations**.
- Our **email worker servers** will continuously **poll the queue** and ask:  
  _“Hey, do you have something to send?”_
- If there are **millions of messages**, we can **scale the email workers horizontally**, i.e., spin up more workers to process them in parallel.


### But Wait… Another Challenge

Even if you scale your email servers, you’re still **dependent on Gmail**, which may have **rate limits** — for example, only **5 emails per second** per user/account.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ze01m3dsi8vuiae28v42.png)


#### To handle this, you can:

- Implement **rate limiting logic** in your workers.
- Use **exponential backoff** and **retry mechanisms**.
- Queue unsent emails in a **Dead-Letter Queue (DLQ)** for retry later.


### Multi-Channel Notification? Use AWS SNS

In today’s world, you don’t just send emails. You also send **WhatsApp messages, SMS, and push notifications**. So how do you scale that?

Simple: Use **AWS SNS (Simple Notification Service)** — a **broadcasting service**.

#### Here’s how:

- You **announce an event** (like `"payment_successful"`) using SNS.
- Multiple **subscribers** (email, SMS, WhatsApp, etc.) are attached to this SNS topic.
- SNS **fan-outs** the message to different channels, each connected to its own queue or service.


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fk03zl2oskzx2f8h1tay.png)


#### So if you have:

- An **SQS queue** for email  
- A **webhook** for WhatsApp  
- An **SMS gateway** for text messages  

Each of them gets the notification **independently**.

### What If Delivery Fails?

Good question.

SNS is **fire-and-forget** by default — no delivery confirmation.  
But you can attach **dead-letter queues (DLQs)** or monitor failures.

So if a mail fails to send, it goes to a **discarded queue**, and you can **retry later**.  
That’s why sometimes you see receipts **2–3 minutes after making a payment**.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q04rphf5a8nfdunp6hy6.png)


### Summary

- Don’t run loops for millions of tasks — **offload to queues**.
- Use **SQS** for message handling and decoupling.
- Use **SNS** for multi-channel announcements.
- Add **DLQs and retry logic** to handle external API failures.
- Design the system to be **asynchronous, scalable, and resilient**.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hipznqwdeo0n77ey9vj9.png)

---

✍️ **Author**: *Omkar Sharma*  
📬 *Feel free to connect on [LinkedIn](https://www.linkedin.com/in/omkarsharmaa/) or explore more on [GitHub](https://github.com/omkarsharma2821)* 