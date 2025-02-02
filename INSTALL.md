
### Prerequisites
1. **Rust Toolchain**: Conduit is written in Rust, so you'll need the Rust toolchain installed.
2. **PostgreSQL**: Conduit uses PostgreSQL as its database backend.
3. **System Dependencies**: Ensure you have necessary system dependencies like `git`, `make`, and `build-essential` (or equivalent for your OS).

### Step 1: Install Rust
If you don't have Rust installed, you can install it using `rustup`:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

After installation, ensure the Rust toolchain is up to date:

```bash
rustup update
```

### Step 2: Install PostgreSQL
Install PostgreSQL on your system. For example, on Ubuntu:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

Create a new PostgreSQL user and database for Conduit:

```bash
sudo -u postgres createuser --pwprompt conduit
sudo -u postgres createdb --owner=conduit conduit
```

### Step 3: Clone the Conduit Repository
Clone the Conduit repository from GitHub:

```bash
git clone https://github.com/girlbossceo/conduit.git
cd conduit
```

### Step 4: Configure Conduit
Copy the example configuration file and modify it according to your setup:

```bash
cp conduit-example.toml conduit.toml
```

Edit `conduit.toml` to set the database connection details and other settings. For example:

```toml
[global]
server_name = "your.domain.com"
database_backend = "postgres"
database_url = "postgres://conduit:yourpassword@localhost/conduit"
```

### Step 5: Build and Run Conduit
Build Conduit using Cargo:

```bash
cargo build --release
```

Once built, you can run Conduit:

```bash
./target/release/conduit
```

### Step 6: Set Up a Reverse Proxy (Optional)
For production use, it's recommended to set up a reverse proxy using Nginx or Apache. Here's an example Nginx configuration:

```nginx
server {
    listen 80;
    server_name your.domain.com;

    location / {
        proxy_pass http://127.0.0.1:6167;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Step 7: Run Conduit as a Service (Optional)
To run Conduit as a service, you can create a systemd service file:

```bash
sudo nano /etc/systemd/system/conduit.service
```

Add the following content:

```ini
[Unit]
Description=Conduit Matrix Homeserver
After=network.target

[Service]
User=yourusername
WorkingDirectory=/path/to/conduit
ExecStart=/path/to/conduit/target/release/conduit
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start the service:

```bash
sudo systemctl enable conduit
sudo systemctl start conduit
```

### Step 8: Verify Installation
You can verify that Conduit is running by visiting `http://your.domain.com` in your browser or using a Matrix client to connect to your server.

### Troubleshooting
- **Logs**: Check the logs for any errors. If running as a service, use `journalctl -u conduit` to view logs.
- **Database Issues**: Ensure the PostgreSQL user and database are correctly set up and accessible.
- **Firewall**: Ensure ports are open if you're running Conduit on a remote server.

That's it! You should now have a running instance of Conduit.
