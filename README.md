# Ansible Site for FX2 Deployments

Steps of deploying FX2s:

**Note:** Use `--limit` for any of these playbooks to avoid running on the entire hosts file, which you **should do** because otherwise things in production will factory reset. Using `--check` mode is also a good idea.

1. Manually access the CMC
   1. Factory reset the CMC
   2. Setup networking so the CMC is reachable
   3. No need to touch the slots yet
2. Add nodes to `hosts` file in their correct group (`FC430` vs `FC630` vs `FC830`)
3. `ansible-playbook cmc-status.yaml` to check if the CMC is reachable.
4. `ansible-playbook cmc-init.yaml` which will set the initial CMC config.
5. `ansible-playbook cmc-idrac-factoryreset.yaml` which will factory reset all iDracs in each chassis. Wait 10 mins for everything to reset.
6. `ansible-playbook cmc-idrac-init.yaml` which will set the initial config for each newly reset iDrac. iDracs will have DHCP enabled after this. Wait a few minutes for the leases to kick in.
7. `ansible-playbook idrac-init.yaml` which will set the initial idrac config that needs to be done on the idrac itself, not the cmc.
