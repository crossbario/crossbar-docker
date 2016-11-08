This contains the build stuff for Crossbar.io / Docker on x86-64.

## Build and Deploy

To build and deploy the Crossbar.io Docker image to DockerHub, do:

```console
make build
make test
make publish
```

## End-user Experience

The end user experience of a user with only Docker installed is one command to download the Crossbar.io Docker image and start a new container with Crossbar.io and a default node configuration:

    sudo docker run -it -p 8080:8080 crossbario/crossbar

This will log something like:

```console
(python279_1)oberstet@thinkpad-t430s:~$ sudo docker run -it -p 8080:8080 crossbario/crossbar
Unable to find image 'crossbario/crossbar:latest' locally
latest: Pulling from crossbario/crossbar
203137e8afd5: Already exists
2ff1bbbe9310: Already exists
933ae2486129: Already exists
a3ed95caeb02: Already exists
2d42d06df78a: Pull complete
e03ae9db9b1e: Pull complete
ace459e00fa2: Pull complete
d7fec1afe33f: Pull complete
99025136c0c6: Pull complete
de036859af2f: Pull complete
0c01584329af: Pull complete
Digest: sha256:908f5dddbb5f3f81c31b51a7788e5dfdd9861bacd5ddd802e9a6f22a1de9b349
Status: Downloaded newer image for crossbario/crossbar:latest
2016-03-26T16:20:19+0000 [Controller      1] Automatically choosing optimal Twisted reactor
2016-03-26T16:20:19+0000 [Controller      1] Running on Linux and optimal reactor (epoll) was installed.
2016-03-26T16:20:20+0000 [Controller      1] New node key generated!
2016-03-26T16:20:20+0000 [Controller      1]      __  __  __  __  __  __      __     __
2016-03-26T16:20:20+0000 [Controller      1]     /  `|__)/  \/__`/__`|__) /\ |__)  |/  \
2016-03-26T16:20:20+0000 [Controller      1]     \__,|  \\__/.__/.__/|__)/~~\|  \. |\__/
2016-03-26T16:20:20+0000 [Controller      1]
2016-03-26T16:20:20+0000 [Controller      1]     Crossbar.io Version: 0.13.0
2016-03-26T16:20:20+0000 [Controller      1]     Node Public Key: 9cd453a2275c191ca492afd5b518fa02d28f1de538c0c0a55aa011fa76715216
2016-03-26T16:20:20+0000 [Controller      1]
2016-03-26T16:20:20+0000 [Controller      1] Running from node directory '/var/crossbar/.crossbar'
2016-03-26T16:20:20+0000 [Controller      1] Controller process starting (PyPy-EPollReactor) ..
2016-03-26T16:20:20+0000 [Controller      1] Node configuration loaded from 'config.json'
2016-03-26T16:20:20+0000 [Controller      1] Node ID '833733a93566' set from hostname
2016-03-26T16:20:20+0000 [Controller      1] Using default node shutdown triggers [u'shutdown_on_worker_exit']
2016-03-26T16:20:20+0000 [Controller      1] Joined realm 'crossbar' on node management router
2016-03-26T16:20:20+0000 [Controller      1] Starting Router with ID 'worker1'...
2016-03-26T16:20:21+0000 [Router         12] Worker process starting (PyPy-EPollReactor) ..
2016-03-26T16:20:22+0000 [Controller      1] Router with ID 'worker1' and PID 12 started
2016-03-26T16:20:22+0000 [Router         12] Realm 'realm1' started
2016-03-26T16:20:22+0000 [Controller      1] Router 'worker1': realm 'realm1' (named 'realm1') started
2016-03-26T16:20:22+0000 [Controller      1] Router 'worker1': role 'role1' (named 'anonymous') started on realm 'realm1'
2016-03-26T16:20:22+0000 [Router         12] Site starting on 8080
2016-03-26T16:20:22+0000 [Controller      1] Router 'worker1': transport 'transport1' started
...
```

You can also offer an alternative configuration for Crossbar.io by mounting your own node volume. The volume mounted should contain a `.crossbar` folder containing `config.json` at a minimum.

Running the Crossbar.io Docker image with a custom node configuration volume can be performed as follows:

    sudo docker run -it -p 8080:8080 -v /local/path:/node crossbario/crossbar
