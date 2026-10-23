# App Service patch status

The status page is at https://adapptonazure.github.io/app-service-status/.

It shows when each built-in runtime stack on Azure App Service for Linux was first seen patched on each platform release channel, for the October 2026 php2shell, Brotli and eMudhra vulnerabilities.

## Updating the page

Edit `index.html` by hand. The comment at the top of the file explains the cell classes. Each push to `main` publishes `index.html` and `styles.css` through `.github/workflows/pages.yml`; nothing else in the repository is deployed.
