# PuppetAgent---LocalDocker-Simulation
PuppetAgent - LocalDocker Simulation


<pre>

docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <puppet_server_container_name_or_id>
# Use a base image (choose an appropriate base image)
FROM ubuntu:latest

# Update package lists and install required dependencies
RUN apt-get update && apt-get install -y wget

# Download Puppet agent .deb package
RUN wget https://apt.puppet.com/puppet7-release-jammy.deb && \
    dpkg -i puppet7-release-jammy.deb && \
    apt-get update

# Install Puppet agent
RUN apt-get install -y puppet-agent

# Set environment variables for Puppet
ENV PATH="/opt/puppetlabs/bin:$PATH"

# Your additional Dockerfile configurations...

# Start your application or perform additional configurations
# Set the Puppet agent command with the hosts file modification as the entry point
CMD sh -c "echo '172.18.0.2 puppet' | tee -a /etc/hosts > /dev/null && /opt/puppetlabs/bin/puppet agent --no-daemonize --verbose"

</pre>
<pre>
docker image build -t puppet-agent:t1 .
docker network create puppet-network

docker run -d --network puppet-network --name puppet-agent-node1  puppet-agent:t1
docker run -d --network puppet-network --name puppet-agent-node2  puppet-agent:t1
docker run -d --network puppet-network --name puppet-agent-node3  puppet-agent:t1
docker run -d --network puppet-network --name puppet-agent-node4  puppet-agent:t1
docker run -d --network puppet-network --name puppet-agent-node5  puppet-agent:t1
...
...
</pre>
