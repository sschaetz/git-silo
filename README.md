## git-silo
*josh-based git proxy for siloed checkouts to allow limited access (silo) to a git repository*

**⚠️⚠️ you probably should not use this (see caveats below) ⚠️⚠️**

Uses [josh](https://github.com/josh-project/josh) and
[nginx](https://www.nginx.com/) to allow partial (siloed) checkouts of git
repositories.

### What

josh allows workspace-based cloning and pushing (via HTTP) of repositories but
does not allow for access control. We add rudimentary access control using
nginx reverse proxy that:

- adds basic credentials to the requests to the git HTTP endpoint
- rewrites URLs to force checkouts through a josh workspace and exlude
  the workspace file itself

### Instructions

1. Follow josh
   [instructions](https://josh-project.github.io/josh/guide/workspaces.html)
   to create a workspace (not using this setup)
2. Update docker-compose and nginx conf file to your use-case:
   - we assume this is running in `/home/ubuntu/git-silo`
   - set `<ORG>` to our org/username
   - add `<USER:PASSWORD>` (base64 encoded) to the nginx conf
   - update the `<REPO-NAME>` in the re-write rule
   - update the `<WORKSPACE-NAME>` in the re-write rule
3. `docker-compose up`

### Caveats
- we put credentials in the nginx config file, that's not great
- anybody with access to this server will be able to interact with git
  (pull/push) so this needs additional guardrails
