# transmission-seedbox

Simple [docker-compose][docker-compose] config to start a password-protected
lightweight [Transmission torrent][transmission] seedbox with web UI (proxied
through a [Caddy][caddy] HTTP server.

[docker-compose]: https://docs.docker.com/compose/
[transmission]: https://transmissionbt.com/
[caddy]: https://caddyserver.com/

## Usage

0. Make sure that [docker][docker] and [docker-compose][docker-compose] are
   installed.

1. Edit `.env` and change the `USER` and `PASS` environment variables to set up
   the basic HTTP auth protection of the web UI.

2. Run `docker-compose up`! You should see both the torrent client and web
   server logs in your terminal. 
   
   By default the web server starts on port 80.  To change this port, set the
   `HTTP_PORT` environment variable like: `HTTP_PORT=8000 docker-compose up`.

3. Visit your server's address in a web browser to access Transmission web UI
   (e.g., `http://yourserver/`). Login using the username and password set in 
   step #1. Use the web UI to upload and manage torrents.

4. Once a torrent has been completed, visit the `/downloads` URL path of your
   server (e.g., `http://yourserver/downloads`) to access and download the 
   files.

   If you have a lot of files in your `downloads` folder, it may be helpful to
   use `wget` to recursively grab them all. For example:
   `wget --no-parent --recursive http://yourserver/downloads/complete/`.


5. To stop the torrent client and server, press `Ctrl-C` or issue the command:
   `docker-compose down`.

**Persistence:** To keep the torrent client and web server running even after
logging out of the server, use a [tmux][tmux] or [screen][screen] client. Or
in step #2, use `docker-compose up -d` to run the services in "detached" mode.

[docker]: https://docs.docker.com/
[tmux]: https://thoughtbot.com/blog/a-tmux-crash-course
[screen]: https://linuxize.com/post/how-to-use-linux-screen/

## Design philosophy

The general goal is to have a simple and quick way of starting a seedbox on any
docker-ready server: just clone this repo and type `docker-compose up`. It
should "just work" without any configuration (except setting username and
password).

The torrent client should have a web UI and docker image, which meant that we
had the following choices: [rtorrent][rtorrent], [deluge][deluge],
[transmission][transmission-docker], and [qbittorent][qbittorent]. Although
rtorrent seemed popular with seedboxes, its startup time through docker was
very slow and its configuration seemed complex (needing its own nginx).
Transmission seemed to be the easiest to setup out of the box with minimal
configuration; startup time was also very fast.

For security and performance, we want to proxy Transmission's web UI through a
web server. [Caddy][caddy] was selected over [nginx][nginx] for its ease in
configuration. We also use Caddy to serve the completed torrents over HTTP so
that we may re-download them to local computer.

[rtorrent]: https://hub.docker.com/r/linuxserver/rutorrent/
[deluge]: https://hub.docker.com/r/linuxserver/deluge/
[transmission-docker]: https://hub.docker.com/r/linuxserver/transmission/
[qbittorent]: https://hub.docker.com/r/linuxserver/qbittorrent/
[nginx]: https://nginx.org/en/

