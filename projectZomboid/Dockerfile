FROM marcermarc/steamcmd:latest

LABEL maintainer "docker@marcermarc.de"

# ---------------------------
# install project zomboid 
# remove useless 32-bit files
# ---------------------------
RUN su -l steam -c " \
        /steam/cmd/steamcmd.sh \
         +login anonymous \
         +force_install_dir /steam/pz \
         +app_update 380870 validate \
         +quit \
	  " \
	&& rm -R /steam/pz/ProjectZomboid32 /steam/pz/ProjectZomboid32.json /steam/pz/jre /steam/pz/linux32

# -----------------------------------------------
# change to the executing user from the baseimage
# -----------------------------------------------
USER steam

# ---------------------------------
# volumes:
# * the data folder with world etc.
# ---------------------------------
VOLUME /steam/pz/data

# -----------------------------------
# environment defined in start script
# -----------------------------------
ENV PATH="/steam/pz/jre64/bin:$PATH" \
    LD_LIBRARY_PATH="/steam/pz/linux64:/steam/pz/natives:/steam/pz:/steam/pz/jre64/lib/amd64:{LD_LIBRARY_PATH}"

# -----------------------------------
# port forwarding:
# - 8766 and 8767 for steam
# - 16261 for game
# - 16262 and up for every slot
# -----------------------------------
EXPOSE 8766:8766/tcp 8767:8767/tcp 16261:16261/tcp

WORKDIR /steam/pz
ENTRYPOINT ["./ProjectZomboid64", "-pzexejavacmd", "/steam/pz/jre64/bin/java -classpath /steam/pz/pzexe.jar zombie.pzexe", "-pzexeconfig", "ProjectZomboid64.json", "-cachedir=/steam/pz/data"]

# -------------------------------------------------------------------
# change the command to change adminpassword and add extra parameters
# -------------------------------------------------------------------
CMD ["-adminpassword", "123456"]