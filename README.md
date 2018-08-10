# upsource-git-hook
This git post-receive hook will automatically create a new code review in Upsource. This hook is implemented in Ruby and uses:
* JSON
* Net/HTTP
* Upsource API

No external gems are required and this should work out of the box.

# Why would I need this?
At [Bio::Neos, Inc](http://bioneos.com), we employ the [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/) strategy for our version control paradigm. We really like the workflow as it keeps our repositories clean and we love that linear history! A part of the Git Workflow is that you never push your feature branches, you merge them back into a branch (ours is typically `devel`). This plays a big reason with why we needed to make this hook...

Another key component of our tooling is [Upsource](https://www.jetbrains.com/upsource/) for efficient code reviews. Upsource is a really nice system for performing code reviews, but it only allows for code reviews on post-commit code. And moreso, it is really geared towards reviewing a branch as a whole. And here is the gotchya! for us, we never push our branches so we are always doing a code review on one branch. Now, we certainly could all go ahead and `push` our code, then login to Upsource and create a review with the appropriate commits and then assign reviewers. But lets be honest, we always forget. We tried to make this a part of our workflow but everyone forgot. So we thought we'd try having Upsource automatically make a review for us when code was pushed. There are two options for that:
1. Create a review for **every** commit that is pushed, which can result in a completely unmanageable number of review **or**
2. Append commits to an existing, open review. This works as long as the reviews are closed as quickly as possible, otherwise completely unrelated commits can appear in the same review.

So we were left with either forgetting to create a code review or having nonsense reviews. But then we thought, couldn't we use a Git hook? We would know the `reference` that was updated and all the commit hashes that were pushed. And Upsource has a great [API](https://upsource.jetbrains.com/~api_doc/index.html) available to use! So off we went, seeing if we could make these reviews easier on our developers...

# What do I need for this to work?
You need to:
1. Place this `post-receive` hook in your `<repo>/hooks` directory (make sure it is executable and owned by the correct user/group!)
2. An Upsource installation with access to the API (Basic authorization is required)
3. Add some git config variables

## `post-receive` hook
Make sure the hook in this repo is placed in your `hooks` directory on your repo server. Again, make sure this script is executable. Note that this is a Ruby script, so you'll obviously need Ruby installed.

If you are like us and had multiple scripts you wanted to run on the `post-receive` hook (we notify Slack on a new `push`), the common suggestion is to create your `post-receive` script as a script that itself identifies the other scripts and executes them ([this](https://gist.github.com/mjackson/7e602a7aa357cfe37dadcc016710931b) is a good example)

## Upsource
The Upsource API is available by default, however to make requests you need to be authorized. The API uses Basic authorization by specifying the `Authorization` header. The token is to be your `login:password`, Base64 encoded.

## Git config variables
TODO
