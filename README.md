🧠 Debugging Journey: How I Fixed My Flask + MySQL Docker Deployment

Today I finally got my Flask + MySQL two-tier application fully working in Docker 🐳 — but it wasn’t easy.
I hit multiple real-world issues that forced me to dig deeper into how containers actually communicate.

Here’s what happened 👇

🔹 Problem 1 – MySQL connection errors:
At first, Flask kept throwing Can't connect to MySQL server on 'mysql' (115) and Unknown database 'default_db'.
Turns out the Flask app wasn’t reading my environment variables properly, so it kept defaulting to a “local” DB name.

Solution:
I passed correct variables (MYSQL_HOST, MYSQL_USER, MYSQL_PASSWORD, MYSQL_DB) via docker run -e, recreated the container, and made sure both backend and MySQL shared the same custom Docker network (appnet).

🔹 Problem 2 – Timing and startup dependency:
The Flask container would start before MySQL was ready, causing initial connection failures.

Solution:
I learned to either restart the Flask container after MySQL was up or add small initialization logic (wait-for-db pattern). That ensured MySQL finished booting before Flask tried to connect.

🔹 Problem 3 – Port and access confusion:
I was testing on port 5000, but it wasn’t accessible from the browser.

Solution:
Mapped the container’s port 5000 to host port 80 (-p 80:5000), so I could access it directly from the public IP of my EC2 instance — and it worked flawlessly!

✅ Result:
Finally saw my custom HTML page — “Flask App [2-Tier] – Junoon 🔥” — live in the browser, with messages being stored in MySQL through Dockerized communication.

Every error taught me something real:

How containers discover each other by name

How Docker networks isolate and connect tiers

How environment variables and startup order actually matter in multi-container systems

Next, I’ll be scaling this setup using Docker Compose and Kubernetes, with real health checks and persistent storage.

#DevOps #Docker #Flask #MySQL #Python #CloudEngineering #LearningByDoing #AliKhanProjects #Debugging #Containers #AWS

<img width="1010" height="1302" alt="Screenshot 2025-10-30 at 7 51 22 AM" src="https://github.com/user-attachments/assets/1ee017f1-8a37-475d-b9da-8897992d5f7b" />
