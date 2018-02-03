* jenkins-debian-glue for Docker, auto setup package


Run with something like:

    docker run --cap-add=SYS_ADMIN -p 8080:8080 --stop-signal=SIGPWR -t -d --name=jdg

The SYS_ADMIN cap is necessary for pbuilder, which wants to bind-mount proc.
