# Ansible Role: `aisbergg.acl`

This Ansible role is a wrapper around the [`acl`](https://docs.ansible.com/ansible/latest/modules/acl_module.html) module.

## Requirements

None.

## Role Variables

| Variable | Default | Comments |
|----------|---------|----------|
| `acl_acls` | `[]` | List of ACL to be set. See the official docs for the [ACL module](https://docs.ansible.com/ansible/latest/modules/acl_module.html) for a list of available parameters.<br>**Note**: `state` defaults to `present` in this role. |
| `acl_acls[x].create_dir` | `false` | If true, the specified path will be created as a directory. |
| `acl_acls[x].owner` |  | Owner of the directory. (Only when `create_dir` is true) |
| `acl_acls[x].group` |  | Group of the directory. (Only when `create_dir` is true) |
| `acl_acls[x].mode` |  | Mode of the directory. (Only when `create_dir` is true) |
| `acl_apply_defaults` | `true` | When set to `true`, any default ACL entry will also be applied as a regular entry (== `setfacl -d -m u:foo:rwX && setfacl -m u:foo:rwX`). |

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  vars:
    acl_apply_defaults: true
    acl_acls:
      # grant user 'joe' read access to a file
      - path: /etc/foo.conf
        entity: joe
        etype: user
        permissions: r

      # set default and regular ACL for group 'bar'
      - path: /var/www/demo
        entity: bar
        etype: group
        permissions: rwX
        default: true
        # ensure directory exists
        create_dir: true
        owner: bar
  roles:
    - aisbergg.acl
```

## License

MIT

## Author Information

Andre Lehmann (aisberg@posteo.de)
