# transmission-seedbox

Simple docker-compose config to start a lightweight Transmission client and UI
seedbox.

## Setup

1. Edit `docker-compose.yml` to edit the `USER` and `PASS` environment
   variables to set up the HTTP auth protection of the web UI.

2. Edit `http/Caddyfile` to set the URL that the HTTP server will listen to.
   Also edit the username and password fields to be consistent with those set
   in step #1.

## Usage

Just run `docker-compose up`!
