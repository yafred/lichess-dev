# Remote SSH

## Create a key pair on the client machine (in {HOME}/.ssh folder)
```
ssh-keygen -t rsa -b 2048
```

It generates 2 files: {identity} and {identity}.pub

## Create or append vscode config {HOME}/.ssh/config
```
Host {remote server}
  HostName {remote server}
  User {userid}
  PasswordAuthentication no
  IdentityFile {path to the {identity} file created above}
```


## Authorize ssh connection on the server

On the server, create or append {HOME}/.ssh/authorized_keys with the content of {identity}.pub
