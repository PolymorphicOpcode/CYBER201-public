## Create users Systematically (bash script)
```bash
#!/bin/bash
while read -r line; do
    password=$(echo "$line" | cut -d ' ' -f 1 | cut -c 3-)  # extract the password
    username=$(echo "$line" | cut -d ' ' -f 2 | tr '[:upper:]' '[:lower:]')  # extract the username
    useradd -m -p "$password" "$username"  # create the user with the given password
done < "$1"
```

## Systematically change passes
```bash
#!/bin/bash
while read -r line; do
    password=$(echo "$line" | cut -d ' ' -f 1 | cut -c 3-)  # extract the password
    username=$(echo "$line" | cut -d ' ' -f 2 | tr '[:upper:]' '[:lower:]')  # extract the username
    echo "$username:$password" | sudo chpasswd
done < "$1"
```