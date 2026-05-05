# Ansible Role: Book Review Backend Deployment

Hi !

This Ansible role deploys the **Book Review App Backend** (Node.js + Express + MySQL) on Ubuntu servers.

I didn’t build the original application itself, but as a DevOps Engineer I took the project, improved several parts of it, and created this role to make deployment fast, consistent, and much easier to manage. It saves me a lot of time whenever I need to set up the app on a new server.



## What This Role Does

- Installs and configures MySQL
- Creates the database and dedicated application user
- Sets up Node.js environment
- Copies the backend code and installs dependencies
- Configures environment variables securely
- Runs the app with PM2 for production (auto-restart enabled)
- Opens the necessary firewall port



## Dependencies

None. This role installs everything it needs by itself (Node.js, MySQL, PM2, etc.).


## How to Use

1. Add the role to your Ansible project.
2. Review and update the variables in `defaults/main.yml` (especially the passwords and JWT secret — don’t forget to change them!).
3. Run your playbook.

Once finished, the backend should be live on port 3001 and managed by PM2.
