# SAM Installer

A set of tools to properly configure SAM with Foreman in production and development.

## Production Usage

```
$ yum install -y sam-installer

# install SAM with Foreman and smarty proxy with Puppet and Puppetca
$ sam-installer
```

### Production Examples

```
# Install also provisioning-related services

sam-installer --capsule-dns            "true"\
                  --capsule-dns-forwarders "8.8.8.8"
                  --capsule-dns-forwarders "8.8.4.4"\
                  --capsule-dns-interface  "virbr1"\
                  --capsule-dns-zone       "example.com"\
                  --capsule-dhcp           "true"\
                  --capsule-dhcp-interface "virbr1"\
                  --capsule-tftp           "true"\
                  --capsule-puppet         "true"\
                  --capsule-puppetca       "true"

# Install only DNS with smart proxy

sam-installer --capsule-dns            "true"\
                  --capsule-dns-forwarders "8.8.8.8"
                  --capsule-dns-forwarders "8.8.4.4"\
                  --capsule-dns-interface  "virbr1"\
                  --capsule-dns-zone       "example.com"\
                  --capsule-puppet         "false"\
                  --capsule-puppetca       "false"

# Generate certificates for installing capsule on another system
capsule-certs-generate --capsule-fqdn "mycapsule.example.com"\
                       --certs-tar    "~/mycapsule.example.com-certs.tar"

# Copy the ~/mycapsule.example.com-certs.tar to the capsule system
# register the system to SAM and run:
capsule-installer --parent-fqdn          "master.example.com"\
                  --register-in-foreman  "true"\
                  --foreman-oauth-key    "foreman_oauth_key"\
                  --foreman-oauth-secret "foreman_oauth_secret"\
                  --pulp-oauth-secret    "pulp_oauth_secret"\
                  --certs-tar            "/root/mycapsule.exampe.com-certs.tar"\
                  --puppet               "true"\
                  --puppetca             "true"\
                  --pulp                 "true"\
                  --dns                  "true"\
                  --dns-forwarders       "8.8.8.8"\
                  --dns-forwarders       "8.8.4.4"\
                  --dns-interface        "virbr1"\
                  --dns-zone             "example.com"\
                  --dhcp                 "true"\
                  --dhcp-interface       "virbr1"\
                  --tftp                 "true"\
```

## Data Reset

If you run into an error during the initial installation or wish to reset your installation entirely, the installer provides a reset option. To reset the database and all subsystems:

```
sam-installer --reset
```

## Updating Packages

This repository uses the gems librarian and librarian-puppet to handle the dependent
puppet modules. To update all the modules and automatically commit the result:

```
gem install librarian librarian-puppet puppet
rel-eng/librarian-update
```

`tito` is used for doing the releases.
