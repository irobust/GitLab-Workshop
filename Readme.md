# Software Version Control Workshop
## Modules
1. Git Config and Git Attributes
1. Git Basic Command
1. Git Sub Modules
1. Git Branch and Merge

## Gitlab Installation
```
docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab:Z \
  --volume $GITLAB_HOME/logs:/var/log/gitlab:Z \
  --volume $GITLAB_HOME/data:/var/opt/gitlab:Z \
  --shm-size 256m \
  gitlab/gitlab-ee:latest
```

### Checking status
```
docker logs -f gitlab
```

### Gathering initial root password
```
sudo docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```