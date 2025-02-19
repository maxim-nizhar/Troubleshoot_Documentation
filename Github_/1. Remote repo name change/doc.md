# Trouble I was facing:
Repository Remote Change:
• I first changed the repository remote from "StartupHunt" to "CoFoundry" on GitHub.
• This involved updating the remote configuration so that the new repository URL (git@github.com:maxim-nizhar/CoFoundry.git) would be used for fetching and pushing commits.

Commit Operation Appeared Inactive:
• After updating the remote, I attempted to commit changes locally using a command like:
git commit -m "some message"
• However, nothing appeared to happen and no updates were visible on the GitHub repository. This is because local commits alone do not update the remote repository—they only record changes in the local repository.

Pushing Reveals SSH Permission Error:
• When trying to push the commits with:
git push -u origin main
• I encountered the error:
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
• This error confirmed that the push attempt was being blocked due to SSH authentication issues.

SSH Key Troubleshooting:
• I inspected my SSH directory with:
ls -al ~/.ssh
• The listing did not show any public key files (e.g., id_rsa.pub or id_ed25519.pub), indicating that my system did not have an SSH key set up for GitHub authentication.
• Without a proper public key available in GitHub, the connection using the git@github.com URL was refused.


# Resolution

Resolution documentation:

Verified the existing remote URL using:
• Command: git remote -v

• Output:
origin  https://github.com/maxim-nizhar/StartupHunt.git (fetch)

origin  https://github.com/maxim-nizhar/StartupHunt.git (push)

Changed the repository remote from "StartupHunt" to "CoFoundry" by executing:
• Command: git remote set-url origin git@github.com:maxim-nizhar/CoFoundry.git

Confirmed the remote update with:
• Command: git remote -v

• Output:
origin  git@github.com:maxim-nizhar/CoFoundry.git (fetch)

origin  git@github.com:maxim-nizhar/CoFoundry.git (push)


Subsequent troubleshooting steps were as follows:

Check for an existing SSH key:
• Executed: ls -al ~/.ssh

• Observation: No public key (e.g., id_rsa.pub or id_ed25519.pub) was present.

Generate a new SSH key:
• Command: ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

• Accepted the default file location and optionally set a passphrase.

Start the ssh-agent and add the newly generated key:
• Command: eval "$(ssh-agent -s)"
• Then: ssh-add ~/.ssh/id_rsa

Copy the public key to the clipboard:
• Command: pbcopy < ~/.ssh/id_rsa.pub

Add the copied public key to your GitHub account:
• Logged in to GitHub, navigated to “Settings” → “SSH and GPG keys,” clicked “New SSH key,” and pasted the key.

Test the SSH connection:
• Command: ssh -T git@github.com

• Verified that GitHub accepted the connection by displaying a welcome message.

Finally, pushed the local commits:
• Command: git push -u origin main

These steps resolved the SSH authentication issue and enabled successful pushing to the new CoFoundry repository.
