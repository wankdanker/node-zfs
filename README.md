                      __                             ___ 
                     /\ \                          /'___\
      ___     ___    \_\ \     __           ____  /\ \__/  ____
    /' _ `\  / __`\  /'_` \  /'__`\ _______/\_ ,`\\ \ ,__\/',__\
    /\ \/\ \/\ \L\ \/\ \L\ \/\  __//\______\/_/  /_\ \ \_/\__, `\
    \ \_\ \_\ \____/\ \___,_\ \____\/______/ /\____\\ \_\\/\____/
     \/_/\/_/\/___/  \/__,_ /\/____/         \/____/ \/_/ \/___/

[![Build Status](https://secure.travis-ci.org/calmh/node-zfs.png)](http://travis-ci.org/calmh/node-zfs)

node-zfs
========

node-zfs is a simple wrapper around the `zfs` and `zpool` commands. It enables
ZFS management scripting using Node.js under SmartOS, Solaris, OpenIndiana and
FreeBSD. Node-zfs is also tested successfully on Ubuntu.

# Installation

Using npm: 

```
$ npm install --save zfs
```
# Usage

```
var zfsFilesystem = require('zfs');
```

##Now you can address the zfs library as follows:

### To use the zfs command implementation:
```
zfsFilesystem.zfs.
```
###To use the zpool implementation:

```
zfsFilesystem.zpool.
```

## Implemented commands for the ZFS tool

###ZFS list

The list command lists a specific zfs dataset or all datasets. All possible options can be found inside the lib/zfs.js file.

```
zfsFilesystem.zfs.list(function (err, list) {
    console.log(list)
});
```

###ZFS get

Get the parameters of a specific dataset or all datasets. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    name: 'my-dataset-name',
    property: 'quota'
};

zfsFilesystem.zfs.get(opts, function (err, options) {
    console.log(options);
});
```

###ZFS set

Set a specific option for a given dataset. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    name: 'my-dataset-name',
    property: 'quota',
    value: '1024G'
};

zfsFilesystem.zfs.set(opts, function (err, output) {
    console.log(output);
});
```

###ZFS Destroy

Remove a dataset from the ZFS filesystem

```js
var opts = {
    name: 'my-dataset-name'
};

zfsFilesystem.zfs.destroy(opts, function (err, output) {
    console.log(output);
});
```

###ZFS Create

Create a new dataset inside the ZFS filesystem. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    name: 'my-new-dataset-name'
};

zfsFilesystem.zfs.create(opts, function (err, output) {
    console.log(output);
});
```

###ZFS Snapshot

Creates a snapshot with the given name. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    name: 'my-new-snapshot-name',
    dataset: 'my-dataset-name'
};

zfsFilesystem.zfs.snapshot(opts, function (err, output) {
    console.log(output);
});
```

###ZFS Clone

Creates a clone of the given snapshot. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    snapshot: 'my-snapshot-name',
    dataset: 'my-mountpoint-name'
};

zfsFilesystem.zfs.clone(opts, function (err, output) {
    console.log(output);
});
```


### ZFS Mount

Mount the filesystem with the specified name or all filesystems. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    dataset: 'my-filesystem-name'
};

zfsFilesystem.zfs.mount(opts, function (err, output) {
    console.log(output);
});
```
### ZFS Unmount

Unmount the filesystem or the mountpoint or all filesystems. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    name: 'my-filesystem-name',
    force: true
};

zfsFilesystem.zfs.unmount(opts, function (err, output) {
    console.log(output);
});
```

### ZFS Send

Initiates a send of a given snapshot and returns a readable stream. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    snapshot : '/pool/dataset@snapshot',
    verbose : true
};

zfsFilesystem.zfs.send(opts, function (err, sendStream) {
    sendStream.on('error', function (err) {
        console.error(err);
    });

    sendStream.on('verbose', function (data) {
        console.log(data);
    });

    sendStream.pipe(process.stdout).on('end', function () {
        console.log('done');
    });
});
```

### ZFS Receive

Initiates a receive and returns a writable stream to which a send stream may be written. All possible options can be found inside the lib/zfs.js file.

```js
var opts = {
    dataset : '/pool2/dataset',
    verbose : true
};

zfsFilesystem.zfs.receive(opts, function (err, receiveStream) {
    receiveStream.on('error', function (err) {
        console.error(err);
    });

    receiveStream.on('verbose', function (data) {
        console.log(data);
    });

    process.stdin.pipe(receiveStream).on('end', function () {
        console.log('done');
    });
});
```

## Implemented commands for the ZPool tool

###ZPool Create

Creates a new storage pool containing the virtual devices specified. All possible options can be found inside the lib/zpool.js file.

```js
var opts = {
    name: 'my-datastore',
    devices: ['/dev/vdb', '/dev/vdc']
};

zfsFilesystem.zpool.create(opts, function (err, output) {
    console.log(output);
});
```

###ZPool Add

Adds the specified virtual devices to the given pool. All possible options can be found inside the lib/zpool.js file.

```js
var opts = {
    name: 'my-datastore',
    devices: '/dev/vdd'
};

zfsFilesystem.zpool.add(opts, function (err, output) {
    console.log(output);
});
```

###ZPool Set

Sets the given property on the specified pool. See the zpool manpage for possible options and values.

```js
var opts = {
    name: 'my-datastore',
    property: 'comment',
    value: 'Added some comment to a datastore'
};

zfsFilesystem.zpool.set(opts, function (err, output) {
    console.log(output);
});
```

###ZPool Destroy

Destroys the given pool, freeing up any devices for other use.

```js
var opts = {
    name: 'my-datastore',
};

zfsFilesystem.zpool.destroy(opts, function (err, output) {
    console.log(output);
});
```

###ZPool List

Lists the given pools along with a health status and space usage. If no pools are specified, all pools in the system are listed. 

```js
zfsFilesystem.zpool.list(function (err, output) {
    console.log(output);
});
```

###ZPool Get

Retrieves the given list of properties. When no name and property specified, it shows all properties for all datastores. See the zpool manpage for possible options and values.

```js
zfsFilesystem.zpool.get(function (err, output) {
    console.log(output);
});
```

License
-------

MIT
