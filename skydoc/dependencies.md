# Installing Dependencies #

Open-STF has 12 dependencies installed via docker images, which download, maintain a database of devices and are used to configure the application. These are:

  * `stf-app@3100.service`
  * `stf-auth@3200.service`
  * `stf-migrate.service`
  * `stf-processor@1.service`
  * `stf-reaper.service`
  * `stf-storage-plugin-apk@3300.service`
  * `stf-storage-plugin-image@3400.service`
  * `stf-storage-temp@3500.service`
  * `stf-triproxy-app.service`
  * `stf-triproxy-dev.service`
  * `rethinkdb.service`
  * `rethinkdb-proxy-28015.service`
  
  Note: if using vim/vi to create each file, press ‘i’ before pasting the code. If this is not performed only trailing characters from the first instance of character ‘i’ will be inserted.
  
### RethinkDB deployment on LSD ###

The following is a rough guide showing how to deploy RethinkDB on LSD, using docker and systemd. This is by no means a final version, as no backup or proper data storage setup, but it is sufficient for development and testing.

Spin up an instance, using an operating system with systemd. In this document CoreOS is used as it is lightweight and has Docker pre-installed.
Install docker.
Create the file rethinkdb.service. Replace the authkey with whatever you want it to be. Change the version from 2.1.1 to whatever is newest. The reason we keep it static instead of using “latest” is because a new version may require the database to be rebuilt. This will allow a controlled installation and not happen without being aware.

` [Unit]
Description=RethinkDB
After=docker.service
Requires=docker.service `  
`[Service] `  
`EnvironmentFile=/etc/environment`  
`TimeoutStartSec=0`  
`Restart=always `  
`ExecStartPre=/usr/bin/docker pull rethinkdb:2.1.1`  
`ExecStartPre=-/usr/bin/docker kill %p`  
`ExecStartPre=-/usr/bin/docker rm %p `  
`ExecStartPre=/usr/bin/mkdir -p /srv/rethinkdb  `
`ExecStartPre=/usr/bin/chattr -R +C /srv/rethinkdb`   
`ExecStart=/usr/bin/docker run --rm \ `  
 ` --name %p \`  
  `-v /srv/rethinkdb:/data \`  
  `-e "AUTHKEY=YOUR_RETHINKDB_AUTH_KEY_HERE" \ `  
  `--net host \`  
  `rethinkdb:2.1.1 \`  
  `rethinkdb --bind all \`  
   ` --cache-size 8192`  
`ExecStop=-/usr/bin/docker stop -t 10 %p `  
`[Install]`  
`WantedBy=multi-user.target `  

Enable the service file
sudo systemctl enable /full/path/to/rethinkdb.service
e.g	sudo systemctl enable /home/core/rethinkdb.service

Start the service
sudo systemctl start rethinkdb.service

Check that the database is running by connecting to the ip of the database server, port 8080. You should see the dashboard. Click the “Data Explorer” tab:


In the Data Explorer tab, use the following command and set the auth key to whatever you chose it to be earlier:

r.db('rethinkdb').table('cluster_config').get('auth').update({auth_key: 'YOUR_RETHINKDB_AUTH_KEY_HERE'})

RethinkDB should now be running. Access it via your instances floating IP using port 8080.


