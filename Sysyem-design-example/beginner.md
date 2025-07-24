## Your University Website Will Never Go Down Again — Beginner Friendly System Design

Let’s create a very simple and absolute beginner-friendly system design for our university website.


### 🌤 Normal Days — No Result, No Announcement, No Admit Card

On usual days when no results are declared, no announcements are made, and no admit cards are released, the traffic on the website is very normal.

In an **ideal case**, let's start from a **very simple** design — no load balancer, no auto-scaling, none of those typical complex components.

![Simple Server](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qbzz1d1kqmy0n8naqstw.png)

### Components Used:

- **Server**: This can be any highly configured machine that is up and running 24/7. Technically, your personal laptop can also act as a server — but obviously, no one wants that.
- **DNS (Domain Name System)**: It’s a global directory that maps domain names to IP addresses. You pay for that public IP.


### ⚠️ What Happens During High Traffic?

At the time of **result declaration** or **admit card release**, traffic increases drastically.  
The same server configuration cannot handle the sudden surge in requests.

The result? The application **faces downtime** or **crashes**.

![Single Server Overload](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8f964maea32dw4sjhy09.png)


### ✅ Solution 1: Vertical Scaling

So, what can we do?

We can **increase the configuration** of the server from:
- 4 CPUs → 8 CPUs
- 64 GB RAM → 128 GB RAM

This is called **Vertical Scaling**, which means increasing the power of the **existing machine**.

![Vertical Scaling](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fz3xh5o7ntc149n5awcx.png)


### ❌ Challenges with Vertical Scaling:
- We **cannot increase the configuration of a running server**.
- First, we need to **stop the server**, upgrade it, and then start it again — which causes **downtime**.
- It’s not recommended for **high-availability** applications.


### ✅ Solution 2: Horizontal Scaling

So, what else can we do?

We can use **Horizontal Scaling**, where we **replicate the resources** — basically, we **add new servers** instead of upgrading the existing one.

![Horizontal Scaling](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2v5d0svic5l2htswyzuc.png)

### ❌ Challenge with Horizontal Scaling:
- A **new IP** will be assigned to each new server.
- But, all requests will still go to the **original server** unless we have a way to distribute traffic.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r08ln9gcc868picv79yy.png)

Any idea how to solve that?

Well, this is what I’ll discuss in the next design! 🚀

---

Stay tuned for the next part where we’ll talk about **load balancers**, **traffic distribution**, and more smart strategies to handle peak-time traffic without breaking the system!

✍️ **Author**: *Omkar Sharma*  
📬 *Feel free to connect on [LinkedIn](https://www.linkedin.com/in/omkarsharmaa/) or explore more on [GitHub](https://github.com/omkarsharma2821)* 