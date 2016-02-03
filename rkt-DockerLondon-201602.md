%title: Docker London 2016 - Introduction to `rkt`
%author: @lukeb0nd luke@yld.io
%date: 2016-02-03




-> Introduction to <-
\                                  \_    \_   
                             \_ \__| | \_| |\_ 
                            | '\__| |/ / \__|
                            | |  |   <| |\_ 
                            |\_|  |\_|\\\_\\\\\__|


-> # Docker London <-
-> ## February 2016 <-


-> Luke Bond <-
-> YLD.io <-
-> @lukeb0nd <-

---

# WHO IS THIS TALK FOR?

This talk is for you if you:

- are curious about what `rkt` is
- are curious about why `rkt` exists
- want to know how it differs from Docker
- are confused about rkt, runc, appc and the Container Wars timeline

I'll also give a little demo of how to use `rkt`.

---

# WHO AM I?

- I'm a backend developer
- Specialise in Node.js these days
- DevOps-curious
- Docker
- Built some things with Docker on CoreOS
  - Including a thing called "Paz" - [http://paz.sh]

I work for YLD.io, a London-based software engineering consultency
that specialises in Node.js, Docker and React.

---

# THE CONTAINER WARS

-> *That* blog post: [https://coreos.com/blog/rocket/] <-

> We should stop talking about Docker containers,
> and start talking about the Docker Platform.
> It is not becoming the simple composable building
> block we had envisioned.
-> -- *Alex Polvi*, CoreOS, 1/12/2014 on the CoreOS blog <-

So they started building `rkt`.

---

# THE CONTAINER WARS

> While we are at it, we are cleaning up and fixing a few things
> that we’d like to see in a production ready container.
-> -- *Ibid.* <-

CoreOS felt strongly about the following:
- Composability
- Security
- Image Distribution
- Openness

This mostly comes back to Docker's design, with its daemon and
monolithic binary. For the vast majority of people this is fine.
We love Docker's UX and an integrated platform only improves that.

---

# WHY DOES COREOS CARE ABOUT THESE THINGS SO MUCH?

Composability
- independant, composable tools; not one monolith
- the Docker daemon precludes composability of container processes

Security
- note that this is before the large strides Docker took in 2015

Image Distribution
- open to alternative protocols & distribution methods
- discovery leveraging standard technologies like DNS
- not a registry

Openness
- CoreOS called out Docker for removing the "Open Container Manifesto"

---

# RKT, RUNC, APPC & DOCKER

`rkt` was announced alongside "App Container" specifications:
- *App Container Image* - signed and optionally encrypted tgz
- *App Container Runtime* - the actual launching of containers
- *App Container Discovery* - discovering and downloading images

The `appc` GitHub organisation has been active since November 2014.

Check out the repositories at https://github.com/appc

---

# RKT, RUNC, APPC & DOCKER

Fast-forward to DockerCon San Francisco, June 22nd 2015, when
Docker announced the *Open Container Project* (now called the
*Open Container Initiative*).

> Docker will be donating both our base container format and runtime,
> runC, to this project, to help form the cornerstone for the new
> technology.  And, in a particularly exciting recent development,
> the talented people behind appc are now joining us as co-founders.
> ...
> I am especially grateful that Alex Polvi and Brandon Phillips
> from CoreOS, the founders of appc, will be joining the OCP,
> as it speaks volumes about our common desire to help unite the
> industry and to take the best ideas, wherever they originated,
> into something that provides the best outcomes for users and
> the industry.
-> -- *Ben Golub*, Docker, 22/6/2015 on the Docker blog <-

So OCI was founded with Docker reference specs, not appc.

> Part of this [the creation of `rkt`] was the appc container
> spec. This will now merge with the Docker container format (2.0)
> under OCP.  It’s a no brainer.  **Appc can run a victory lap**
> **right now: it shook things up; job done.**
-> -- *Alexis Richardson*, Weaveworks, 22/6/2015 on the Weaveworks blog<-
-> (Emphasis mine) <-

---

# RKT, RUNC, APPC & DOCKER

Let's recap up to July 2015:

- CoreOS shook things up enough for Docker to commit to open standards
- OCI was founded with Docker's image and runtime, not those of appc
- `runc` is a reference implementation of OCF, but not of appc
- `rkt` is an implementation of appc, but not yet OCF

So that leaves `rkt` in the position of now lagging behind in terms
of support for the now-standard container format:

> Today rkt is a leading implementation of appc, and we plan
> on it becoming a leading implementation of OCP.
-> -- *Alex Polvi*, CoreOS, 22/6/2015 on the CoreOS blog <-

---

# RKT, RUNC, APPC & DOCKER

Fast-forward to December 2015 and this blog post from CoreOS:

> While initially it appeared there was some overlap between OCI
> and appc, the OCI has solely focused on the runtime, which is
> more narrowly focused than we anticipated for the project. Our
> efforts to bring in container image and image distribution of
> appc have not been incorporated, but we still believe these are
> important parts of a container standard.
> ...
> While we are happy the industry has come together to support the
> OCI runtime, CoreOS believes that the the most critical aspect of
> container standards is the distributable container image: the
> part that gives end-user portability.
-> -- *Alex Polvi*, CoreOS, 8/12/2015 on the CoreOS blog <-

It sounds like CoreOS is a bit frustrated.

Is Docker paying lip-service to open standards, dragging its feet
on the part that would really help CoreOS to catch up to them?

---

# THE CONTAINER WARS

- This isn't a war it's business as usual; it's strategy.
- Remember that the container space is young and up for grabs.
- Docker is [making a platform play](https://blog.docker.com/2014/09/docker-closes-40m-series-c-led-by-sequoia/).

> My understanding is that the Docker Platform will be a choice
> for companies that want vSphere for containers— that is, they
> want a whole platform off the shelf.
-> -- *Alex Polvi*, CoreOS, 7/1/2015 on readwrite.com <-

- Docker has a head start and huge adoption and mindshare
- If someone else catches up before platform is complete, they
  risk losing their competitive advantage

---

# THE DOCKER PLATFORM

- Docker daemon model and monolithic CLI tool plays to their strength
- Docker's lack of composability a limiting factor for platform-builders
  - I know this from building Paz
- Those building platforms on Docker's tools are in a conundrum
  - Go all in on Compose & Swarm and have no differentiating tech
  - Build only on the daemon, yet suffer lack of control over processes

---

# RKT - FOR PLATFORM BUILDERS

This is what makes `rkt` so attractive to platform builders.

> There are really two buckets of users for Rocket and they
> could both be considered "platform builders."
>
> The first set of platform builders are companies like Cloud
> Foundry, Mesosphere, or cloud service providers (Amazon Web
> Services, Google, Rackspace) that are building a platform as
> their product. Rocket allows them to add containers to their
> platform while keeping the rest of what they do today.
>
> The second set are enterprises that already have an existing
> environment and want to add containers to it. These would
> typically be large enterprises that have already invested in
> their own internal platform and want to layer in containers.
-> -- *Alex Polvi*, CoreOS, 7/1/2015 on readwrite.com <-

---

# THE CONTAINER WARS - MY TAKE

In Circle CI's blog post titled *Container Wars* (April 2015, pre-OCI),
Paul Biggar paints a grim picture of a bloody war ahead.

- With the benefit of hindsight, I disagree
- Platform-builders will gravitate to `rkt`
- Everyone else (99.9% of people) will continue to fuel Docker's success
- `rkt` will mature and grow in popularity, but mostly in that niche

I don't think this is win/lose for either company.

---

# OVERVIEW OF RKT TOOLS

## Building Images with `acbuild`

Given that `rkt` is a reference implementation of the appc spec, it
natively runs appc images:

> `acbuild` is a command line utility to build and modify App
> Container Images (ACIs), the container image format defined in
> the App Container (appc) spec.

`acbuild` commands are analogous to lines of a `Dockerfile`.

A shell script of `acbuild` commands is the appc equivant of a `Dockerfile`.

Here is an example script for building an appc image
for a basic Node.js app:

    #!/usr/bin/env bash
    acbuild --debug begin
    acbuild --debug set-name lukebond/demo-api
    acbuild --debug dep add quay.io/coreos/alpine-sh
    acbuild --debug run -- apk update
    acbuild --debug run -- apk add nodejs
    acbuild --debug copy package.json /app/package.json
    acbuild --debug run -- /bin/sh -c 'cd /app && npm install'
    acbuild --debug copy index.js /app/index.js
    acbuild --debug port add http tcp 9000
    acbuild --debug set-working-directory /app
    acbuild --debug set-exec -- /usr/bin/node index.js
    acbuild --debug write --overwrite demo-api-latest-linux-amd64.aci

The `docker2aci` tool can be used to convert Docker images to appc.

---

# OVERVIEW OF RKT TOOLS

## Running appc containers with `rkt`

Launching a built appc container is simple:

    $ sudo rkt --insecure-options=image run demo-api-latest-linux-amd64.aci
    rkt: using image from local store for image name coreos.com/rkt/stage1-coreos:0.14.0
    rkt: using image from file demo-api-latest-linux-amd64.aci
    rkt: using image from local store for image name quay.io/coreos/alpine-sh

Killing containers is a bit of a hassle at the moment:

    $ sudo machinectl kill rkt-e31f1475-c9d5-4f62-b318-f9909e50a30d 

`rkt` can also run Docker images.

    $ sudo rkt run --insecure-options=image docker://lukebond/demo-api
    rkt: using image from local store for image name coreos.com/rkt/stage1-coreos:0.14.0
    rkt: remote fetching from URL "docker://lukebond/demo-api"
    $ sudo rkt list
    UUID    APP   IMAGE NAME          STATE NETWORKS
    6671a9dc  demo-api  registry-1.docker.io/lukebond/demo-api:latest running default:ip4=172.16.28.5
    $ machinectl list
    MACHINE                                  CLASS     SERVICE
    rkt-6671a9dc-e398-473c-aeff-74157fcac9f5 container nspawn 

---

# FURTHER READING

[Latest rkt features, including "fly"](https://coreos.com/blog/rkt-0.15.0-introduces-rkt-fly.html)
[rkt announcement - CoreOS blog](https://coreos.com/blog/rocket/)
[Open Container Project announcement - Docker blog](https://blog.docker.com/2015/06/open-container-project-foundation/)
[Weaveworks' Analysis of the OCP announcement - Weaveworks blog](http://www.weave.works/docker-open-container-project-please-make-it-awesome/)
[CoreOS' co-announcement of the OCP - CoreOS blog](https://coreos.com/blog/app-container-and-the-open-container-project/)
[OCP Progress report including name change to OCI - Docker blog](http://blog.docker.com/2015/07/open-container-format-progress-report/)
[CoreOS on OCI, appc and other standards - CoreOS blog](https://coreos.com/blog/making-sense-of-standards/)
