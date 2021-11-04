---
slug: my-first-github-action
title: My First Github Action
date: 2021-11-03
---

Recently I wrote a simple nodejs script to parse YAML front matter in Markdown files, and convert them into JSON. In the spirit of automation, I wanted to find a way to automate running the script.

I have used GitHub Actions a lot, but have never written my own, so I thought this might be a good time to learn. Luckily for me, [GitHub has great docs](https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action) so I didn't really run into many issues.

First, I just took my script and instead of writing to a file, just print out the results. I tried to see if it would run as an action. It did, but it couldn't find the directory I was pointing to. I had to use the `actions/checkout@v2` in a step before to get access to the files in my repo. I ran it again, this time it worked.

I changed the script to take input from the action, ran it again, and it worked.

I then tried to write to a file instead of printing the result. No luck, I couldn't figure it out, by this [GitHub Communtiy Post](https://github.community/t/can-github-actions-directly-edit-files-in-a-repository/17884/7) helped me figure it out. Instead of writing to the file system with node, I had to either use another action or run use git commands in my workflow. I chose the latter.

Before I could do that, instead of returning the result from my script, I had to send it to the action output. A bit of fiddling with shell commands later, It worked! This is the step I added:

```yaml
run: |
  echo '${{ steps.demo1.outputs.output }}' > 'res.json'
  git config user.email "actions@github.com"
  git config user.name "hehe"
  git add --all
  git commit -m "Update json" || echo "No changes to commit"
  git push
```

I did some additional configuration to add features to the action, so it was more generic and not only useful for my needs, and added a demo to the repo. I noticed at the top it was prompting me to publish the action so I did!

However I realised my documentation was awful, and it would probably be really hard to use it even if you wanted to, so I spent some time looking at other actions documentation and wrote my own. I also kept flip flopping between 'Front Matter to JSON' and 'Markdown Metadata to JSON', I decided on 'Front Matter to JSON' becauce you can have yaml front matter in files other than markdown, so I thought it'd be more general.

Please check out the action [here!](https://github.com/marketplace/actions/front-matter-to-json-action). I'm real keen to improve it so please do open issues, create PRs :smile:.
