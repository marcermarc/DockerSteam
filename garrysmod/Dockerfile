FROM marcermarc/steamcmd:latest

LABEL maintainer "docker@marcermarc.de"

# -----------------------------------------------------
# install needed packages for garrysmod
# install cstrike and garrysmod and configure the mount
# -----------------------------------------------------
RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive \
    apt-get -y install lib32stdc++6 lib32tinfo5 \
 && apt-get -y autoremove \
 && apt-get -y clean \
 && apt-get -y purge \
 && su -l steam -c " \
        /steam/cmd/steamcmd.sh \
         +login anonymous \
         +force_install_dir /steam/cstrike \
         +app_update 232330 validate \
         +force_install_dir /steam/gmod \
         +app_update 4020 validate \
         +quit \    
     && echo '"mountcfg"{"cstrike" "/steam/cstrike"}' >> /steam/gmod/garrysmod/cfg/mount.cfg \
    "

# -----------------------------------------------
# change to the executing user from the baseimage
# -----------------------------------------------
USER steam

# ----------------------------------------------------------
# variables:
# * GAMEMODE: change the gamemode
# * PLAYER: maximum player on the server
# * MAP: select the map at the start of the server
# * AUTHKEY and WORKSHOPCOLLECTION: for workshop-collections
# ----------------------------------------------------------
ENV GAMEMODE=terrortown \
    PLAYER=32 \
    MAP=gm_construct \
    AUTHKEY= \
    WORKSHOPCOLLECTION=

# ---------------------------------------
# volumes:
# * the server-configuration
# * the data folder (used by some addons)
# * the cache of workshop-downloads
# ---------------------------------------
VOLUME /steam/gmod/garrysmod/cfg/server.cfg \
       /steam/gmod/garrysmod/data/ \
       /steam/gmod/garrysmod/cache/srcds/

# -----------------------------------
# only port 27015 has to be forwarded
# -----------------------------------
EXPOSE 27015:27015/tcp 27015:27015/udp

WORKDIR /steam/gmod
ENTRYPOINT ["./srcds_run", "-game", "garrysmod", "-nohltv", "-norestart", "+gamemode", "$GAMEMODE", "-maxplayers", "$PLAYER", "+map", "$MAP", "-authkey", "$AUTHKEY", "+host_workshop_collection", "$WORKSHOPCOLLECTION"]