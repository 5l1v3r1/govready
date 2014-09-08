### Amazon Linux (2014.03 HVM ami-76817c1e) quickstart (64 bit)
Note: **This is experimental and is not currently working!!**

Help us fix this issue! https://github.com/GovReady/govready/issues/45

```
# Download OpenSCAP RPMs for Amazon Linux. (Thanks to Owen for building the RPMs)
# Note: This is experimental, no signing yet of RPMs

wget http://0e01fbc32a350ec514ac-c80f4f0ac7f2efb7e499607e5e8fd7f4.r76.cf5.rackcdn.com/openscap-1.0.3-2.amzn1.x86_64.rpm
wget http://0e01fbc32a350ec514ac-c80f4f0ac7f2efb7e499607e5e8fd7f4.r76.cf5.rackcdn.com/openscap-devel-1.0.3-2.amzn1.x86_64.rpm
wget http://0e01fbc32a350ec514ac-c80f4f0ac7f2efb7e499607e5e8fd7f4.r76.cf5.rackcdn.com/openscap-engine-sce-1.0.3-2.amzn1.x86_64.rpm
wget http://0e01fbc32a350ec514ac-c80f4f0ac7f2efb7e499607e5e8fd7f4.r76.cf5.rackcdn.com/openscap-engine-sce-devel-1.0.3-2.amzn1.x86_64.rpm
wget http://0e01fbc32a350ec514ac-c80f4f0ac7f2efb7e499607e5e8fd7f4.r76.cf5.rackcdn.com/openscap-extra-probes-1.0.3-2.amzn1.x86_64.rpm
wget http://0e01fbc32a350ec514ac-c80f4f0ac7f2efb7e499607e5e8fd7f4.r76.cf5.rackcdn.com/openscap-python-1.0.3-2.amzn1.x86_64.rpm
wget http://0e01fbc32a350ec514ac-c80f4f0ac7f2efb7e499607e5e8fd7f4.r76.cf5.rackcdn.com/openscap-selinux-1.0.3-2.amzn1.noarch.rpm
wget http://0e01fbc32a350ec514ac-c80f4f0ac7f2efb7e499607e5e8fd7f4.r76.cf5.rackcdn.com/openscap-utils-1.0.3-2.amzn1.x86_64.rpm

# Install the OpenSCAP RPMs using localinstall method
sudo yum --nogpgcheck localinstall -y *.rpm

# Install SCAP-Security-Guide
sudo yum install --enablerepo=epel scap-security-guide

# Set a password for root
sudo passwd root

# Install govready using curl. govready will install OpenSCAP and SCAP-Security-Content
curl -Lk io.govready.org/install | sudo bash

# Switch to root so scanner can run all tests properly
su -

# Create a directory and cd into it
mkdir myfisma
cd myfisma

# Initialize the directory
govready init

# Import Amazon cpe-dictionary.xml and cpe-oval.xml SCAP data into local scap/content directory
govready import https://raw.githubusercontent.com/GovReady/govready/master/templates/ssg-amzn2014.03.2hvm-cpe-dictionary.xml
govready import https://raw.githubusercontent.com/GovReady/govready/master/templates/ssg-amzn2014.03.2hvm-cpe-oval.xml

# Update GovReadyfile using sed command (or update the CPE line manually using a text editor)
sed -i 's:^CPE.*:CPE = scap/content/ssg-amzn2014.03.2hvm-cpe-dictionary.xml:' GovReadyfile

# Run a scan
govready scan

# FAILURE - Here is where we get a segmentation fault! This is a problem.


```
