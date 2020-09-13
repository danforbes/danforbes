1. Sign up for Google Cloud Platform Free Tier
1. Go to Getting Started > Deploy a prebuilt solution
1. Search for "ubuntu focal"
1. Launch an [Ubuntu Focal](https://console.cloud.google.com/marketplace/details/ubuntu-os-cloud/ubuntu-focal) virtual
   machine (VM)
   - Series: N1
   - Machine type: n1-standard-2
   - Boot disk: 100 GB
   - Identity and API access: No service account
   - Management, security, disks, networking, sole tenancy > Security
     - Block project-wide SSH keys
     - add an SSH key
   - Create
1. Login to the VM
   - Use your VM's external IP address
   - Use the name associated with the SSH key you added
   - `ssh <name>@<address>`
1. Download the Polkadot binary
   - `wget https://github.com/paritytech/polkadot/releases/download/v0.8.23/polkadot`
   - `chmod 755 ./polkadot`
1. Create a launch script, `launch-kusama.sh`
   - Choose a name for the node
   - ```
     #!/bin/sh

     ./polkadot --chain kusama --name=<name> --wasm-execution=Compiled --pruning=archive 
     ```
   - `chmod 755 launch-kusama.sh`
1. Create a service file, `kusama.service` (use the name associated with the SSH key you added)
   - ```
     [Unit]
     Description=Kusama validator node

     [Service]
     User=dan
     WorkingDirectory=/home/<name>
     ExecStart=/home/<name>/launch-kusama.sh
     Restart=always

     [Install]
     WantedBy=multi-user.target
     ```
   - `sudo mv kusama.service /etc/systemd/system`
1. Start the Kusama node as a service
   - `sudo systemctl daemon-reload`
   - `sudo systemctl start kusama.service`
1. Check service status
   - `sudo systemctl status kusama.service`
1. Enable service on every reboot
   - `sudo systemctl enable kusama.service`
1. Log out with `exit`
