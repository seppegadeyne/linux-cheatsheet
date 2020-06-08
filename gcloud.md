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

## Dynamic resources

### Scale out - instance template

#### Create and delete instance templates:
```
gcloud compute instance-templates create test-template
gcloud compute instance-templates delete test-template --quiet
gcloud compute instance-templates create test-template \
    --machine-type=n1-standard-1 \
    --image-project=ubuntu-os-cloud \
    --image-family=ubuntu-1804-lts \
    --tags=http-server,https-server \
    --metadata=startup-script="#!/bin/bash
        sudo apt-get update
        sudo apt-get --assume-yes install apache2"
```
Tags http-server and https-server will apply suitable firewall rules. 

#### Info from instance templates:
```
gcloud compute instance-templates list
gcloud compute instance-templates describe test-template
```

### Scale out - instance group

#### Create and delete instance groups:
```
gcloud compute instance-groups managed create test-group \
    --zone=europe-west1-b \
    --template=test-template \
    --size=3 
gcloud compute instance-groups managed delete test-group
```

#### Resize instance group:
```
gcloud compute instance-groups managed resize test-group --size=4
```

#### Get information from the instance group
```
gcloud compute instance-groups managed list
gcloud compute instance-groups managed describe test-group
```

### Scale out - autoscaling instance group
```
gcloud compute instance-groups managed set-autoscaling test-group \
    --max-num-replicas=10 \
    --min-num-replicas=1 \
    --cool-down-period=15
```

## Cloud SQL
### Machine types
```
gcloud sql tiers list
```

### Create or delete an instance 
```
gcloud sql instances --help
gcloud sql instances create test-sql \
    --tier=db-f1-micro \
    --region=europe-west1 \
    --authorized-networks=200.1.1.1/32 \
    --root-password=TMv4ABbB9a6uLsKL
gcloud sql instances delete test-sql
```

### Info
```
gcloud sql instances list
gcloud sql instances describe test-sql
```

### Allow VM access to SQL
```
gcloud sql instances patch test-sql2 \
    --authorized-networks=ip_address_vm/32
```

### Connect to SQL from VM
```
sudo apt-get update
sudo apt-get install mysql-client
mysql --host sql_ip_address --user=root --password=password
```

## Cloud storage
### Create or delete a bucket
```
gsutil mb gs://test-bucket
gsutil rb gs://test-bucket
```

### Copy and delete files / objects
```
gsutil cp file gs://test-bucket
gsutil rm gs://test-bucket/file
gsutil rsync gs://test-bucket .
```

 ### Show content
 ```
gsutil ls
gsutil ls gs://test-bucket
gsutil cat gs://test-bucket/file
```
