# dokku-copy-files-to-image

Copies files from the host's `$DOKKU_ROOT/$APP/DOKKU_FILES` directory to the `/app` directory of a dokku image before the image is released.

Only affects deploys where buildpacks are in use. Files are not available during the build step.

## requirements

- dokku 0.4.0+
- docker 1.6.x

## installation

```shell
# on 0.4.x
dokku plugin:install https://github.com/dokku/dokku-copy-files-to-image.git  copy-files-to-image
```

## usage

To use, create a `DOKKU_FILES` directory in `$DOKKU_ROOT/$APP`. For instance, if we have an application called `lolipop`:

```shell
# assuming $DOKKU_ROOT is /home/dokku
mkdir -p /home/dokku/lolipop/DOKKU_FILES
```

Next, add your files to that directory. You will also need to ensure the `dokku` user has ownership and read access to these files:

```shell
# assuming $DOKKU_ROOT is /home/dokku
chown -R dokku:dokku /home/dokku/lolipop/DOKKU_FILES
chmod -R +r /home/dokku/lolipop/DOKKU_FILES
```


Once that is done, any deploy should automatically add the file to the `/app` directory of the image, and the files will be available during container runs.

### caveats

- Does not copy directories
- Does not copy files prefixed by periods
- Will not persist mode on copied files
