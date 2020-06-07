# Google Cloud
```
gcloud init
gcloud config list
gcloud --help
gcloud compute --help
``` 

## VM data center
### Zone
- Data center, ex. europe-west1-b
```
gcloud compute zones list
```

### Region
- Collection of zones, ex. europe-west1
```
gcloud compute regions list
```

### Type
- Hardware configuration (cores, memory)
```
gcloud compute machine-types list
```

### Image
- Software image of the VM (OS, application)
```
gcloud compute images list
```

## VM instance
### Create, delete, stop and start
```
gcloud compute instances create test-vm
gcloud compute instances stop test-vm
gcloud compute instances start test-vm
gcloud compute instances delete test-vm
gcloud compute instances create test-vm \
    --machine-type=n1-standard-1 \
    --zone=europe-west1-b \
    --image-project=ubuntu-os-cloud \
    --image-family=ubuntu-1804-lts \
    --tags=chat
```

### Info
```
gcloud compute instances list
gcloud compute instances describe test-vm
```

### Login
```
gcloud compute ssh test-vm
```

## VM startup scripts
After creating a VM startup scripts can be executed.

### Create instance with local startup script
```
gcloud compute instances create test-vm --metadata-from-file=startup-script=startup.sh
```

### Online startup script
```
gcloud compute instances create test-vm --metadata=startup-script-url="url"
```

### Inline startup script
```bash
gcloud compute instances create rocketchat \
  --machine-type=n1-standard-1 \
  --zone=europe-west1-b \
  --image-project=ubuntu-os-cloud \
  --image-family=ubuntu-1804-lts \
  --tags=chat \
  --metadata=startup-script="#!/bin/bash
    apt-get update
    snap install rocketchat-server"
```

## Networking
### VM default IP setup

- One intern private address
- One ephemeral external address (dynamic)

### External IP

- Needs to be registered separately (paid)

## Firewall
Internal private networks are connected to the internet with a firewall.

### Firewal rules
```
gcloud compute firewall-rules list
gcloud compute firewall-rules describe default-allow-internal
gcloud compute firewall-rules create http8080 --allow=tcp:8080 --target-tags=VM_tag
gcloud compute firewall-rules delete http8080
```
Rules with a target tag apply only to VM instances with the same tag.




