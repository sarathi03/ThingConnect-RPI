# ThingConnect-RPI
ThingConnect Gateway and CLI was successfully dumped into the Raspberrypi (Ubuntu Core 24.4)

https://chatgpt.com/share/69146138-2f58-8007-9b5d-555abb65dc9b
thingconnect gateway's ChatGPT Reference

After installation (on Ubuntu Core / Pi)
sudo snap install thingconnect-gateway_1.0.0_arm64.snap --dangerous
# Check services
sudo snap services thingconnect-gateway
# Start manually (if not auto-started)
sudo snap start thingconnect-gateway.tc-gateway
# Run CLI
thingconnect-gateway.tc-cli --help
# View logs
sudo snap logs thingconnect-gateway.tc-gateway -f
5:36
Got it — the LXD build can’t find your CLI publish folder. Let’s fix the path and copy the CLI binary there.
Run these exact commands:

# 1) Go to repo root
cd ~/ThingConnect.Gateway

# 2) Ensure both publish folders exist
mkdir -p src/ThingConnect.Gateway/bin/Release/net8.0/linux-arm64/publish
mkdir -p src/ThingConnect.Gateway.CLI/bin/Release/net8.0/linux-arm64/publish

# 3) Copy Gateway publish (from your Downloads/ThingConnect.Gateway folder)
cp -r ~/Downloads/ThingConnect.Gateway/* \
  src/ThingConnect.Gateway/bin/Release/net8.0/linux-arm64/publish/

# 4) Copy CLI (yours is a single 75MB file in Downloads)
cp ~/Downloads/ThingConnect.Gateway.CLI \
  src/ThingConnect.Gateway.CLI/bin/Release/net8.0/linux-arm64/publish/

# 5) Make both binaries executable
chmod +x src/ThingConnect.Gateway/bin/Release/net8.0/linux-arm64/publish/ThingConnect.Gateway
chmod +x src/ThingConnect.Gateway.CLI/bin/Release/net8.0/linux-arm64/publish/ThingConnect.Gateway.CLI

# 6) Verify they exist (you should see two files listed)
ls -l src/ThingConnect.Gateway*/bin/Release/net8.0/linux-arm64/publish/ThingConnect.*
If you see both files in step 6, build again (LXD backend):

cd ~/ThingConnect.Gateway/snap
snapcraft --use-lxd
If it still complains, paste the output of:

ls -l ../src/ThingConnect.Gateway.CLI/bin/Release/net8.0/linux-arm64/publish/
