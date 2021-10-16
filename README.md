## Description

This is what happens when you mix Ansible + Kubernetes + Minecraft + CloudFlare + quite some spare time.

## How it works

From running a single `ansible-playbook` command, in a few minutes you will be able to join a new Minecraft server through its own domain name.

The workflow looks like this:

1. Run the Ansible playbook
2. Create a new unique namespace in the designed kubernetes cluster
3. Generate a new DNS record for the new server

As shown below, "CLIENT_SEED" is just a unique identifier for each Minecraft server deployed.

## Requirements

- A working kubernetes cluster (it is where we will deploy our Minecraft servers)
- A registered domain name (it is the main thing we will be using to access the Minecraft servers)
- A CloudFlare account (it is where new DNS records will be created)
- An existing subdomain like `minecraft.domain.tld` pointing to the public IP address from which servers will be accessed (it will be used to create new CNAME records for each new Minecraft server)

## Usage

- Create a new Minecraft server for user 'abcd1234':

  ```
  ansible-playbook playbooks/kubernetes.yml -t deploy -e "CLIENT_SEED=abcd1234"
  ```

  _Note: if variable "CLIENT_SEED" is not set it will be generated automatically_

- Destroy an existing Minecraft server (and its data) from user 'abcd1234' (specifying the assigned port is required to avoid mistakes when it comes to deleting stuff):

  ```
  ansible-playbook playbooks/kubernetes.yml -t destroy -e "CLIENT_SEED=abcd1234" -e "MC_SERVER_PORT=25568"
  ```

## Considerations

- This is a PoC and it comes "as is". For example, the persistent storage brought for each Minecraft server by default points to a static hostPath location on the Kubernetes node in which it is supposed to run, that does not necessarily have to already exist on your own nodes.
- Please review the manifests before considering to use these files on your environment to guarantee a minimal usage consistency.

## License

Kubecraft is released under the MIT license.
