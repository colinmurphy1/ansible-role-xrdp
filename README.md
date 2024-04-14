# Ansible xrdp role

An Ansible role that installs and configures [Xrdp](https://www.xrdp.org/) on a Linux server. This role defaults to the Xorgxvnc backend, and also supports features such as sound using the Pipewire module. This role has been tested and works on Debian, with support for other distros planned for a future release.

## Role variables 

*For a complete list of variables, see defaults/main.yml*

* `xrdp-service`: Service settings
  * `state`: xrdp service state
  * `enabled`: if true, xrdp will start on boot
* `xrdp_port`: the port xrdp listens on, defaults to 3389
* `xrdp_certificate`: path to a certificate to use, optional
* `xrdp_certificate_key`: path to a certificate key, optional
* `xrdp_ssl_protocols`: SSL protocols to use, defaults to TLS1.2 and TLS1.3
* `xrdp_buffers`: send and receive buffer size, a larger value can improve performance on high resolution sessions and sessions with audio.
  * `send`: send buffer size
  * `recv`: receive buffer size 
* `xrdp_login_title`: login window title, defaults to hostname
* `xrdp_fix_scrolling`: if set to true, a [touchpad scrolling hack](https://github.com/neutrinolabs/xorgxrdp/issues/150) is enabled. 
* `xrdp_audio`: enable the pipewire audio plugin
* `xrdp_security`: Security settings
  * `permit_root`: allow root login
  * `max_login_retry`: maximum failed logon attempts
  * `group_check`: if true, users must be in `tsusers_group` to log in
  * `tsusers_group`: group allowed to log in, defaults to `tsusers`
* `xrdp_session`: session related settings
  * `kill_disconnected`: kill disconnected sessions
  * `max_sessions`: maximum allowed sessions per host
  * `disconnected_time_limit`: if set to a non-zero value, the time before killing disconnected sessions
  * `idle_time_limit`: how long a session may remain idle before being timed out

## Example playbook

```yaml
- host: desktop1
  become: yes
  roles:
    - cmurphy.xrdp
  vars:
    xrdp_certificate: /etc/certs/mycert.pem
    xrdp_certificate_key: /etc/certs/acme/mycert-key.pem
    xrdp_buffers:
      send: 8388608
      recv: 8388608
    xrdp_audio: true
```

## License 

MIT

## Author

[Colin Murphy](https://colinmurphy.me)
