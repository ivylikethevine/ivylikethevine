```bash
nas_name=maccy # user configurable
nas_path=/mnt/sandisk # user configurable

nas_hostname="$nas_name.local"
ssh_dir_path=/home/$USER/.ssh/
ssh_key_name=homelab
ssh_key_path="$ssh_dir_path$ssh_key_name"

sudo mkdir -p &&
  ssh-keygen -t rsa -C "homelab auth" -f $ssh_key_path &&
  eval "$(ssh-agent -s)" &&
  ssh-add ~/.ssh/homelab && 
  ssh-copy-id -i $ssh_key_path $USER@$nas_hostname &&
  crontab -l > cron-nas-backup &&
  echo "0 0 * * * rsync --mkpath --delete -ravh /home/$USER/.octoprint/data/backup $USER@$nas_hostname:$nas_path/$hostname" >> cron-nas-backup &&
  crontab cron-nas-backup &&
  rm cron-nas-backup
```
