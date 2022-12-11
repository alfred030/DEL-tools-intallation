## To install or update eksctl
Determine whether you already have eksctl installed on your device.

```
eksctl version

```

Download and extract the latest release of eksctl with the following command.

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

```

Move the extracted binary to /usr/local/bin.

```
sudo mv /tmp/eksctl /usr/local/bin

```

Test that your installation was successful with the following command.

```
eksctl version

```
