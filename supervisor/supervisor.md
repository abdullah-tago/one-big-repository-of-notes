# Supervisor Documentation

## Purpose

Supervisor is a process control system that allows you to monitor and control a number of processes on UNIX-like operating systems. It is commonly used to keep applications running, automatically restart them on failure, and manage logs.

## Advantages of Using Supervisor

- **Automatic Process Restart:** Automatically restarts crashed or stopped processes to ensure high availability.
- **Centralized Process Management:** Manage multiple applications and services from a single interface.
- **Easy Configuration:** Simple, human-readable configuration files for defining and controlling processes.
- **Logging:** Built-in support for capturing and rotating stdout and stderr logs.
- **Process Grouping:** Organize related processes into groups for easier management.
- **Remote Control:** Control processes using the `supervisorctl` command-line tool or via XML-RPC API.
- **User Privilege Management:** Run processes as different users for improved security.
- **Autostart on Boot:** Automatically start configured processes when Supervisor starts.
- **Graceful Shutdown:** Handles process shutdowns cleanly and can send custom signals.

## Common Arguments (in `supervisord.conf` or program config)

- **command**: The command to run your program (e.g., the path to your Python script).
- **directory**: The working directory for the command.
- **autostart**: If `true`, the program starts automatically when Supervisor starts.
- **autorestart**: If `true`, Supervisor restarts the program if it exits unexpectedly.
- **user**: The user to run the program as.
- **stdout_logfile**: File path for standard output logs.
- **stderr_logfile**: File path for error logs.
- **environment**: Set environment variables for the program.

## Common Supervisor Commands

- `supervisord`  
  Starts the Supervisor daemon.

- `supervisorctl status`  
  Shows the status of all managed processes.

- `supervisorctl start <program>`  
  Starts a specific program.

- `supervisorctl stop <program>`  
  Stops a specific program.

- `supervisorctl restart <program>`  
  Restarts a specific program.

- `supervisorctl reread`  
  Reloads the configuration files.

- `supervisorctl update`  
  Applies changes after rereading configs.

## Example Program Configuration

```ini
[program:demo-app]
directory=/mnt/c/Users/Admin/srv/sample-application
command=/mnt/c/Users/Admin/srv/sample-application/.venv/bin/python /mnt/c/Users/Admin/srv/sample-application/app.py
autostart=true
autorestart=true
stderr_logfile=/var/log/demo-app.err.log
stdout_logfile=/var/log/demo-app.out.log
user=amt
environment=PYTHONUNBUFFERED=1
```

## Example Usage

1. **Start Supervisor:**
   ```
   supervisord -c /etc/supervisor/supervisord.conf
   ```

2. **Check status:**
   ```
   supervisorctl status
   ```

3. **Restart your app:**
   ```
   supervisorctl restart demo-app
   ```

---

For more details, see the [Supervisor documentation](http://supervisord.org/)
