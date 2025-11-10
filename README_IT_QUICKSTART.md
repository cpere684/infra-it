```markdown
# infra-it — Quickstart & Safety Checklist

What this package contains
- playbooks/it.yml            : top-level play that calls role 'it'
- inventory.ini              : example inventory (edit host IP and ansible_user)
- group_vars/all.yml         : global variables (shared with other packages)
- roles/it/                  : IT role tasks and handlers
- README_IT_QUICKSTART.md    : this file

Important safety notes
- Netplan/network changes are off by default. To set static IPs, use host_vars/<hostname>.yml for each host.
- The play disables password-based SSH login for safety. Ensure your SSH key is added for the created it_user before locking password auth.
- Test on a single host first with --limit.

Quickstart (3 steps)
1) Prepare inventory.ini with an [it] group and a host line (use ansible_ssh_private_key_file for key-based auth).

2) Edit group_vars/all.yml:
   - Set ssh_pub_key to your control machine public key (single-line).
   - Set it_user to the username you want to create.
   - Optionally add or remove packages from it_extra_packages.

3) Dry-run then run on a single host
   Dry-run:
     ansible-playbook -i inventory.ini playbooks/it.yml --limit it1 --check --diff
   Real run:
     ansible-playbook -i inventory.ini playbooks/it.yml --limit it1 --ask-become-pass

Verification
- Check user exists:
  sudo id itadmin
- Check authorized_keys:
  sudo cat /home/itadmin/.ssh/authorized_keys
- Check docker:
  sudo docker version
- Check ufw:
  sudo ufw status
- Check fail2ban:
  sudo systemctl status fail2ban

Notes
- To enable docker-compose updates, update the get_url to the release you prefer.
- If you want passworded sudo for the it_user, add a /etc/sudoers.d snippet (use validate: 'visudo -cf %s' in the task).
- For production usage, store secrets in an Ansible Vault file.
```
