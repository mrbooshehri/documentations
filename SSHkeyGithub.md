# Connect to github via SSH key
1. Generating a new SSH key
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
2. Adding your SSH key to the ssh-agent
```bash
eval "$(ssh-agent -s)"
> Agent pid 59566
ssh-add ~/.ssh/id_ed25519
```
3. Add the SSH key to your account on GitHub [How](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

