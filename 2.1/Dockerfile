FROM openshift/jenkins-slave-base-centos7

# Labels consumed by Red Hat build service
LABEL com.redhat.component="rh-dotnet21-jenkins-slave-docker" \
      name="dotnet/dotnet-21-jenkins-slave-centos7" \
      version="2.1" \
      architecture="x86_64" \
      release="1" \
      io.k8s.display-name="Jenkins Slave .NET Core 2.1" \
      io.k8s.description="The jenkins slave dotnet image has the dotnet tools on top of the jenkins slave base image." \
      io.openshift.tags="openshift,jenkins,slave,dotnet,dotnet21"

# Don't download/extract docs for nuget packages
# Don't do initially populate of package cache
# Enable nodejs and dotnet scl
ENV DOTNET_CORE_VERSION=2.1 \
    BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable" \
    ENABLED_COLLECTIONS="rh-nodejs8 rh-dotnet21" \
    NUGET_XMLDOC_MODE=skip \
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1

COPY contrib/bin/scl_enable /usr/local/bin/scl_enable

# Install
RUN yum install -y centos-release-dotnet centos-release-scl-rh && \
    INSTALL_PKGS="rh-dotnet21 rh-nodejs8-npm" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
# yum cache files may still exist (and quite large in size)
    rm -rf /var/cache/yum/*

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001
