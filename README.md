# docker-update-hosts
bash script to add host entry in `/etc/hosts` when a docker container is run. 
Based on [this answer](https://stackoverflow.com/a/63656003) on [Stack Overflow](https://stackoverflow.com) 

Why this solution?

- Short script, easy to review (didn't want to give some un-reviewed application with lots of dependencies access to the Docker socket (which means root access))
- It uses docker events to run every time a container is started or stopped (other solutions posted here run every second in a loop, which is way less efficient)
- Updates `/etc/hosts`, no separate DNS server needed.
- Only dependencies are `bash`, `mktemp`, `grep`, `xargs`, `sed`, `jq` and `docker`, all of which I had already installed.

Just put the script somewhere, e.g. `/usr/local/bin/docker-update-hosts`

*Note: The script removes the `_1` suffix added by docker-compose from container names. If you don't want that just remove `|sub("_1$"; "")` from the script.*

You can use a systemd service to run this synchronously with Docker: `/etc/systemd/system/docker-update-hosts.service`

To activate, run:
```
systemctl daemon-reload
systemctl enable docker-update-hosts.service
systemctl start docker-update-hosts.service
```
All thanks to [Florian Kaiser](https://github.com/fnkr)
