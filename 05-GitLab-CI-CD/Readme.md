# GitLab CI/CD
## Setting up GitLab Runner
### Download and run gitlab-runner docker container
```
docker run -d --name gitlab-runner --restart always \
          -v /srv/gitlab-runner/config:/etc/gitlab-runner \
          -v /var/run/docker.sock:/var/run/docker.sock \
          gitlab/gitlab-runner:latest
```

### Register gitlab-runner with gitlab
```
sudo docker exec -it gitlab-runner gitlab-runner register
```