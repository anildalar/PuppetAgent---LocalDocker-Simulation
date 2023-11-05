# PuppetAgent---LocalDocker-Simulation
PuppetAgent - LocalDocker Simulation


# Use the Ubuntu base image
FROM ubuntu:latest

# Use root user for initial setup
USER root

# Update package lists and install necessary dependencies
RUN mkdir -p /var/lib/apt/lists/partial && chmod 755 /var/lib/apt/lists/partial
RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo \
    python3.0 \
    python3-pip

# Install Puppet agent
RUN apt-get install -y wget
RUN wget https://apt.puppet.com/puppet7-release-focal.deb
RUN dpkg -i puppet7-release-focal.deb
RUN apt-get update && apt-get install -y puppet-agent


# Set the Puppet agent command with the hosts file modification as the entry point
CMD sh -c "echo '172.18.0.2 puppet' | sudo tee -a /etc/hosts > /dev/null && sudo /opt/puppetlabs/bin/puppet agent --no-daemonize --verbose"
