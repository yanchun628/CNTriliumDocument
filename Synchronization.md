Trilium is offline-first note taking application - when you use the desktop application, all the data is stored locally, but you also have an option to set up synchronization to the server instance. When you add another desktop client, you can get to star-shaped topology:

[[images/star-topology.png]]

This means that there's one central server (we'll call this instance _sync server_) and several _client_ (sometimes called _desktop_) instances which all point to this sync server and synchronize against it.

Once sync is set up, synchronization is automatic and ongoing - you don't need to trigger it manually. It should "just work".

## How to set up synchronization

### Setup synchronization from desktop instance to sync server

This approach is used when you already have a desktop instance of Trilium and you want to [[setup sync server on your web host|Server installation]]. 

So let's assume your server instance is already deployed but it's uninitialized (no data). Then open your desktop instance, click on Options -> Sync tab -> Sync configuration and set "Server instance address" to point to your sync server. Click Save.

[[images/sync-config.png]]

Now click on "Test sync" button which will tell you if the handshake with sync server succeeded. If yes, sync with sync server started - client started pushing all the data towards the server instance. This might take some time to finish, but you can close the Options dialog and keep using Trilium.

You can also check the server instance periodically to see if the sync finished. Once it's finished you should see the login screen.

### Setup synchronization from sync server do desktop instance

This is used when you already have sync server and you want to set up desktop instance to sync with (from) it.

Here we assume that you downloaded [the most recent release](https://github.com/zadam/trilium/releases/latest) for your platform, unzipped it and ran it.

Since the desktop instance is completely empty, it will first ask if you want to create initial document or you want to set up sync with sync server - you need to choose the second option.

[[images/sync-init.png]]

You'll need to configure Trilium server address and importantly also correct username / password (sync setup requires authentication).

Click on "Finish setup" button and if everything went fine, you'll see this screen:

[[images/sync-in-progress.png]]

Once the sync is finished, you'll be automatically redirected to the Trilium application.

## Proxy setup

Two different setups are supported:

* you can explicitly set proxy server to be used in Options / Sync. Only unauthenticated proxy servers are currently supported.
* if no proxy server is explicitly configured, then Trilium will use system proxy settings 

## Conflict resolution

You can sometimes encounter a situation where you edit same note in multiple instances before the note changes are synchronized.

Trilium handles this situation by just picking up the newer change and discarding the older change. The older change should still be visible in [[note revisions]] so it should be possible to recover any data lost in conflict resolution.

## Hash check

After each completed sync, Trilium computes hash of all synced data on both client and sync server. If there's a difference, something went wrong and Trilium will run automatic recovery mechanism.