# Metabase Setup


Prerequisites:
- Install Dokku on your server
- It's helpful to setup an alias to make running commands on dokku from your own computer

`$ alias dokku='ssh -t dokku@your-ip'`


## Download Metabase jar

Get the most recent JAR file from: http://www.metabase.com/start/jar.html

	$ git add metabase.jar
	$ git commit -am 'Add v... of metabase'

## Create application on dokku
```
dokku apps:create metabase

# Get the DATABASE URL
dokku config:get metabase DATABASE_URL

# and set it here
dokku config:set metabase MB_DB_CONNECTION_URI=<url from earlier> MB_DB_TYPE=postgres BUILDPACK_URL=https://github.com/heroku/heroku-buildpack-jvm-common.git

# Link any interesting database from other dokku containers
dokku mysql:link your-other-dokku-app metabase
# Note the urls and add them to the metabase dashboard.
```

# Deploy
```
$ git remote add dokku dokku@your-dokku-host.com:metabase
$ git commit -am 'deploy'
$ git push dokku master
```

# Updating Metabase

- Download the new jar from Metabase and replace the old jar.

```
$ git add metabase.jar
$ git commit -m 'Upgrade to v...'
$ git push dokku master
```

You may have to wait a few moments for migrations to run. To check on the progress run the following commnad

	$ dokku logs metabase

# Source
https://medium.com/@sndyrgrs/how-to-run-metabase-on-dokku-dffdfe1ef442#.hby06m5lm
