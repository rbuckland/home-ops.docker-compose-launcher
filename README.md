# docker-compose systemd launcher

Uses a directory name as convention.

* This is a simple "symlink" based docker-compose systemd launcher. 
* Each systemd "docker-compose" service has it's own directory in `/srv/docker/`
* New services are added by 
   - creating a new directory `/srv/docker/my-thing/`
   - adding a `docker-compose.yml` to that folder.
   - adding a symlink e.g.
     ```
     cd /etc/systemd/system/multi-user.target.wants && \
     ln -s /lib/systemd/system/dc@.service dc@my-thing.service 
     ```
   - update and start the service.
     ```
     systemctl daemon-reload
     systemctl enable dc@my-thing.service
     systemctl start dc@my-thing.service
     ```

* See the contents of [dc@.service](dc@.service) to see it's internals.
    

## Installing

(working example with minecraft)

(assumed `sudo su - `)
1. mkdir -p `/srv/docker/minecraft-with-mods`
2. add [mine-craft/docker-compose.yml](minecraft/docker-compose.yml) to `/srv/docker/minecraft-with-mods`
3. cp the file [dc@.service](dc@.service) to  `/lib/systemd/system/dc@.service`
4. Install the systemd target 
     ```
     cd /etc/systemd/system/multi-user.target.wants && \
     ln -s /lib/systemd/system/dc@.service dc@minecraft-with-mods.service 
     systemctl daemon-reload
     systemctl enable dc@minecraft-with-mods.service
     systemctl start dc@minecraft-with-mods.service
     ```

## Credits

[https://github.com/MiGoller/dc-systemd-template](https://github.com/MiGoller/dc-systemd-template)
