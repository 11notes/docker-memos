![banner](https://github.com/11notes/defaults/blob/main/static/img/banner.png?raw=true)

# MEMOS
![size](https://img.shields.io/docker/image-size/11notes/memos/0.25.0?color=0eb305)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![version](https://img.shields.io/docker/v/11notes/memos/0.25.0?color=eb7a09)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![pulls](https://img.shields.io/docker/pulls/11notes/memos?color=2b75d6)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)[<img src="https://img.shields.io/github/issues/11notes/docker-MEMOS?color=7842f5">](https://github.com/11notes/docker-MEMOS/issues)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![swiss_made](https://img.shields.io/badge/Swiss_Made-FFFFFF?labelColor=FF0000&logo=data:image/svg%2bxml;base64,PHN2ZyB2ZXJzaW9uPSIxIiB3aWR0aD0iNTEyIiBoZWlnaHQ9IjUxMiIgdmlld0JveD0iMCAwIDMyIDMyIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPgogIDxyZWN0IHdpZHRoPSIzMiIgaGVpZ2h0PSIzMiIgZmlsbD0idHJhbnNwYXJlbnQiLz4KICA8cGF0aCBkPSJtMTMgNmg2djdoN3Y2aC03djdoLTZ2LTdoLTd2LTZoN3oiIGZpbGw9IiNmZmYiLz4KPC9zdmc+)

Run memos rootless, distroless and secure by default!

# INTRODUCTION 📢

A modern, open-source, self-hosted knowledge management and note-taking platform designed for privacy-conscious users and organizations. Memos provides a lightweight yet powerful solution for capturing, organizing, and sharing thoughts with comprehensive Markdown support and cross-platform accessibility.

![DASHBOARD](https://github.com/11notes/docker-memos/blob/master/img/Dashboard.png?raw=true)

# SYNOPSIS 📖
**What can I do with this?** Run the prefer IaC reverse proxy distroless and rootless for maximum security.

# UNIQUE VALUE PROPOSITION 💶
**Why should I run this image and not the other image(s) that already exist?** Good question! Because ...

> [!IMPORTANT]
>* ... this image runs [rootless](https://github.com/11notes/RTFM/blob/main/linux/container/image/rootless.md) as 1000:1000
>* ... this image has no shell since it is [distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md)
>* ... this image is auto updated to the latest version via CI/CD
>* ... this image has a health check
>* ... this image runs read-only
>* ... this image is automatically scanned for CVEs before and after publishing
>* ... this image is created via a secure and pinned CI/CD process
>* ... this image is very small

If you value security, simplicity and optimizations to the extreme, then this image might be for you.

# COMPARISON 🏁
Below you find a comparison between this image and the most used or original one.

| **image** | 11notes/memos:0.25.0 | neosmemo/memos |
| ---: | :---: | :---: |
| **image size on disk** | 14.3MB | 65.9MB |
| **process UID/GID** | 1000/1000 | 0/0 |
| **distroless?** | ✅ | ❌ |
| **rootless?** | ✅ | ❌ |


# VOLUMES 📁
* **/memos/var** - Directory of all uploaded data

# COMPOSE ✂️
```yaml
name: "memos"

x-lockdown: &lockdown
  # prevents write access to the image itself
  read_only: true
  # prevents any process within the container to gain more privileges
  security_opt:
    - "no-new-privileges=true"
    
services:
  postgres:
    # detailed info about this image: https://github.com/11notes/docker-postgres
    image: "11notes/postgres:16"
    <<: *lockdown
    environment:
      TZ: "Europe/Zurich"
      POSTGRES_PASSWORD: "${POSTGRES_PASSWORD}"
      POSTGRES_BACKUP_SCHEDULE: "0 3 * * *"
    volumes:
      - "postgres.etc:/postgres/etc"
      - "postgres.var:/postgres/var"
      - "postgres.backup:/postgres/backup"
    tmpfs:
      - "/postgres/run:uid=1000,gid=1000"
      - "/postgres/log:uid=1000,gid=1000"
    networks:
      backend:
    restart: "always"

  app:
    depends_on:
      postgres:
        condition: "service_healthy"
        restart: true
    image: "11notes/memos:0.25.0"
    environment:
      TZ: "Europe/Zurich"
      MEMOS_DSN: "postgresql://postgres:${POSTGRES_PASSWORD}@postgres:5432/postgres?sslmode=disable"
    volumes:
      - "memos.var:/memos/var"
    ports:
      - "3000:5230/tcp"
    networks:
      frontend:
      backend:
    restart: "always"

volumes:
  postgres.etc:
  postgres.var:
  postgres.backup:
  memos.var:

networks:
  frontend:
  backend:
    internal: true
```

# DEFAULT SETTINGS 🗃️
| Parameter | Value | Description |
| --- | --- | --- |
| `user` | docker | user name |
| `uid` | 1000 | [user identifier](https://en.wikipedia.org/wiki/User_identifier) |
| `gid` | 1000 | [group identifier](https://en.wikipedia.org/wiki/Group_identifier) |
| `home` | /memos | home directory of user docker |

# ENVIRONMENT 📝
| Parameter | Value | Default |
| --- | --- | --- |
| `TZ` | [Time Zone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) | |
| `DEBUG` | Will activate debug option for container image and app (if available) | |

# MAIN TAGS 🏷️
These are the main tags for the image. There is also a tag for each commit and its shorthand sha256 value.

* [0.25.0](https://hub.docker.com/r/11notes/memos/tags?name=0.25.0)

### There is no latest tag, what am I supposed to do about updates?
It is of my opinion that the ```:latest``` tag is dangerous. Many times, I’ve introduced **breaking** changes to my images. This would have messed up everything for some people. If you don’t want to change the tag to the latest [semver](https://semver.org/), simply use the short versions of [semver](https://semver.org/). Instead of using ```:0.25.0``` you can use ```:0``` or ```:0.25```. Since on each new version these tags are updated to the latest version of the software, using them is identical to using ```:latest``` but at least fixed to a major or minor version.

If you still insist on having the bleeding edge release of this app, simply use the ```:rolling``` tag, but be warned! You will get the latest version of the app instantly, regardless of breaking changes or security issues or what so ever. You do this at your own risk!

# REGISTRIES ☁️
```
docker pull 11notes/memos:0.25.0
docker pull ghcr.io/11notes/memos:0.25.0
docker pull quay.io/11notes/memos:0.25.0
```

# SOURCE 💾
* [11notes/memos](https://github.com/11notes/docker-MEMOS)

# PARENT IMAGE 🏛️
> [!IMPORTANT]
>This image is not based on another image but uses [scratch](https://hub.docker.com/_/scratch) as the starting layer.
>The image consists of the following distroless layers that were added:
>* [11notes/distroless](https://github.com/11notes/docker-distroless/blob/master/arch.dockerfile) - contains users, timezones and Root CA certificates
>* [11notes/distroless:localhealth](https://github.com/11notes/docker-distroless/blob/master/localhealth.dockerfile) - app to execute HTTP requests only on 127.0.0.1

# BUILT WITH 🧰
* [memos](https://github.com/usememos/memos)

# GENERAL TIPS 📌
> [!TIP]
>* Use a reverse proxy like Traefik, Nginx, HAproxy to terminate TLS and to protect your endpoints
>* Use Let’s Encrypt DNS-01 challenge to obtain valid SSL certificates for your services

# ElevenNotes™️
This image is provided to you at your own risk. Always make backups before updating an image to a different version. Check the [releases](https://github.com/11notes/docker-memos/releases) for breaking changes. If you have any problems with using this image simply raise an [issue](https://github.com/11notes/docker-memos/issues), thanks. If you have a question or inputs please create a new [discussion](https://github.com/11notes/docker-memos/discussions) instead of an issue. You can find all my other repositories on [github](https://github.com/11notes?tab=repositories).

*created 11.08.2025, 15:48:35 (CET)*