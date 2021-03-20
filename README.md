To configure repository to work on printers themselves:

1. Go to your Github account settings -> Developer settings -> Personal access tokens.
2. Generate a new token for that specific printer, with write access to repository.
3. Create a /home/pi/.netrc file
4. Add content to it:
```
  machine github.com
  login <your-github-login>
  password <token-you-just-generated>
```
5. If not done already, tell git who you are:
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

Done. Now you may push to the repo from the printer directly.

There are two files that are untracked because they are very printer-specific, printer.cfg-template and variables.cfg-template. Clone them into printer.cfg and variables.cfg and modify as needed.