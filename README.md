<div align="center">
  <h1>Wanderlust</h1>
  <h2>The Ultimate Travel Blog 🌍✈️ for You</h2>
</div>

<img src="https://github.com/krishnaacharyaa/wanderlust/assets/116620586/17ba9da6-225f-481d-87c0-5d5a010a9538" alt="Preview Image" />

<hr />

<h1>🐳 Wanderlust — Docker Compose Stack (MongoDB + Redis + Backend + Frontend + Nginx)</h1>

<p>This repository runs the <strong>Wanderlust</strong> app using Docker Compose — a complete environment including the database, cache, backend API, frontend app, and Nginx reverse proxy.</p>

<h2>🚀 Services Overview</h2>
<ul>
  <li><strong>MongoDB 7</strong> → Database</li>
  <li><strong>Redis 7</strong> → Cache / Queues</li>
  <li><strong>Backend</strong> → Node.js app (port <code>3000</code>)</li>
  <li><strong>Frontend</strong> → Vite development server (port <code>5173</code>)</li>
  <li><strong>Nginx</strong> → Reverse proxy (port <code>80</code>, optional <code>443</code> for future TLS)</li>
</ul>

<h3>🔁 Nginx Proxies</h3>
<ul>
  <li><code>/</code> → <strong>frontend:5173</strong></li>
  <li><code>/api</code> → <strong>backend:3000</strong> (prefix stripped — <code>/api/users</code> → backend <code>/users</code>)</li>
</ul>

<hr />

<h2>📁 Project Structure</h2>

<pre><code>wanderlust/
├── backend/
│   ├── Dockerfile
│   ├── package.json
│   ├── .env
│   └── src/
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   ├── .env
│   └── src/
├── docker-compose.yml    ← Main Docker configuration
├── nginx.conf            ← Nginx reverse proxy config
├── .gitignore
└── README.md
</code></pre>

<hr />

<h2>🧩 Prerequisites</h2>

<p>Before starting, make sure you have:</p>
<ul>
  <li><strong>Docker</strong> 24+</li>
  <li><strong>Docker Compose</strong> v2+</li>
</ul>

<p>Verify installation:</p>

<pre><code>docker --version
docker compose version
</code></pre>

<p>💡 <strong>Note:</strong> Use a Linux host or VM (e.g., AWS EC2) with <strong>ports 80 and 443</strong> open in your firewall or security group.</p>

<hr />

<h2>🗂️ Repository Layout</h2>

<pre><code>.
├─ backend/
│  ├─ Dockerfile
│  └─ ... your backend code
├─ frontend/
│  ├─ Dockerfile
│  └─ ... your frontend code (Vite)
├─ nginx.conf                # Mounted into the Nginx container
└─ docker-compose.yml
</code></pre>

<hr />

<h2>⚙️ Quickstart</h2>

<p>Spin up all services with Docker Compose:</p>

<pre><code>docker compose up -d --build
</code></pre>

<p>Check Nginx configuration:</p>

<pre><code>docker compose exec nginx nginx -t
</code></pre>

<p>View logs:</p>

<pre><code>docker compose logs -f nginx
</code></pre>

<p>Then visit your app at:</p>

<pre><code>http://&lt;EC2-IP&gt;/
</code></pre>

<hr />

<h2>🧰 Useful Commands</h2>

<pre><code># Rebuild and restart containers
docker compose up -d --build

# Stop all running containers
docker compose down

# View running containers
docker ps

# Follow logs for a specific service
docker compose logs -f &lt;service-name&gt;
</code></pre>

<hr />

<h2>🔧 Environment Variables</h2>

<p>Each service has its own <code>.env</code> file for configuration:</p>
<ul>
  <li><code>backend/.env</code> → DB connection strings, API keys, JWT secrets, etc.</li>
  <li><code>frontend/.env</code> → API URLs and environment variables</li>
</ul>

<p>Be sure to update them with your actual environment settings before deployment.</p>

<hr />

<h2>🧱 Service Details</h2>

<h3>Nginx</h3>
<ul>
  <li>Acts as a reverse proxy for backend and frontend</li>
  <li>Configuration file: <a href="./nginx.conf">nginx.conf</a></li>
  <li>Mounted in the container via <code>docker-compose.yml</code></li>
</ul>

<h3>Backend</h3>
<ul>
  <li>Node.js (Express) application</li>
  <li>Exposes port <code>3000</code></li>
  <li>Connects to MongoDB and Redis</li>
</ul>

<h3>Frontend</h3>
<ul>
  <li>Vite-based React app</li>
  <li>Development server runs on port <code>5173</code></li>
  <li>Served via Nginx on <code>/</code></li>
</ul>

<hr />

<h2>🧩 Troubleshooting</h2>

<table>
  <thead>
    <tr><th>Problem</th><th>Possible Solution</th></tr>
  </thead>
  <tbody>
    <tr><td>Nginx not starting</td><td>Run <code>docker compose exec nginx nginx -t</code> to validate configuration</td></tr>
    <tr><td>Frontend not loading</td><td>Ensure Vite server is up at port 5173</td></tr>
    <tr><td>502 Bad Gateway</td><td>Check if backend container is running and reachable</td></tr>
    <tr><td>MongoDB connection failed</td><td>Verify <code>.env</code> file has the correct MongoDB URI</td></tr>
    <tr><td>Redis errors</td><td>Confirm Redis container is running and accessible</td></tr>
  </tbody>
</table>

<hr />

<h2>👨‍💻 Contributors</h2>

<table>
  <thead>
    <tr><th>Name</th><th>Role</th></tr>
  </thead>
  <tbody>
    <tr><td>Emon Shil</td><td>Maintainer</td></tr>
    <tr><td>[Contributors]</td><td>Development / DevOps</td></tr>
  </tbody>
</table>

<hr />

<h2>📄 License</h2>

<p>This project is licensed under the <strong>MIT License</strong>.  
See the <a href="./LICENSE">LICENSE</a> file for details.</p>

<hr />

<blockquote>
  ✨ <em>“Wanderlust — Because every great journey deserves a great app.”</em>
</blockquote>

<img width="1902" height="779" alt="image" src="https://github.com/user-attachments/assets/70672fd8-f424-4ec7-969f-a4570754191d" />

