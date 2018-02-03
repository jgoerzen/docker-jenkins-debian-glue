FROM jgoerzen/debian-base-apache:stretch

RUN mv /usr/sbin/policy-rc.d.disabled /usr/sbin/policy-rc.d && \
    apt-get update && \
    apt-get -y --no-install-recommends install gnupg2 fakechroot && \
    wget https://raw.github.com/mika/jenkins-debian-glue/master/puppet/apply.sh && \
    bash ./apply.sh docker_password && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && /usr/local/bin/docker-wipelogs && \
    rm apply.sh && \
    mv /usr/sbin/policy-rc.d /usr/sbin/policy-rc.d.disabled

# We need to work around issues with debootstrap trying to chroot and mount
# /proc inside Docker.  This doesn't work.  That's why we install fakechroot
# above.

RUN sed -e 's/cowbuilder --create/fakechroot cowbuilder --create/ \
        -e 's/--variant=buildd/--variant=fakechroot' \
        -e 's/cowbuilder --update/fakechroot cowbuilder --update/ \
        -i /usr/bin/build-and-provide package &&
    sed -i 's:piuparts_wrapper:piuparts_wrapper, /usr/bin/fakechroot:' /etc/sudoers.d/jenkins


EXPOSE 8080
CMD ["/usr/local/bin/boot-debian-base"]
