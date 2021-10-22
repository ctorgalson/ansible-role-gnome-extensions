# Ansible Role Gnome Config

An Ansible role for idempotently managing Gnome desktop configuration.

For the moment, only supports downloading, enabling, disabling, and removing
Gnome extensions that are available from https://extensions.gnome.org, as well
as enabling and disabling Gnome extensions that have been installed via Apt.

Future plans include adding support for downloading via git and the management
of other Gnome settings and configuration.

## Requirements

For obvious reasons, this role requires a host with a Gnome desktop environment.

In addition, the role currently uses Debian/Ubuntu-specific tools.

## Role Variables

### Vars

| Variable name | Required | Default value | Description |
|---------------|----------|---------------|-------------|
| `gnome_extensions_url` | yes | `https://extensions.gnome.org` | The base of the url used to download Gnome extensions. |
| `gnome_packages` | yes | `[{name: 'gnome-shell', state: 'present'}, {'gnome-shell-extensions', 'present'}]` | The packages required on the target host for the role to function in the first place. |

### Defaults: extension-related

| Variable name | Required | Default value | Description |
|---------------|----------|---------------|-------------|
| `gnome_extensions` | yes | `[]` | Describes the url and desired existence/enabled state for each Gnome extension (see note below for details). |

#### `gnome_extensions` variable

This variable has three required properties:

| Property name | Required | Type | Accepted values |
|---------------|----------|------|-----------------|
| `url`         | yes      | string  | A url suitable for [ansible.builtin.get_url](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/get_url_module.html) |
| `state`       | yes      | string  | `present` or `absent` |
| `enabled`     | yes      | boolean | `True`, `False`, or [anything ansible regards as 'truthy'](https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html#conditionals-based-on-variables) |

- url: the url of the extension on extensions.gnome.org, e.g. `https://extensions.gnome.org/extension/750/openweather`,
- state: whether or not the extension should be present in the file system, e.g. `present` or `absent`,
- enabled: whether or not to enable the extension, e.g. `true` or `false`

## Dependencies

The role has no special dependencies. 

## Example Playbook

The following playbook:

  - downloads the [Open Weather](https://extensions.gnome.org/extension/750/openweather/) and [Put Windows](https://extensions.gnome.org/extension/750/openweather/) extensions and ensures they are
    installed,
  - removes the [Gnome Screenshot Tool](https://extensions.gnome.org/extension/1112/screenshot-tool/) extension after first disabling it,

```yaml
- name: Manage Gnome extensions.
  hosts: servers
  vars:
    gnome_user: "instance"
    gnome_extensions:
    gnome_user: "instance"
    gnome_extensions:
      - url: "https://extensions.gnome.org/extension/750/openweather/"
        state: "present"
        installed: true
      - url: "https://extensions.gnyi"ome.org/extension/39/put-windows/"
        state: "present"
        installed: true
      - url: "https://extensions.gnome.org/extension/1112/screenshot-tool/"
        state: "absent"
        installed: false
  tasks:
    - name: "Include ansible-role-gnome-config"
      include_role:
        name: "ansible-role-gnome-config"
```

## Testing

The role is fully tested in Molecule on Docker and verified with Ansible with
two exceptions: enabling and disabling Gnome extensions.

If anybody knows how to get enough of Gnome reproducibly running in Docker so
that I can enable these two things to be tested, please let me know!

## License

GPL-3.0-only

## Acknowledgments

Big thanks to @PeterMosmans et al whose handy [Ansible Role: customize-gnome](https://github.com/PeterMosmans/ansible-role-customize-gnome) inspired this one.
