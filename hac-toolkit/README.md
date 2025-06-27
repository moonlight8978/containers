# Home Assistant Container Toolkit

A comprehensive Docker container toolkit designed for Home Assistant automation and management tasks. This container includes essential tools for configuration management, encryption, templating, and notifications.

## Included Software

This container includes the following software:

- **sops** - Mozilla SOPS (Secrets OPerationS) for encrypting and decrypting configuration files
- **age** - Simple, secure file encryption tool with key generation capabilities
- **shoutrrr** - Notification library for sending messages to various services
- **gomplate** - Template processor for configuration files
- **yq**, **jq** - Data manipulation tool
- **ripgrep** - Fast text search tool
- **duplicity** - Encrypted backup tool
- **curl** - Command-line tool for transferring data
- **tar** - Archiving utility

## Usage

### Pull the Image

```bash
docker pull ghcr.io/moonlight8978/homeassistant-container-toolkit:latest
```

### Run Interactive Shell

```bash
docker run -it --rm --v ./bin:/usr/local/bin ghcr.io/moonlight8978/homeassistant-container-toolkit:latest sh
```

## Common Use Cases

- Use `sops` and `age` to manage secrets, merge multiple secrets to single one with `yq`

- Generating templates with `gomplate`

- Manage incremental backups with `duplicity`, ignore files with `rg`

- Sending notifications with `shoutrrr`

## Examples

```bash
# Generate key
age-keygen -o key.txt

# Decrypt secret
sops decrypt secrets.enc.yml > /tmp/secrets.yml

# Merge secrets with non-sensitive config
yq '. *= load("values.yml")' /tmp/secrets.yml > /tmp/values.yml

# Generate .env file
gomplate --datasource value=/tmp/values.yml --file .env.gotmpl --out .env

# Making backup archive
rg --files --no-ignore-parent --ignore-file=.ignore . | tar -vczf backup.tar.gz -T -

# Backup the file
duplicity \
  --allow-source-mismatch \
  --full-if-older-than $RETENTION \
  --no-encryption \
  --s3-region-name $S3_REGION \
  --s3-endpoint-url $S3_ENDPOINT_URL \
  --progress \
  backup \
  backup.tar.gz \
  s3://$S3_BUCKET/$S3_PREFIX
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.
