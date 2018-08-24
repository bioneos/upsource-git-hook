# upsource-git-hook
This git post-receive hook will automatically create a new code review in Upsource. This hook is implemented in Ruby and uses:
* JSON
* Net/HTTP
* Upsource API

No external gems are required and this should work out of the box.

## What do I need for this to work?
You need to:
1. Place this `post-receive` hook in your `<repo>/hooks` directory (make sure it is executable and owned by the correct user/group!)
2. An Upsource installation with access to the API (Basic authorization is required)
3. Add some git config variables

### `post-receive` hook
Make sure the hook in this repo is placed in your `hooks` directory on your repo server. Again, make sure this script is executable. Note that this is a Ruby script, so you'll obviously need Ruby installed.

If you are like us and had multiple scripts you wanted to run on the `post-receive` hook (we notify Slack on a new `push`), the common suggestion is to create your `post-receive` script as a script that itself identifies the other scripts and executes them ([this](https://gist.github.com/mjackson/7e602a7aa357cfe37dadcc016710931b) is a good example)

### Upsource
The Upsource API is available by default, however to make requests you need to be authorized. The API uses Basic authorization by specifying the `Authorization` header. The token is to be your `login:password`, Base64 encoded.

### Git config variables
The following variables need to be configured for the hook to work properly (these can be set with `git config hooks.upsource.xxxx 'value'`:
* `url` - This should be the URL of your upsource installation
* `project-id` - The Upsource ID of the project you will be creating reviews on
* `reference` - The git reference that you want to monitor (e.g., `devel`, `master`, `releases`, etc)
* `auth` - Your `login:password` to be used for authenticating to Upsource, Base64 encoded
* `review-group` (optional) - If you would like to automatically have a group of users assigned to the review, this is the ID of the Group to assign. If you do not specify this, you will need to manually assign reviewers in Upsource

## TODO
This is a quick and dirty implementation that was whipped up to make creating reviews easier on our developers and to help maintain our review process. There are a few things that can (and hopefully will) be improved:
* Right now we can only monitor one git reference, ideally we could specify a wildcard and catch any updates
* Only a review group can be assigned automatically now, individual user assignments would be great
* I think Upsource now lets you use an API token for accessing the API, which would be **way** better than storing the Base64 encoded credentials
